---
layout: article
title: "EG-CERT Writeup: EG-CTF Finals 2019" 
categories: [Cybersecurity]
author: Ahmed Elmayyah
tags: [Cybersecurity, Writeup, CTF]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: /assets/images/writeup-egcert-finals-2019/cert_header.jpg
cover: /assets/images/writeup-egcert-finals-2019/cert_cover.jpg
---
EG-CTF Finals 2019 was held on December 4th in EIEC, New Cairo at ICT 2019’s venue.

The challenges were extremely fun and challenging and I learned A LOT during the CTF.
<!--more-->

{: style="text-align:center"}
![score](/assets/images/writeup-egcert-finals-2019/score.png)

Having solved 5 challenges alongside my team, here are the writeups for them:

# Category: Starter

## I - Baby P“

{: style="text-align:center"}
![text](/assets/images/writeup-egcert-finals-2019/text.png)

The code is a language called BrainFuck

> ++++[++++>---<]>++.++.----.>-[--->+<]>-.[----->+<]>++.>--[-->+++++<]>.>+[--->++<]>+.+++[->++++<]>.[-->+<]>----.[->++<]>-.+[--->++<]>++.>-[----->+<]>.+++[->++<]>.---.-[-->+<]>-.--[--->++<]>.[++>---<]>++.--[--->++<]>---.++[->+++<]>.-----------.[---->+++++<]>-.++[->+++<]>++.---------.++[--->+++++<]>.-----------.[---->+++++<]>-.--[--->+<]>-.+[----->++<]>-.++[->+++<]>.+[->+++<]>+.+[----->++<]>-.++[->+++<]>.---------------.[--->+<]>++.--[->+++++<]>.-[--->+<]>++.+++++++++++.[-->+<]>-----.++[->++<]>+..[->+++++<]>.[----->+<]>+.-------.--------.+++++++++++++.-[->+++<]>-.+[--->+<]>+++.-------.--[--->+<]>--.+++[++>---<]>.[->++<]>-.-[-->+<]>.>--[-->+++<]>.

