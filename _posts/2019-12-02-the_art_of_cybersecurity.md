---
layout: article
title: The Art of Cybersecurity (And how to get into it) 
categories: [Cybersecurity]
author: Ahmed Elmayyah
tags: [Cybersecurity, Guide]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: /assets/images/the-art-of-cybersecurity/init3_header.png
cover: /assets/images/the-art-of-cybersecurity/init3_cover.png
---

You put on your black hoodie, install Kali Linux, change your Facebook cover photo to edgiest pic related to security that you can find and boom! hacker mode. Hell yes, you’re ready to become the next Elliot Alderson and hack the FBI with your 1337 command line tools, that router in your neighbour’s living room that you always wanted access to, and …. let’s just take it back a couple of steps, shall we?

<!--more-->

Cybersecurity isn’t like it is shown in the average Hollywood show or move. It is an art, it consists of many smaller forms of art. I’ll talk about a few of them today.

If we strip everything down, cybersecurity is basically “protecting computer systems from theft or damage”. Sounds simple enough right?

Maybe. But, the field of cybersecurity is huge and there are a lot of branches coming out of it. And while learning about cybersecurity and its fields can make you the hacker you always wanted to be, you’re probably gonna find it easier (and definitely less/not illegal) to practice hacking ethically through bug bounty programs and research. More on that later.

Let’s take a look at a few of the fields you can start studying today, sorted from easiest to hardest, in my opinion. That does not imply that reverse engineering is hard, it is just not as easy for the average beginner. You still have to give it a try before you judge anyway.

Keep in mind that this guide is by no means the “perfect roadmap coming from an expert”. I am just a security enthusiast who happens to have a blog.

So please, check it out and also check out what other security enthusiasts are saying and then try and decide what works best for you.

# Web Security

{: style="text-align:center"}
![xss](/assets/images/the-art-of-cybersecurity/xss.png)

The art of exploiting web applications, websites, servers, and the vulnerabilities related to them.

Web security is a great place to start if you want to get straight into bug bounty programs.

