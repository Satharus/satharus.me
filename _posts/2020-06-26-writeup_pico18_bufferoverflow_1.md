---
layout: article
title: "Buffer Overflows: PicoCTF2018-BufferOverflow-1" 
categories: [Cybersecurity]
author: Ahmed Elmayyah
tags: [Cybersecurity, C, Exploitation, Writeup, Buffer Overflows]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: /assets/images/writeup-pico18-bufferoverflow-1/pico18_bof1_header.jpg
cover: /assets/images/writeup-pico18-bufferoverflow-1/pico18_bof1_cover.jpg
---
>I wrote this post back in late May 2019 but hadn’t published it and I don’t really remember why I didn’t. It was supposed to be a follow up for the post on the [buffer overflow basics](https://satharus.me/cybersecurity/2019/05/22/buffer_overflows_the_basics.html). 
>
> Anyway since I’ve already written it, here it is.
<!--more-->

For those that don’t know, [PicoCTF](https://picoctf.com/) is a free cybersecurity CTF game targeted at middle and high school students. It has some really nice challenges that get harder as you progress, starting out with very easy ones. I always recommend it as a good starting point for CTFs. This specific challenge is a good starting point to start learning about buffer overflows and how to exploit them.

If you don’t know what CTFs are, check out my article: [The Art of Cybersecurity (And how to get into it)](https://satharus.me/cybersecurity/2019/12/02/the_art_of_cybersecurity.html) for some starter info.

> “Okay now you’re cooking! This time can you overflow the buffer and return to the flag function in this [program](https://2018shell.picoctf.com/static/d6146450a41960f6ce43dbfb300d9ef4/vuln)? You can find it in /problems/buffer-overflow-1_0_787812af44ed1f8151c893455eb1a613 on the shell server.” [Source](https://2018shell.picoctf.com/static/d6146450a41960f6ce43dbfb300d9ef4/vuln.c). “

This is the description of the challenge. Since we have the source code available, it is easier to start there and then resort to disassembling the program or debugging it if necessary. By inspecting the source code of the program, we find that the input is taken via a buffer of size 32.


```c
#define BUFSIZE 32
#define FLAGSIZE 64
```

```c
void vuln(){
  char buf[BUFSIZE];
  gets(buf);

  printf("Okay, time to return... Fingers Crossed... Jumping to 0x%x\n", get_return_address());
}
```
Enter any string and it’ll say “Okay, time to return… Fingers Crossed… Jumping to 0x80486b3”.

Note: 0x80486b3 was the address in my case. It’ll probably be different if you try to solve the challenge.

This means that it returns to that address in the program's memory. If we change that value, we can make the program return to any address by writing over it using the input.

This can be achieved using a buffer overflow exploit.

I tried entering 32 ‘a’ chars and I didn’t see any change in the address, so I kept increasing them until I found that the address starts changing after entering 44 characters.

The explanation for this is that the saved instruction pointer (before the call to `vuln()`) is located 12 bytes after the last byte in the buffer.

Checking the assembly code using ObjDump, the function we want to run: `win()`, is found at address 080485cb. Which is exactly where we want to return.

```c
void win() {
  char buf[FLAGSIZE];
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("Flag File is Missing. Problem is Misconfigured, please contact an Admin if you are running this on the shell server.\n");
    exit(0);
  }

  fgets(buf,FLAGSIZE,f);
  printf(buf);
}
```

So that means we need to enter 44 dummy characters to the input of the program, and then the address we want the program to return to.

There are no ASCII characters for `0x08 0x04 0x85 0xcb` so we need to write them as raw bytes.

Piping the output from a small BASH script into the input of the program solves this issue for us.

```sh
for a in `seq 1 44`
do
    echo -n 'a'
done

echo -e \\xcb\\x85\\x04\\x08
```

Mind that the address is entered in reverse (byte-wise) because Intel x86 is Little-Endian which means the least significant byte maps to the lowest memory address. So 0x12345678 would be stored in memory as 0x78563412.

{: style="text-align:center"}
![Output](/assets/images/writeup-pico18-bufferoverflow-1/Output.png)

And boom! We have a flag 🙂

**Flag: picoCTF{addr3ss3s_ar3_3asy3656a9b3}**

- Difficulty: Easy

- Number of points: 200

- Number of solves (as of 23/5/2019): 3065

Thanks for reading!
