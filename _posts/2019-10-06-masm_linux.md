---
layout: article
title: Using MASM with the Irvine Library on GNU/Linux 
categories: [Tech]
author: Ahmed Elmayyah
tags: [Tech, Linux, Assembly, Programming, Low Level, Guide]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: /assets/images/masm-linux/masm_header.jpg
cover: /assets/images/masm-linux/masm_cover.jpg
---

In this post I’ll be talking about how to run MASM alongside Irvine library on your Linux system. MASM and Irvine library are both requirements for the assembly language course at my university, and well, I am not a fan of Windows so here they are on Linux.

<!--more-->

Let’s just get familiar with the software that we will need:

- **MASM**: Microsoft Macro Assembler, an x86 assembler for Windows systems.
- **WINE**: WINE is Not an Emulator, a compatibility layer that runs Windows software on macOS and *nix systems.
- **PlayOnLinux**: is a software that helps you manage WINE programs and makes WINE easier to use.
    - There’s also [PlayOnMac](https://www.playonmac.com/en/download.html) which does the same thing but for macOS.
- **SASM**: Simple ASM is a cross-platform IDE for assembly.
- **Irvine Library**: A library that adds some useful assembly functions, it is the accompanying library for the great book: [Assembly Language for x86 Processors](http://asmirvine.com/).

Worth mentioning:

- Visual Studio 2005 Express seems to work using PlayOnLinux so you could use it instead of SASM if you found SASM not feature-rich enough.
- [Along32](https://github.com/janka102/Along32) is a library that rewrites some of Irvine Library’s code, but I couldn’t get it to work for a couple of reasons so I just couldn’t be bothered.
- I stumbled upon the [masm-unix GitHub repo](https://github.com/tuxxi/masm-unix) 2 months after originally publishing this article, I haven’t tested it but it may also be worth looking it.

We won’t be using these but you could try and tell me the results.

Note: You can try following the same steps on macOS as WINE is supported on it too. I haven’t tested it though so I have no idea if it’ll work or not.

# Downloading the Software

The first step, of course, is downloading the software we need.

Download MASM from [here](http://website.assemblercode.com/masm32/masm32v11r.zip), DO NOT get it from Microsoft’s site as it won’t work.

- [http://website.assemblercode.com/masm32/masm32v11r.zip](http://website.assemblercode.com/masm32/masm32v11r.zip)

Download SASM from [here](https://dman95.github.io/SASM/english.html), make sure to download the Windows version as the Linux version doesn’t support MASM due to Microsoft’s EULA.

- [https://dman95.github.io/SASM/english.html](https://dman95.github.io/SASM/english.html)

Download Irvine library from [here](http://www.kipirvine.com/asm/examples/Irvine_7th_Edition.msi).

- [http://www.kipirvine.com/asm/examples/Irvine_7th_Edition.msi](http://www.kipirvine.com/asm/examples/Irvine_7th_Edition.msi)

## Downloading WINE and PlayOnLinux

They’re pretty easy to install on most Linux distributions.

You need to download the following packages, check what they’re called on your distro:

- wine
- winetricks
- playonlinux

Optionally, you may also need to download these:

- wine-mono
- wine_gecko

If you’re using Ubuntu/Linux Mint/elementaryOS, you can check out these guides to install PlayOnLinux:

- [https://help.ubuntu.com/community/PlayOnLinux](https://help.ubuntu.com/community/PlayOnLinux)
- [http://wiki.playonlinux.com/index.php/Installing_PlayOnLinux](http://wiki.playonlinux.com/index.php/Installing_PlayOnLinux)
- [https://www.sysnettechsolutions.com/en/linux/install-playonlinux](https://www.sysnettechsolutions.com/en/linux/install-playonlinux)

Also check this to install WINE which is required for PlayOnLinux to work:

- [https://www.pcsuggest.com/install-wine-ubuntu](https://www.pcsuggest.com/install-wine-ubuntu/)
- [https://vitux.com/how-to-install-wine-on-ubuntu](https://vitux.com/how-to-install-wine-on-ubuntu/) 

If you use Arch Linux/Manjaro or any Arch-based distro, you can install everything you need just by running this command: `# pacman -S wine winetricks wine-mono wine_gecko playonlinux`

# Installing the Software

## Making a Virtual Drive and Installing MASM

Now that we have WINE and PlayOnLinux installed, we can now install MASM, SASM, and Irvine Library.

Just a note on how PlayOnlinux works, it basically has virtual drives which act as Windows installations where you can install multiple programs and libraries on the same virtual drive and they’d all work as if they were on the same C:\ drive on a real Windows installation (most of the time).

Run PlayOnLinux and click on the plus icon that says “Install” then choose “Install a non-listed program” at the bottom left.

{: style="text-align:center"}
![install non listed](/assets/images/masm-linux/installnonlisted.png)


Read these instructions, they’re useful if you’re going to be using PlayOnLinux for other software later on.

{: style="text-align:center"}
![warnings](/assets/images/masm-linux/warnings.png)

After clicking next a couple of times, you should reach this screen. Choose “Install a program in a new virtual drive”.

{: style="text-align:center"}
![install-1](/assets/images/masm-linux/install-1.png)

Click on “Next” and then choose a name for your virtual drive. I set this to “MASM”.

Keep clicking on “Next” until you reach this screen and choose “64 bits windows installation”.


{: style="text-align:center"}
![64bit](/assets/images/masm-linux/64bit.png)


At this point, especially if you’re using Ubuntu or any of its derivatives, PlayOnLinux may ask you to install wine_gecko and wine-mono automatically. In this case, let it install it. It will take some time to download and the size will vary depending on your distribution and your version of WINE.

After that, you’ll reach this screen. The first thing you want to install is MASM. Click on “Browse” and browse to the directory where the installer is and install it like you would install a Windows program. If it asks you which partition to install MASM32 on, choose C:\\ and **NOT** Z:\\.

{: style="text-align:center"}
![browse](/assets/images/masm-linux/browse.png)


{: style="text-align:center"}
![masminstall](/assets/images/masm-linux/masminstall.png)

{: style="text-align:center"}
![extract](/assets/images/masm-linux/extract.png)

After you run through the prompt screens, MASM has probably been installed but PlayOnLinux fails to detect as it is still running some tests. We can click on “Cancel” in the PlayOnLinux prompt window that says “PlayOnLinux is installing your application…” after a minute or two.

## Installing SASM and Irvine Library

Click on the gear icon that says “Configure” in the main PlayOnLinux interface.

Choose the virtual drive that you just created and click on “Miscellaneous”. After that, choose to run a .exe in the virtual drive.

It’ll prompt you to choose a file, just choose the files for SASM and Irvine library and install them using their default settings one after another.

Now we have everything installed!

# Configuring SASM

In the same “Configure” window that you had open, go to “General” and choose to make a shortcut from the drive.

Scroll down until you find sasm.exe and choose it. After that, click on “I don’t want to make another shortcut” and then “Next”.

{: style="text-align:center"}
![sasm](/assets/images/masm-linux/sasm.png)

Now SASM will appear in PlayOnLinux and on your desktop! Run it.

Open the settings:

In “Build”, set the assembler to “MASM”.

In “Common”, set the font you like and its size and set the default code editor text to the following. As for the font, I found Liberation Mono with size 15 to work best for me.

```nasm
INCLUDE C:\Irvine\Irvine32.inc
INCLUDELIB C:\Irvine\Irvine32.lib
INCLUDELIB C:\Irvine\User32.lib
INCLUDELIB C:\Irvine\Kernel32.lib
.data
; Write Your Variables Here
.code
main PROC
; Write Your Code Here ...
call WaitMsg
exit
main ENDP
END main
```
Now whenever you want to run your program, click on “Build” and then “Run in a new window”.

{: style="text-align:center"}
![run](/assets/images/masm-linux/run.png)

And voila!

{: style="text-align:center"}
![running](/assets/images/masm-linux/running.png)

# Making Things a Little Prettier

Depending on your system, SASM may look very ugly and the fonts will look very bad when running under WINE.

We can change that a little bit.

In PlayOnLinux, go to the configure window, under “Wine” and go to “Configure Wine”.

Go to “Graphics” and increase the DPI a little bit. It’ll differ from system to system. My laptop worked well with 130. On my desktop, 96 worked without any problems.

{: style="text-align:center"}
![dpi](/assets/images/masm-linux/dpi.png)

After that go to “Desktop Integration”

You can install .msstyles themes and apply them. This worked on my laptop but for some reason on my desktop, the application would crash if I had any theme applied.

{: style="text-align:center"}
![xp](/assets/images/masm-linux/xp.png)


You may also notice that if the output in the console gets too big you lose the lines at the top. To make the console scrollable and give it a larger buffer size do the following:

Run any sample assembly program, and right click the console and click on “Properties”.

{: style="text-align:center"}
![properties](/assets/images/masm-linux/properties-1.png)

Set the height of the buffer zone in “Configuration” to 1000 or any large number.

{: style="text-align:center"}
![buff](/assets/images/masm-linux/buff-1.png)


# Bonus: Installing Visual Prolog

Another course I am taking this semester is logic programming which requires “Visual Prolog”.

This has nothing to do with assembly, but it’s just worth mentioning that you can run the software with WINE without any trouble. Just install it the same way, either by creating a new virtual drive or installing it on the same virtual drive as MASM.

{: style="text-align:center"}
![prolog](/assets/images/masm-linux/prolog-2.png)

Thanks a lot for reading. I hope this guide was useful to you!

This guide is based on this [guide](https://github.com/Open-Source-Community/Third-Grade-Materials/blob/master/First%20Term/Assembly/masm.pdf) by [OSC – Open Source Community](https://www.facebook.com/oscgeeks/).
