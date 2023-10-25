---
layout: article
title: "Back to Basics: Why do we return 0?" 
categories: Tech
author: Ahmed Elmayyah
tags: [Tech, Linux, C, Programming]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: //assets/images/exit-codes/exitcodes_header.jpg
cover: //assets/images/exit-codes/exitcodes_cover.jpg
---

There is a very high chance that if you read my blog, you've seen the line of code `return 0;` at some point. Specifically, you've probably seen it in a `main()` function, like so:

```c
int main()
{
	//Do stuff
	return 0;
}
```

Why, though? Can I return something that isn't zero?

The simple answer is yes. `main()` is, after all, a function and can return whatever you want it to. Not just that, but programs in general can have _any_ exit code.

# Exit Codes

We have discussed before that when you run a program, the OS loads it as a _process_. That process executes instructions until it exits. However, before it exits, it sets an exit code(value) for the OS to tell it why or how it exited. Returning from the `main()` function makes the process terminate and sets the exit code to `main()`'s return value.

{: style="text-align:center"}
![](/assets/images/exit-codes/return5.png)

On Linux, you can check the exit code of a process by using the shell variable `?`. On Windows, you can do the same by checking the variable `ERRORLEVEL`. e.g. `echo %ERRORLEVEL%`.
{:.info}

What if I want to exit the program from a function that isn't `main()`?

# Exit System Call
You use an exit syscall. This differs between languages and OSs. However, Let's look at Linux for an example. 

On Linux, with the standard C library, you can call [`void exit(int status)`](https://en.cppreference.com/w/c/program/exit). This will exit the process and set the exit code to `status` which is the parameter given to it.

It is worth nothing that returning from the main function does eventually lead to an implicitly-called `exit()` as well.
{:.info}

## Example

```c
#include <stdlib.h>
void exitWithError()
{
	exit(128);
}
int main()
{
	exitWithError();
	return 0;
}
```

{: style="text-align:center"}
![](/assets/images/exit-codes/exit128.png)

# What does this mean?
Well, it is just a way of showing why or how a process terminated. Conventionally, `0` means "All good!". The process executed fine and exited due to a normal reason (i.e. not a crash or error)<sup>[1]</sup>. Any other exit code would indicate some form of message regarding the process termination to the OS or user. 

<sup>[1]</sup> - That is if the developer programmed it _correctly_. 

On some modern operating systems(at least Linux), you'll find exit codes to be ranging from `0` to `255`(an 8-bit integer value).
{:.info}

## Example Use Case

If you've ever used a Debian-based Linux distro before, you may have seen someone tell you to run the command line: `sudo apt update && sudo apt upgrade`. Have you ever asked yourself what `&&` does?

### &&

`&&` ensures that the previous "statement" (in this case, the execution of `apt update`) is true (i.e. exit status is `0`). There are two other useful Linux commands we can use to demonstrate this, which are `true` and `false` which return `0` and `1` respectively.

{: style="text-align:center"}
![](/assets/images/exit-codes/truefalse.png)

You can use this to only proceed if the previous command executed successfully. In the `apt` example, it wouldn't upgrade the packages if there was an issue updating the package infromation.

### ||

Even more interesting is `&&`'s evil sibling: `||`, which will proceed only if the previous command returned anything other than `0` (i.e. had a non-zero exit code).

{: style="text-align:center"}
![](/assets/images/exit-codes/oror.png)

### ||, &&, and Exit Codes

Ultimately, we can use both in combination to do different things based on the success (or failure) of the process. We can use the following program to demonstrate that:

```c
#include <stdio.h>
int main()
{
    char c = getchar();
    if (c == 'A')
        return 0;
    else
        return 255;
}
```

{: style="text-align:center"}
![](/assets/images/exit-codes/example.png)

You may or may not have asked yourself before why we return `0`. Whether you did or not, I hope you found this post useful, fun to read, or both!

# Bonus
But, wait! There's a bonus :) Remember when I said that `main()` is just a function and can return _whatever_ you want? Can we make `main()` return `main()`? Pretty sure nothing can go wrong with that...

```c
#include <stdio.h>

int main()
{
    printf("Bad Recursion, Brb...\n");
    return main();
}
```


{: style="text-align:center"}
![](/assets/images/exit-codes/brb.gif)

If you liked this post, please share it with your friends who like returning `-1` instead of `255`.

As always, thanks for reading.

