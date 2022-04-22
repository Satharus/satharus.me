---
layout: article
title: "Storage Devices: A Smart Trick" 
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
    src: /assets/images/storage-devices/hdd_header.jpg
cover: /assets/images/storage-devices/hdd_square.jpg
---

You go out to the nearest computer store or mall and you buy a brand new fancy 1TB USB 3.0 hard drive with all the cool backup features that no one really uses, and not only until you plug it into your PC that you feel scammed! You find out that it is roughly only 931 “gigabytes” instead of the 1000 or 1024 you were expecting!
<!--more-->

It doesn’t feel good and it really is confusing to most users, the truth is that no one really scammed you and what the manufacturers/distributors are advertising is perfectly legal but not as honest as most of us would like. Most people just don’t know the difference.

### Let's Take a Closer Look

If you look at the cover of the product or at the product itself it will say it is X GB or X TB, here they’re using the prefix Giga(10<sup>9</sup>) or Tera(10<sup>12</sup>), and like any other standard international prefix<sup>[1]</sup> they are represented in base 10(Decimal).

<p style="text-align:center;"><em><span class="_4yxp"><sup>[1]</sup>Quick reminder about the standard international prefixes</span></em></p><p style="text-align:center;"><span class="_4yxp" style="color:#99cc00;">Kilo    = 10<sup>3</sup>  (K) </span></p><p style="text-align:center;"><span class="_4yxp" style="color:#99cc00;">Mega  = 10<sup>6</sup>  (M)</span></p><p style="text-align:center;"><span class="_4yxp" style="color:#99cc00;">Giga   = 10<sup>9</sup>  (G)</span></p><p style="text-align:center;"><span class="_4yxp" style="color:#99cc00;">Tera  = 10<sup>12 </sup>(T)</span></p><p>

So, typically, when you buy a product that is advertised as 1 Terabyte in capacity, it is actually 1000 Gigabytes, which evaluates to the following:

1 TB = 1000 Gigabytes = 1000×1000 Megabytes = 1000<sup>3</sup> Kilobytes = 1000<sup>4</sup> Bytes = 10<sup>12</sup> Bytes

Meanwhile, in the digital world, computers use base 2 (binary).

<p style="text-align:center;">Thus, they use different prefixes. <em>They use the following:</em></p><p><span style="color:#999999;"></span></p><p style="text-align:center;"><span style="color:#99cc00;">Kibi   = 2<span class="_4yxp"><sup>10</sup></span>   (Ki)</span></p><p style="text-align:center;"><span style="color:#99cc00;">Mebi = 2<span class="_4yxp"><sup>20 </sup></span> (Mi)</span></p><p style="text-align:center;"><span style="color:#99cc00;">Gibi   = 2<span class="_4yxp"><sup>30  </sup></span>  (Gi)</span></p><p style="text-align:center;"><span style="color:#99cc00;">Tebi  =  2<span class="_4yxp"><sup>40</sup></span>   (Ti)</span></p>

<section id="a" markdown="1">
So, when you plug that product in, the computer reads its capacity in base 2(binary), not base 10 (decimal). But, remember that your product is 1 _Terabyte_ in capacity not 1 _Tebibyte_.
</section>
So it will evaluate to the following:

<p style="text-align:center;">1 TB = <span class="_4yxp">10<sup>12</sup></span> Bytes</p><p style="text-align:center;"><span class="_4yxp">10<sup>12</sup></span> Bytes = 976,562,500 KiB <span style="color:#ff9900;">(Divide by 1024 to convert Bytes to KibiBytes)</span></p><p style="text-align:center;">976,562,500 KiB = 953,674.32 MiB <span style="color:#ff9900;">(Divide by 1024 to convert KibiBytes to MibiBytes)</span></p><p style="text-align:center;">953,674.32 MiB = 931.32 GiB <span style="color:#ff9900;">(Divide by 1024 to convert MibiBytes to GibiBytes)</span></p>


<section id="" markdown="1">

We find out that indeed, 1 **TeraByte (TB)** = 931.32 **GibiBytes (GiB)**, which is the actual size of the hard drive. Were they to advertise the hard drive as a 1 TiB hard drive, you’d be getting the full 1024 GiB. Sadly this isn’t the case.

The data storage manufacturers aren’t lying here, they’re just kind of exploiting the fact that most people don’t know the difference between a TB and TiB, as we all think in base 10 and are used to the SI prefixes. But, now you know the difference :D!

Thanks for reading this post! I hope you enjoyed it and learnt something.

If you have any suggestions for future posts, make sure to tell me, and don’t forget to share this with your friends if you think that they’ll like it or find it interesting!

</section>
