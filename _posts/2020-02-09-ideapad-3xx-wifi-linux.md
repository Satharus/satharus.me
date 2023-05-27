---
layout: article
title: "IdeaPad 3XX (Realtek): Fixing the WiFi dropping on Linux" 
categories: [Tech]
author: Ahmed Elmayyah
tags: [Tech, Guide, Hardware]
mode: normal 
header:
  theme: dark
article_header:
  type: cover 
  theme: dark
  background_image: false
# Stick to an image with a height of 700
  image:
    src: /assets/images/ideapad-3xx-series-wifi-linux/pepe_header.jpg
cover: /assets/images/ideapad-3xx-series-wifi-linux/pepe_cover.png
---
The IdeaPad 310 (specifically) is a horrible laptop, the keyboard layout was designed by a person whom I can almost swear has never used a keyboard before and the build is extremely cheap. I mean, come on, who places the shift key right next to the arrow key LIKE THIS.
<!--more-->

{: style="text-align:center"}
![Keyboard](/assets/images/ideapad-3xx-series-wifi-linux/Keyboard.png)

You’re typing like any normal human being and you try to reach the shift key but you end up going to another line because you either pressed the arrow up key or (alongside fn) the PgUp key and ruining the whole page.

It is one of the worst purchases I’ve ever made and I legitimately regret buying it.

Anyhow, let’s get started.


# Finding out Which Chip You Have

This guide was only tested on the Lenovo IdeaPad 310, but it should work on any laptop using any of the WiFi chips listed in this repo. Which are:

- rtl8192ce
- rtl8192cu
- rtl8192se
- rtl8192de
- rtl8188ee
- rtl8192ee
- rtl8723ae
- rtl8723be
- rtl8821ae
- rtl8723de

My specific chip is the rtl8821ae.

You can find out which chip you have by running the following command: 

```sh
$ lspci | grep -i wireless
```

{: style="text-align:center"}
![Chip](/assets/images/ideapad-3xx-series-wifi-linux/Chip.png)


# Installing The Correct Driver

There are 2 options for this:

- First one is installing ANY kernel that works on your laptop that is newer than 4.17.
    - This won’t work if you have the RTL8723DE, follow the repo in that case.
- Use any other version of the kernel but follow the guide in this repo to install the driver.
    - [github.com/lwfinger/rtlwifi_new](https://github.com/lwfinger/rtlwifi_new)

There’s no need to do anything if you already have a kernel newer than 4.17.

You can check so by running `uname -r`.

{: style="text-align:center"}
![kernel](/assets/images/ideapad-3xx-series-wifi-linux/Kernel.png)

{: style="text-align:center"}
btw i use arch

After you install the driver, the WiFi _may_ work depending on the distribution you’re using. In my case it worked but it kept dropping every while. It seemed to be an issue with the firmware according to the hours of research I did before getting this cursed chip to work properly. So, on we go to the fine tuning.

# Loading the Kernel Module Correctly

So, just to get a basic understanding of how drivers work in Linux, there are 2 options: either the driver is built into the kernel itself, or it is loaded as a module on top of it.

That’s a very abstract simplification so don’t @ me.

You can find out information about a kernel module by running the command `modinfo`.

We can run this command to get the list of available parameters for the module: 

```sh
$ modinfo rtl8821ae | grep parm -A2
```

{: style="text-align:center"}
![Params](/assets/images/ideapad-3xx-series-wifi-linux/Params.png)

In my case, the problem was that `fwlps` (Firmware control power save) was set to `1`. This made the chip power down occasionally which caused the connection to drop.

The solution is to set `fwlps` to `0`, this will cause the chip to draw a little more power. I frankly don’t care about that much as the power draw isn’t that much anyway.

Now if you were to load the module manually and pass the parameters to it using `modprobe`, it would work. But that’s too much work every time you boot your system.

That’s why the modprobe.d directory exists. It is a directory where you can add configuration files for your modules.

Now, we can add a config file for our WiFi chip so that the kernel loads it with the specified options automatically.

```sh
# echo 'options rtl8821ae fwlps=0' > /etc/modprobe.d/rtl8821ae.conf
```
This command will make a config file with the firmware power save option set to `0`.

Now you can completely power off your system and then power it on again (don’t reboot) and the WiFi SHOULD work normally.

After doing so, you can run the following command to make sure that the module was loaded with the specified options: 

```sh
# systool -avm rtl8821ae | grep param -iA10
```

{: style="text-align:center"}
![sysvtool](/assets/images/ideapad-3xx-series-wifi-linux/sysvtool.png)

If it still doesn’t work then frankly I don’t know what’s wrong but you can try experimenting with the different options.

Some options on my system made the WiFi work, but it made the laptop unable to wake from sleep properly. So it really differs between a system and another, especially if they’re different models/brands.

You can’t really go wrong with it as long as you remember which configuration did what so you can always go back.

I hope that this was somehow helpful, I tried giving some general info and not just talking about the steps of fixing it.

Thanks for reading!

If you found this useful or interesting, share it with your friends. Especially those who are suffering from the wrath of Lenovo’s cheap laptops.
