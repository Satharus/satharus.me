---
layout: article
title: "CyberTalents Writeup: Egypt National Cyber Security CTF 2019" 
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
    src: /assets/images/writeup-national-ctf-2019/nationalctf19_header.jpg
cover: /assets/images/writeup-national-ctf-2019/nationalctf19_cover.jpg
---
CyberTalents’ Egypt National Cyber Security CTF 2019 was held on September 7th in Intercontinental City Stars, Cairo.

It was a pretty fun experience for me, as I had never participated in the nationals before. My team and I solved 3 challenges during the CTF and one after it.

Here are the writeups for what we solved:
<!--more-->

Note: The text for the challenges is not that accurate because I forgot to take screenshots of the challenges, so I tried to remember what they were approximately.

# Record

**Category**: General Information

**Level**: Easy

**Points**: 50

**Text**: It was something in the sense of “I run on your system and store everything you type”.

The challenge is really easy, if you’ve been looking into cybersecurity for a while you’ll know that that’s called a keylogger. If you haven’t then you can get it easily through some online searching.

**Flag: keylogger**

# The One

**Category**: Cryptography

**Level**: Easy

**Points**: 50

Text: It was something in the sense of “It was used in one of Shakespeare’s poems, it’s your turn to find his.” Along with this string: “010000110100000100110010000011011111000101110100110000000110011100110101000100101001110101010000101101011010000000001101000110111001100”

The first thing that came to mind is that it’s a normal binary string! So I tried to convert it to ASCII.

{: style="text-align:center"}
![binary wrong](/assets/images/writeup-national-ctf-2019/binarywrong.png)

It didn’t work, so I found that the string is of length 135, so I just needed to add one zero to make it divisible by 8 and see how it goes.

It did convert to ASCII, but I got this:

{: style="text-align:center"}
![garbage](/assets/images/writeup-national-ctf-2019/garbage.png)

Seems like a load of garbage. So I just figured that it wasn’t ASCII. I did a little bit of searching for “Shakespeare Cipher” and found this image:

{: style="text-align:center"}
![bacon](/assets/images/writeup-national-ctf-2019/bacon-1.png)