Bug bounty programs on sites like [HackerOne](https://www.hackerone.com/), are programs where companies ask you to test their programs, mobile apps, websites, web app, etc.. and submit any security flaws that you find. You get a reward for every bug you find and report successfully.

## A Small Example

A website has an issue with filtering input, and you can give it certain input to gain access to private information or the database.

Check out these 2 videos to get a general idea:
- [Running an SQL Injection Attack – Computerphile](https://www.youtube.com/watch?v=ciNHn38EyRc)
- [Cracking Websites with Cross Site Scripting – Computerphile](https://www.youtube.com/watch?v=L5l9lSnNMxg)

## Resources

- Courses:
    - [Web Application Penetration Testing Course Instructed by Ebrahim Hegazy](https://www.youtube.com/playlist?list=PLv7cogHXoVhXvHPzIl1dWtBiYUAL8baHj)
    - [Hacker 101](https://www.hackerone.com/hacker101)
    - [Cybrary](https://www.cybrary.it/)
    - Paid courses like SANS SEC401.

- Books:
    - Mastering Modern Web Penetration Testing by Prakhar Prasad
    - The Tangled Web: A Guide to Securing Modern Web Applications by Michal Zalewski
    - [Web Hacking 101](https://hackerone.com/resources/web-hacking-101) by Peter Yaworski
    - The Web Application Hacker’s Handbook by Dafydd Stuttard

- Interesting stuff:
    - [NodeGoat](https://github.com/OWASP/NodeGoat)

Thanks to Aly Anwar and Amr Fathi for recommending most of these resources.

# Digital Forensics

{: style="text-align:center"}
![vol](/assets/images/the-art-of-cybersecurity/vol.png)

The art of analysing the footprints and the evidence. The art of backtracking.

This branch of cybersecurity focuses on the analysis of traces and captured files such as captured network traffic, recovered hard drive images, memory dump analysis, etc..

It is basically tracing back the steps that caused the attack, crash, exploit, etc...

Analysis is usually done through tools that help you examine the evidence(files) you have at hand so that you can get to the tiniest of details.

It’s also worth noting that there is a branch of digital forensics called steganography, which is the art of hiding messages in other messages. Such as hiding text in an image or a file in a video.

## A Small Example

A server was running and then crashed because of a malware. You can then analyse the memory dump that was produced at the time of the crash to trace back what caused this to happen.

Check out these 2 videos to get a general idea:

- [Digital Forensic Memory Analysis – Volatility](https://youtu.be/Cs0Gc3GtfZY)
- [Secrets Hidden in Images (Steganography) – Computerphile](https://youtu.be/TWEXCYQKyDc)

Check this write-up for a good example too:

- [InCTF 2019 – Notch It Up](https://fireshellsecurity.team/inctf2019-notch-it-up/)

## Resources

- Courses:
    - [13Cubed Youtube Channel](https://www.youtube.com/user/davisrichardg)
    - [Cyber Triage/Autopsy DFIR Training](https://www.cybertriage.com/training)
    - A few forensics courses on [OpenSecurityTraining.info](http://opensecuritytraining.info/Training.html)
    - Paid courses like SANS 500, 508, 518, 526, and the list goes on.

- Misc:
    - [This cool article I found](https://www.forensicscolleges.com/blog/resources/guide-digital-forensics-tools)
    - [Forensics – CTF Field Guide](https://trailofbits.github.io/ctf/forensics/)

Thanks to 0x01G33k for telling me about some of these resources.


# Reverse Engineering

{: style="text-align:center"}
![asm](/assets/images/the-art-of-cybersecurity/asm.png)

The art of analysing and exploiting binaries.

This branch of cybersecurity focuses on the analysis of binary files. Including malware, of course.

Analysis of binaries is usually done using disassemblers or decompilers which help you read the assembly (or even the high level code) in some cases.

## A Small Example

A malware hits a company’s server. To clean up, you have to know what the malware did exactly and try to: A, disable it, and B, fix the damage done completely.

Check out these 3 videos to get a general idea:

- [MMORPG Bot Reverse Engineering and Tracking - LiveOverFlow](https://www.youtube.com/watch?v=irhcfHBkfe0)
- [Introduction to Reverse Engineering \| Ollydbg Tutorial - The PC Security Channel](https://youtu.be/D6mVIos-S2M)
- [Buffer Overflow Attack – Computerphile](https://youtu.be/1S0aBV-Waeo)

## Resources

- Courses:
    - [OpenSecurityTraining.info](http://opensecuritytraining.info/Training.html) (Literally one of the best)
        - Start with [Intro to x86](http://opensecuritytraining.info/IntroX86.html) and then check out the maps on the site to continue in one of the reverse engineering paths.
    - [Ranjeet Mewada’s Youtube Channel](https://www.youtube.com/channel/UCPE-O5ZBeFqqMs1qHZnFMZw/playlists)
    - [Malware Unicorn](https://malwareunicorn.org/#/workshops)
    - [Reversing Hero](https://www.reversinghero.com/)
    - [beginners.re](https://beginners.re/)
- Reading Material:
    - [Understanding Windows x64 Assembly](https://sonictk.github.io/asm_tutorial/)
    - [onethawt/reverseengineering-reading-list](https://github.com/onethawt/reverseengineering-reading-list)
    - [Malware Analysis Tutorials, Cheatsheet & Tools list](https://gbhackers.com/malware-analysis-cheat-sheet-and-tools-list/)
    - [Android App Reverse Engineering 101](https://maddiestone.github.io/AndroidAppRE/)
    - [Reverse engineering Flutter apps](https://blog.tst.sh/reverse-engineering-flutter-apps-part-1/)
    - [More Material](https://www.msreverseengineering.com/program-analysis-reading-list)

Of course, that’s not all.

There are many many more fields, to name a few:

- Cryptography
- Network Security
- Binary Exploitation
- Cloud Security
- Blockchain Security
- and much much more.

# Now What?

Well, now that you know a little bit about 3 of the major cybersecurity fields, you can start practicing on CTF platforms. Here are a few sites:

- [PicoCTF](https://picoctf.com/)
- [CyberTalents](https://cybertalents.com/challenges)
- [Root-Me](https://www.root-me.org/)

There’s also this list that has many many more: [fareedfauzi.github.io/ctfonline](https://fareedfauzi.github.io/ctfonline/)

You can start with PicoCTF as it is the easiest and learn as you go as the challenges are really beginner friendly at first and then gradually get harder.

Once you find yourself liking a certain type of challenges, you can start studying it more in depth and solving CTFs of that type on other sites as well.

All you’ll need is your mind and a search engine.

You can also check out Wargames at [OverTheWire](https://overthewire.org/wargames/). I recommend that you start with [Bandit](https://overthewire.org/wargames/bandit/).

# Other Relevant Resources

- CTFs
    - [How To Learn Hacking With CTFs](https://youtu.be/Lus7aNf2xDg)
    - [CTF Time](https://ctftime.org/)
    - [JohnHammond/CTF-Katana](https://github.com/JohnHammond/ctf-katana)
    - [CTF Writeups](https://github.com/ctfs)
- Linux Resources
    - [Linux Tutorial for Beginners Playlist](https://www.youtube.com/watch?v=HjuHHI60s44&list=PL6gx4Cwl9DGCkg2uj3PxUWhMDuTw3VKjM)
    - [OSC19-Linux-Workshop-Sessions](https://github.com/Open-Source-Community/OSC19-Linux-Workshop-Sessions)
- Youtube Channels
    - [LiveOverflow](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w)
    - [Null Byte](https://www.youtube.com/channel/UCgTNupxATBfWmfehv21ym-g)
    - [STÖK](https://www.youtube.com/channel/UCQN2DsjnYH60SFBIA6IkNwg)
    - [The Cyber Mentor](https://www.youtube.com/channel/UC0ArlFuFYMpEewyRBzdLHiw)
    - [John Hammond](https://www.youtube.com/user/RootOfTheNull)
    - [Seytonic](https://www.youtube.com/channel/UCW6xlqxSY3gGur4PkGPEUeA)
    - [Computerphile](https://www.youtube.com/channel/UC9-y-6csu5WGm29I7JiwpnA)
    - [Open Security Training](https://www.youtube.com/channel/UCthV50MozQIfawL9a_g5rdg)
    - [HackUCF](https://www.youtube.com/channel/UCmCAVPGJTOrOc-Ux7Y0DHiw)
    - [CyberTalents](https://www.youtube.com/channel/UCAtX7DF2s5f8pzooZowEa1A)
    - [BlueKaizen](https://www.youtube.com/channel/UCv1o5TfVe-srBDmB-O8NWkQ)
    - [Arab Security Conference](https://www.youtube.com/channel/UCbmwDzhCYTfQCKpLApJPapg)
    - [Ben Eater](https://www.youtube.com/user/eaterbc)
- [SECRET HACKER FORUM](https://youtu.be/VCwiZ2dh17Q)
- [An Overview of Cryptography – Gary C. Kessler](https://www.garykessler.net/library/crypto.html)

I hope that this guide was enough to get you sort of started with cybersecurity.

You may feel lost at first and that is completely normal as it is a huge field.

I’ve updated this guide a lot since it was first written. So, please, if you have any recommendations for study material, by all means tweet them at me, or email/dm them to me and I’ll add them to this guide or any future upcoming articles if the material doesn’t fit here.

Share this guide with your friends if you found it useful.
As always, thanks for reading!
