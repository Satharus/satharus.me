---
layout: article
title: "Shield Yourself: Five Tips to Strengthen your Online Security" 
categories: Cybersecurity
author: Ahmed Elmayyah
tags: [Cybersecurity, Cybersecurity Awareness, Guide, Privacy]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: //assets/images/shield-yourself-online-security-tips/shieldyourself_header.jpg
cover: //assets/images/shield-yourself-online-security-tips/shieldyourself_cover.jpg
---

I could talk for the entirety of this post about how connected we are and how much we rely on the internet and technology and all that stuff but I am pretty sure we all know that. So, let's just get straight to the point. The fact is that most people have horrible online security habits. 
<!--more-->
We reuse passwords, we set weak passwords, we accidentally post sensitive data online, and many more bad practices to the point that we more than often prioritise convenience over security. 

What inspired me to write this post is that when I asked a couple of people in my inner circle, they admitted that they reuse pretty weak passwords and have a couple of bad security practices. The bigger problem is that most of my inner circle are techies and if that is what they do, I can guess how bad it is for people who aren't really tech-savvy. In this post, I'll give you five things you can start doing literally now to improve your online security.

## \#1 : Use Multi-Factor Authentication
This is my number 1 tip for anyone as it is often the most abused weakness when present.
### More than one factor?
You see, we often think of usernames and passwords as a factor of authentication. But, they aren't the only factors. Factors of authentication have been classified as the following:
- **Something you know**: A password, a pin, etc...
- **Something you are**: Something that is part of you, your fingerprint or eye scan
- **Something you have**: A key, your mobile phone, a key card, etc...
- There are also "**something you do**" and "**somewhere you are**" but those aren't used as much as they can be impersonated fairly easily

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/2fa.png)

Combine two or more of those and you have got a pretty functional MFA system.

### Example
- You login with your password (something you know) and get an SMS with a code on your phone (something you have) used to verify that this is you logging in
- You login with your password (something you know) and an app on your phone asks you to verify your fingerprint (something you are) 

### Which factors should I use?
Any two or more of them. You can use your fingerprint via your phone. You can use a physical USB key such as [YubiKey](https://www.yubico.com). You can use an authenticator app on your phone such as [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2).

DO NOT USE YOUR PASSWORD MANAGER (see the next section) AS YOUR MFA. This makes it essentially a single factor as both methods of authentication are accessed from the same application.
{:.error}

## \#2: Use a Password Manager

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/password_managers.png)

This tip is also extremely important as it makes it difficult for evil people to get your password to begin with.
### Passwords
Most of the time we use passwords as our login method. If one password gets known somehow and you're using it somewhere else, that is a huge issue as that other account could be accessed using the leaked password too. 

A small technical detail: Passwords are usually stored on servers for whichever service/website you use. However, to increase security, the passwords are never saved as they are (this is called plaintext). When you register, the password goes through a mathematical function which derives a very large number that is called the hash from the password you entered. After that, if everything is implemented well. No one should ever know what your password is! When you try to login, the server hashes the password you entered and compares it to the one stored on the server before letting you log in.
{:.info}

Have any of your current passwords been leaked before? Well, why don't you go over to [haveibeenpwned.com](https://haveibeenpwned.com/) and find out! This website will show you which websites you use have been hacked and if your password was found in the database. If your password was weak, a hacker may have been able to figure out what it is. It is a great idea to change the passwords for the accounts (and everywhere else you use that same password) that appear on that page.

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/haveibeenpwned.png)

{: style="text-align:center"}
Well, it looks like I have been pwned :(

#### Attack Example

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/email.png)

Have you ever received an email like this? If you have, you'll know that usually, they send you your real password. But well, how do they know it? They usually find it in an online leak or find its "hash" mentioned above and figure it out by trying many different passwords until they get it. Sometimes they even send you this email but with a password you no longer use. This just shows that if you've changed your password lately, they _probably_ don't actually have access to your account. If you ever get an email like this. Change your password, reset 2FA recovery codes, delete it, and pray that it won't end like the Black Mirror episode "Shut up and Dance".

### Password Manager?
A password manager is simply software that generates strong passwords for your online accounts and stores them for you. The passwords are stored either locally or on a server in the cloud and are encrypted with your master password, which is the main password you use to access your other passwords. Most password managers have fancy features such as auto-filling your login details when logging in and automatically locking your password vault when you're away. You can use an offline local one like [KeePassXC](https://keepassxc.org/) (which is free and open source) or any other cloud-based password manager. I won't recommend any specifically as they're mostly commercial products. However, you can check out [passwordmanager.com](https://www.passwordmanager.com) for comparisons between products categorised by their support for operating systems.

### Master Password?
Well, the master password for your password manager is crucial. This is a password that has to be strong, easy to remember for yourself, and at the same time long enough to not be guessed. 

Now you may be asking, what's a strong password? A strong password is long, doesn't contain any personal information, and has a good combination of mixed-case characters, numbers, and special characters.