on [this page](https://derekbruff.org/blogs/fywscrypto/historical-crypto/shakespeares-secrets-a-hidden-cipher-in-literatures-greatest-works/)

I found out that this type of cipher is called a “Bacon Cipher”, it uses A and B (or basically any 2 characters) to encode the alphabet. Each character consists of 5 bits, kinda like how we count in binary but you move on to the next letter instead of the next number.

This was pretty good info because our original string’s length is divisible by 5.

So I wrote a small C++ program that splits the long string of 1s and 0s into parts of 5 bits each and converts 0 to A and 1 to B.

```c++
    int main()
    {
        string a = "0100001101000001001100...";
        for (int i = 0; i < a.size(); i++)
        {
            string s;
            for (int j = i; j < i+5; j++)
            {
                if (a[j] == '0') s.push_back('A');
                else s.push_back('B');
            }
            cout << s << ' ';
        }
    }
```
The output of the program was this string:

“ABAAA ABBAB AAAAA BAABB AABAA AAABB ABBBB BAAAB ABBBA BAABB AAAAA AABBA ABBBA ABBAB ABAAA BAABA BAABB BABAB ABAAA ABABB ABABB ABAAA AAAAA ABBAB AAABB ABBBA ABBAA”

I looked up “Bacon Cipher solver” on Duckduckgo and got [this site](https://www.dcode.fr/bacon-cipher).

{: style="text-align:center"}
![crypt](/assets/images/writeup-national-ctf-2019/cryptresults.png)

After decoding the message, I got this text “INATEDPROTAGONISTVILLIANDOM”.

It seemed like English, so I started dividing it into every possible word trying to get what the flag is.

My team and I started wondering, is it “IN A TED PROTAGONIST VILLIAN DOM” or “INATED PROTAGONIST VILLIAN DOM” or should I submit it as is? We tried A LOT of combinations and tried changing the case of the letters or how they were written and nothing really worked.

Until they gave everyone a clue and said “The flag is 3 English words written as is in one string”.

So after about 30 minutes of fiddling around and trying to get the 3 exact words. We reached the conclusion that the third word is “DOMINATED”.

Flag: It turned out to be **“VILLIANDOMINATEDPROTAGONIST”** or **“PROTAGONISTDOMINATEDVILLIAN”**. I don’t really remember.

Also, yes, the word villain was misspelled.

# Runecover

**Category**: Digital Forensics

**Level**: Medium

**Points**: 100

**Text**: I can’t remember at all. 😦

This challenge had a .pcap file attached, so the first thing we tried to do was to open it in wireshark: `$ wireshark runecover.pcap`.

{: style="text-align:center"}
![wireshark](/assets/images/writeup-national-ctf-2019/wireshark.png)

What a bummer. But hey, at least we don’t have to analyse packets now. My teammate tried to use binwalk on it.

{: style="text-align:center"}
![binwalk](/assets/images/writeup-national-ctf-2019/binwalk.png)

`$ binwalk -e runecover.pcap`

{: style="text-align:center"}
![binwalke](/assets/images/writeup-national-ctf-2019/binwalke.png)

That didn’t get us any useful results, so he tried using foremost.

{: style="text-align:center"}
![foremost](/assets/images/writeup-national-ctf-2019/foremost.png)

Aaaand boom! We got the png image.

{: style="text-align:center"}
![flag](/assets/images/writeup-national-ctf-2019/83.png)

# Another Tune

**Category**: Digital Forensics

**Level**: Medium

**Points**: 100

**Text**: “I thought songs were meant to be heard”

The challenge had a file attached that was named “Immigrant-Song-Led-Zeppelin(1).wav”.

We tried to listen to the track first for any clues, but we didn’t find anything unusual. We also tried opening it with Audacity but nothing was unusual at all.

So we used binwalk on it, but we got some random SQL files which we decided that at the time, we shouldn’t waste our time with them.

{: style="text-align:center"}
![binwalk immigrant](/assets/images/writeup-national-ctf-2019/binwalkimmigrant.png)

Using Deepsound(a Windows utility), however, my teammate was able to find a .rar file called CS.rar in the audio file.

{: style="text-align:center"}
![Deepsound](/assets/images/writeup-national-ctf-2019/deepsound-1.png)

Let’s check what that .rar file has inside!

{: style="text-align:center"}
![csrar](/assets/images/writeup-national-ctf-2019/csrar.png)

Wohooo! Now we can get our flag, right? NO. The .rar file is password protected.

We tried bruteforcing the password (using 4 digit numbers), and a lot of other naive brute forcing methods. None worked.

I decided to try using john the ripper.

{: style="text-align:center"}
![hash](/assets/images/writeup-national-ctf-2019/hash.png)

I got the password hash, now it’s time to find the password.

We tried using john without a specific wordlist, and that took about 2 hours and a half without any progress. So we just decided to scrape it as it got the point where it was just using incremental brute-force.

A quick thought came to my mind to use the famous rockyou.txt wordlist since, well, Led Zeppelin are a **rock** band. But sadly we didn’t have enough time to finish the brute force.

Note: I was running the brute force on my main machine using 4 threads, which still wasn’t enough apparently.

After the CTF was over, during lunch, a friend told me that when they reversed the “rockyou.txt” wordlist it actually worked.

So I did exactly that, I reversed the file.

I discovered a fun utility in Linux called “tac” which does what “cat” does but in reverse. It reads the file in reverse order.

{: style="text-align:center"}
`$ tac /usr/share/wordlists/rockyou.txt >> yourock.txt`

In less than 2 seconds, the password was cracked.

{: style="text-align:center"}
![password](/assets/images/writeup-national-ctf-2019/password.png)

Why did reversing the wordlist work?

{: style="text-align:center"}
![rockyou](/assets/images/writeup-national-ctf-2019/rockyoutxt.png)

The password “!!Boom!!” is pretty far down the list at line number 14,343,714 out of 14,344,392 lines. So you need a really fast computer to get down that far in the list as quickly as possible. Reversing the list however makes “!!Boom!!” at the top so you get it a lot faster.

Sidenote: Thanks to Ibrahim Mosaad from [State of Security](https://www.facebook.com/groups/2246549955562216/) for giving me a tip that sometimes shuffling the wordlist is a better idea than reversing it. Which makes sense as ideally it should give you a better overall average. It really depends on the challenge you’re solving.

Sigh, now we can actually get our flag.

{: style="text-align:center"}
![base64](/assets/images/writeup-national-ctf-2019/base64.png)

lol jk, you need to base64 decode it first.

{: style="text-align:center"}
![cs final flag](/assets/images/writeup-national-ctf-2019/cs_finalflag.png)

There we go, finally.

**Flag:** CSFLAG{As of right now, we are at war}

I hope this writeup was useful. It is my first writeup so I’d appreciate any feedback on it. Share it with your hacker friends if you thought that it was fun to read :D!

Thanks for reading!

Cover image credit: [CyberTalents](https://cybertalents.com/competitions/final-egypt-national-cyber-security-ctf-2019).