You can run it at [copy.sh/brainfuck](https://copy.sh/brainfuck/)

**Flag: EGCTF{Wh0_B3li3v3s_Th@7_Th!s_!s_Pr0gr4mm!ng_l@ngu4g3}**

## II - Not Straight!

{: style="text-align:center"}
![text 1](/assets/images/writeup-egcert-finals-2019/text-1.png)

> PV2W66K7GRPTOMK7N5CF62BXGVPTOM2ML5JE6XZXGFPTGNZUJVHTO5JUL5CEYVLPNA2V65KPLFPUITRUL5ZDGNZXGNWF66LCL5JDGNZXGNGF6NZRL5XUIX2PG5PTGNKOGM2V66LOGRPTG2ZUJVPTO33OL42TGT2EL43TCXZQGVPXEM2EKIYF6MZVKIZXMM2SL5HCCX3OGM3TOMKSO5PUONDMIZPUOTRQJRPTIXZVEFPTKMKIG55UMVCDI5CQU===

The code ends in === so I guessed that it was base64, but it wasn’t.

I tried base32 and it worked fine. But, I got the output reversed so I had to reverse it again to get the actual flag.

A one-liner for this would be: `echo BASE32TEXT | base32 -d | rev`

Of course you need to replace BASE32TEXT with the text given in the challenge.

**Flag: EGCTF{7H15_!5_4_L0NG_Fl4G_wR1773n_!N_R3v3R53_0RD3r_50_17_DO35_no7_M4k3_4ny_53N53_7O_Do_17_L3773R_by_l3773r_4ND_YOu_5hoULD_4u7OM473_17_OR_L37_57h_Do_17_4_you}**

## III - So call be maybe?

{: style="text-align:center"}
![text 2](/assets/images/writeup-egcert-finals-2019/text-2.png)

All of the numbers are between 0 and 8 so it seems like something you’d type on a phone keypad.

The first thing that came to my teammate’s mind was T9 predictive text, which was the autocorrect of typing on phones in the early 2000s.

You can easily decipher it here: [dcode.fr/t9-cipher](https://www.dcode.fr/t9-cipher)

{: style="text-align:center"}
![t9](/assets/images/writeup-egcert-finals-2019/t9.png)

**Flag: EGCTF{SENTENCES SHOULD MAKE SENSE AFTER ALL}**


# Category: Misc.

## SCA101

{: style="text-align:center"}
![text 3](/assets/images/writeup-egcert-finals-2019/text-3.png)

At first,the hint wasn’t pushed to the contestants yet.

On connecting to the server, we find that we can give it a string and it replies with “BAD FLAG!!!!”. So it seems to be checking the string we’re sending if it matches the correct flag.

We already know that all flags start with EGCTF{, so my teammate tried sending parts of the flag to see what it will do. He found out that the server delays the response by 1 second for each correct character in the string you send.

{: style="text-align:center"}
![initial-test](/assets/images/writeup-egcert-finals-2019/initial-test.png)

At this point we had no idea what to brute force, so they released this hint for everyone.

{: style="text-align:center"}
![hint](/assets/images/writeup-egcert-finals-2019/hint.png)

This made it much easier, so it’s testing time!

I tried sending EGCTF{0 and then EGCTF{1, etc.. until I found that the first digit is 7. Then I figured it was scripting time (sort of) as I wasn’t gonna try all of the digits manually.

I wrote a small loop in BASH that increases the digit by one every time and sends it to the server, calculating the total time that the command took.

We know that it delays one second for every correct character, so if the number of seconds increase then we know that we entered a new correct character.

{: style="text-align:center"}
![getting-digitstext](/assets/images/writeup-egcert-finals-2019/getting-digitstext.png)

Repeating this for every digit, we got ourselves the flag!

{: style="text-align:center"}
![final flag](/assets/images/writeup-egcert-finals-2019/final-flag.png)

**Flag: EGCTF{73b433927a}**

# Category: Reverse Engineering

## Spaghetti

{: style="text-align:center"}
![text 4](/assets/images/writeup-egcert-finals-2019/text-4.png)

My first instinct was to open the file with IDA-Free, but it turned out to be a .NET binary. Onto DnSpy we go!

Before that, let’s try running the binary..

{: style="text-align:center"}
![no args](/assets/images/writeup-egcert-finals-2019/noargs.png)

Doesn’t seem to do anything interesting, let’s take a look under the hood.

{: style="text-align:center"}
![mainclass](/assets/images/writeup-egcert-finals-2019/mainclass.png)

{: style="text-align:center"}
![ciphertext](/assets/images/writeup-egcert-finals-2019/ciphertext.png)

{: style="text-align:center"}
![encrypt class](/assets/images/writeup-egcert-finals-2019/encryptclass.png)

We find that we have 3 classes:

- Program class
- A class with some sort of ciphertext
- A class that seems to encrypt/decrypt text

Okay, cool. Let’s take a look at the Program class.

{: style="text-align:center"}
![arg func](/assets/images/writeup-egcert-finals-2019/argfunc.png)

This part is interesting enough, the program apparently needs 4 arguments to execute properly. Let’s try that.

{: style="text-align:center"}
![4args-1](/assets/images/writeup-egcert-finals-2019/4args-1.png)

So it works, it prints a different string and waits for a keypress to exit.

Sadly, the names for the functions were all garbage. This made it kind of hard to get an idea of what was exactly going on. I wasted a lot of time renaming the functions, but it helped me understand what was going on.

{: style="text-align:center"}
![cleanprogram](/assets/images/writeup-egcert-finals-2019/cleanprogram.png)

On looking at the clean program, we find that the program basically decrypts the ciphertext that was in the cipher class and then executes a powershell with the decrypted text as an argument.

{: style="text-align:center"}
![debug](/assets/images/writeup-egcert-finals-2019/debug.png)

I debugged the program until I could find the local variable containing the decrypted text. It is way too long for me to post here, but anyway it seemed like garbage with some meaningful code-like strings in the middle.

{: style="text-align:center"}
![decoded code](/assets/images/writeup-egcert-finals-2019/decoded-code.png)

{: style="text-align:center"}
xqq is our decoded message

I had no idea what that decoded message was so I literally searched for “`((GV ‘*mdr*’)`” on DuckDuckGo and found out that it was obfuscated PowerShell code.

So I tried running it in PowerShell and got “Failed!” as an output.

{: style="text-align:center"}
![initial powershell](/assets/images/writeup-egcert-finals-2019/initial-powershell.png)

So it was time to deobfuscate that code and get an idea of what it is doing.

I looked online for quite some time until I stumbled across another writeup that showed that you can treat this whole code as a string and pass it as an argument to `Write-Output` in PowerShell. We just have to remove the part at the start until -JOiN”).

So here we go, It seems to make a little bit more sense.

{: style="text-align:center"}
![second powershell](/assets/images/writeup-egcert-finals-2019/second-powershell.png)

And Again.

{: style="text-align:center"}
![third powershell](/assets/images/writeup-egcert-finals-2019/third-powershell.png)

{: style="text-align:center"}
Oh, it’s base64

Aaaannndd Again

{: style="text-align:center"}
![final powershell](/assets/images/writeup-egcert-finals-2019/final-powershell.png)

You can see the flag being compared in the most retarded way possible. Nevertheless, I cleaned the output a bit, did some dirty BASH magic here and there, and got the flag.

{: style="text-align:center"}
![cleaned file](/assets/images/writeup-egcert-finals-2019/cleanedfile.png)

The command would be: `cat flagCode.ps  | tr {} '\n' | cut -d \' -f 2 | tr -d '\n' | tr -d ' '`

{: style="text-align:center"}
![finalcommand](/assets/images/writeup-egcert-finals-2019/finalcommand.png)

We just have to replace these closing brackets with the appropriate braces.

**Flag: EGCTF{707331c1a40117b7b4f9e33ec295269217bc8df68ad21dd04c867754de56e07a}**

That’s all folks! It was one hell of a CTF and I enjoyed it so much.

If you liked this writeup please share it with your friends. Thanks for reading! 😀

Featured image credit: [EG-CERT](https://www.egcert.eg/)
