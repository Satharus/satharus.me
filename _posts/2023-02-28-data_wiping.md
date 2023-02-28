---
layout: article
title: "Formatting your Hard Drive isn't Enough" 
categories: Tech
author: Ahmed Elmayyah
tags: [Tech, Tech Facts]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: //assets/images/data-wiping/data-wiping_header.jpg
cover: //assets/images/data-wiping/data-wiping_cover.jpg
---

We've all been there, you want to sell your laptop, old PC, or even your hard drive. Then, you recall reading somewhere that data can be recovered from drives even after they've been _formatted_... and, well, of course they can! You look it up on the internet and find not less than seven local businesses and over a hundred global ones that claim to recover data from drives which have been lightly beaten, formatted, corrupted, and many other scenarios.
<!--more-->


You start panicking that the next owner of your item will be able to recover all your _dark_ secrets and plans to conquer the entire planet. But wait, there is no need to panic! Let's take a look at why files are recoverable in the first place and also talk about a few methods on how to securely wipe them.

## Data Storage
### Storage Devices
When we store data, we usually resort to one of two options. Either a hard disk drive (HDD) or a solid state drive (SSD). In both of these cases, they are just physical devices which your computer uses to store your data (be it evil plans to conquer the planet or photos of your cat). These two types of devices use different kinds of technology. However, due to the use of the logical layer we will talk about in the next section, a lot of the time data is partially or even fully recoverable.

{: style="text-align:center"}
![Hard Drive](/assets/images/data-wiping/HDD.jpg)

{: style="text-align:center"}
A typical hard drive (HDD) with its internals exposed

### Filesystems
When it comes to the software (logical) side, operating systems use filesystems to organise and store data on physical drives. A filesystem is basically a way of organising files on a physical disk. It makes it possible for operating systems to organise and locate files, as well as keeping track of their metadata (creation time, size, modification time, etc...).

{: style="text-align:center"}
![Filesystem](/assets/images/data-wiping/Filesystem.png)

{: style="text-align:center"}
A very rough illustration of what a filesystem is

#### Filesystem Tables
A very high level abstraction is that these filesystems usually have indices/tables which indicate where files are located on the physical drive. The OS would then consult these tables whenever it needs to read a file or modify it. An example of such "tables" is the Master File Table "`$MFT`" for the NTFS filesystem used on Windows. Again, this is a major abstraction so please don't @me.

{: style="text-align:center"}
![MFT1](/assets/images/data-wiping/MFT Example1.png)

{: style="text-align:center"}
An example of a filesystem table


When a file is deleted, it isn't actually deleted from the physical drive. Its location is just marked as "free" but the data still resides there until it is overwritten. So basically in this example, the physical location of "World Domination Plan.txt" and other _"deleted"_ files still have their contents at their physical drive locations. However, the filesystem table has their entries marked as "free". If a new file were to be created in any of their locations, it would overwrite the data on the physical drive which has a "free" entry pointing to it.

{: style="text-align:center"}
![MFT2](/assets/images/data-wiping/MFT Example2.png)

{: style="text-align:center"}
An example of a file deletion (note that the data still resides)


This means that when you delete a file, its content isn't actuallty wiped until it is overwritten. The file may be overwritten within an hour, or maybe it will take 10 years. It all depends on how often the drive is used. I was once able to recover 6 year old files from a hard drive of mine as it wasn't a drive I frequently created new files on. The same goes for _"formatting"_ the drive. Most implementations of drive formatting only modify enough parts of the drive to make it usable through a specific filesystem (such as ext4, NTFS, FAT32, etc...), they don't actually wipe the entire drive. This takes us to the next point. If formatting isn't enough and leaves behind traces, how can we recover the "deleted" files?

## Data Recovery
So far we have learned how data is stored on drives, but how does any of this make data recoverable after deletion? Well, we actually (sort of?) answered that indirectly but let's talk about how it is typically done. Again, this is a very major abstraction. In real life scenarios things happen a bit differently but the concept itself is the same.

Usually, data recovery software would check the filesystem table for entries that are marked as "free" and extract the data from the physical locations that they point to. There are other data recovery mechanisms such as file carving, which basically looks for specific file signatures in the unallocated spaces on the drive, but let's not get into too many details!

Data recovery can be done by a lot of different programs from different vendors. [PhotoRec](https://www.cgsecurity.org/wiki/PhotoRec) is a free and open source utility for the Windows, Linux, macOS, and a couple of other OSs. There are also a lot of other commercial solutions available.

{: style="text-align:center"}
![Photorec](/assets/images/data-wiping/PhotoRec_running.png)

## Digital Data Disposal
Digital data disposal methods differ depending on whether you have an SSD or HDD at hand. Let's take a look at both.
### Hard Drives
#### Secure Deletion
When it comes to _actually_ deleting the data, it is usually done by marking the filesystem table entry as "free" and at the same time also overwriting the data in the physical location with a random pattern and then zeros. It also cleans most evidence of the file's existence from the filesystem's table. This ensures that there is absolutely no way of recovering the wiped data.

We have a couple of different wiping tools that we can use. A popular one that is free and easily accessible is [SDelete](https://learn.microsoft.com/en-us/sysinternals/downloads/sdelete) from the SysInternals suite for Windows. For Linux, we have the GNU `shred` utility, `secure-delete`, and a couple of other methods which can be found on the [Arch Wiki](https://wiki.archlinux.org/title/Securely_wipe_disk). These tools can be used to delete individual files and some of them can be used to securely wipe recoverable areas of the disk.

#### Drive Degaussing
Hard drives are basically magnetic platters. We can use a strong magnet to practically ruin the structure of the hard drive. This is a form of physical destruction and renders the drive unusable after doing it.

### Solid State Drives
Good news! SSDs actually (in most cases) regularly zero out locations which are marked as free in the filesystem table in order to preserve their lifespan. This is called `TRIM` and is implemented for most -if not all of- modern SSDs and OSs. Even better news, most SSDs have utilities created by their manufacturers or are available through the UEFI BIOS to securely erase and zero out the entire SSD or a specific partition. This wipes any existing files completely.

It is extremely dangerous to run hard drive (HDD) secure deletion software on SSDs, it shortens their lifespan significantly and are often unnecessary. Please resort to using the software supplied by the manufacturer.
{:.warning}


### Physical Destruction
Well, we discussed a couple of humane ways to destroy data. But, what if the data is extremely sensitive? What if your plan to conquer the world is so secret that you can't trust any software or any human being? Well, in this case, The National Institute of Standards and Technology's (NIST) [Guidelines for Media Sanitisation](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r1.pdf) recommendeds that you physically destruct the drive since that the data is so highly sensitive.

{: style="text-align:center"}
![](/assets/images/data-wiping/Gilfoyle.gif)


I hope this blog post was fun for you to read. Share it with your friends if you've learned something new.

And as always, thanks for reading!