#### Example
If your wife is called Linda and your birthday is on the 21<sup>st</sup> of January 1977. `Linda*21011977` is a **horrible** password. It is "long", has a special character, a number, and mixed-case characters. However, anyone who spends maybe 3 minutes on your Facebook profile, Instagram, Twitter, or anywhere you have personal info posted should be able to figure out your birthday and your wife's name. They can then start guessing and trying different combinations such as `linda21011977`, `21011977-linda`, etc... It goes without saying that this doesn't apply to your birthday and your wife's name only. It applies to the birthdays of friends and relatives, places you've lived, and things you like. Anything that could be figured out from your public profiles or found through social engineering is a big no.

To avoid confusion, keep in mind that the hacker won't be using the regular login screen which has a timeout after usually 3 tries. They'd usually have to obtain your password's hash from a leak or similar.
{:.info}

A good master password would be something like `Junkyard-Museum-Dr1ver-Apple` which makes absolutely no sense and has nothing to do with you. You can also create a fun story in your head to remember it!

I really shouldn't have to mention that using that specific password is not a good idea. Generate and use something similar, but not that exact one.
{:.error}

### How do I get started?
1. Choose a password manager
2. Spend the lesser part of an evening changing the passwords of your most important accounts to ones automatically generated by the password manager 
	- That could be your bank, social media accounts, game accounts, etc... Anything you value
3. Use the password manager to login now instead of using your old password
4. Whenever you use an account which you haven't changed the password for, change it and add it to the password manager
5. Enjoy your enhanced security

## \#3: Protect your Bank Cards 

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/debit.png)

{: style="text-align:center"}
Image credit: Wikimedia Commons


The convenience of using your online debit/credit card is very nice. You don't have to worry about cash or anything and most of the time you find it necessary to shop online. Needless to say, it is a horrible practice to enter your card number everywhere you want to shop. A lot of online vendors store your payment info in very non-secure ways. Because of that, you'd usually want to avoid entering your payment details anywhere.

### What should I do then?
- Use a temporary or virtual card when possible
	- This feature exists for some banks where you can create a virtual card from your banking app
	- It also exists for some mobile wallets and other similar options
- Use cash on delivery or pay using another method which doesn't include handing over your payment details
- Use a service such as [PayPal](https://www.paypal.com/) and pay using it when possible
	- Sure, you've handed over your payment details to PayPal. But, would you rather give it to _just_ PayPal or give it to all other websites and services?


## \#4: Be Careful About What You Post and Where

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/Location.png)

{: style="text-align:center"}
Image credit: Stockio.com

It can be tempting for a lot of people to share details of your life online. Sure, it can be fun. But also, when sharing details be careful. Are you posting pictures with location data in them? Do you have personal data on your profile that is unnecessary? 

What you post online is used to identify you. It can be used to guess your passwords, location, where your home is, when your home is empty, etc... 

{: style="text-align:center"}
![](https://i.redd.it/31fmyyl24ug51.jpg)

Think of something like this picture above but for your social media profiles. I've come up with a short non-exhaustive list of things you should avoid posting:
- **Location data**: Where you live, where you are, where you go often, etc...
- **Your personal schedule**: When you're out for work, when you're in the gym, etc...
- **Personal details**: Your birthday, your exact wedding day (i.e. anniversary date), etc...

Malicious actors can use this information against you in many different ways. One of which is impersonation, they may be able to impersonate you using the information you have made public. They could also tie the information with the real world and perform malicious actions such as theft.

Bonus: This isn't just limited to what you post yourself online. Check the permissions of your phone apps, see if any of them are collecting your location, accessing your files, or recording using your microphone when they shouldn't.
{:.success}

## \#5: Protect your Devices

{: style="text-align:center"}
![](/assets/images/shield-yourself-online-security-tips/AVs.png)

Protecting your computer or phone is perhaps the most crucial step. No security or privacy really matters if your device gets malware or gets stolen, right? I mean, sure, maybe you don't have important files but aren't you logged in from your browser on important accounts?
### Software Protection
This is quite simple. Invest in a proper anti-malware solution (also known as an antivirus) and keep it updated. There are way too many solutions out there. I will not recommend any of them, just do your research and buy whatever works for you and is within your budget. And please, please, don't pirate your antivirus. That is one of the worst things you can do.

### Physical Protection
This boils down to maybe three things you can do without going through too much hassle:
- Always lock your device when you're not using it or leaving your desk
- Try to keep your device in physically safe places so that they won't get stolen
	- I know, nobody loses their device on purpose. Just remember to be careful
- Avoiding plugging unknown devices into your devices or vice versa, including:
	- USB drives you found somewhere
	- Mice/Keyboards/Thunderbolt docks that aren't from a trusted person
	- Public USB charging sockets or stations for phones

## Summary
Here is a nice tl;dr for you if you just want the tips right away!
 1. Start using MFA everywhere
 2. Use a password manager, make your passwords stronger
 3. Avoid as much as you can entering your debit/credit card number anywhere
 4. Be very careful what you post online, is it really necessary?
 5. Install an antivirus, keep it updated, avoid plugging your devices into anything untrusted and vice versa


Thanks a lot for reading! I really hope this post has made you realise how small things you can do can help you go a long way in your online security. I hope it also showed you how small things we dismiss as unimportant may be a security or privacy risk to us! If you ever feel like the things mentioned here are too much effort, just remember that convenience and security are two opposite sides of the scale. You can't have both fully at any given time, but you can compromise based on how much risk you can afford and how much effort you can do.

If you liked this post, please share it with your friends who are worried that they are being surveilled by The Galactic Empire.
