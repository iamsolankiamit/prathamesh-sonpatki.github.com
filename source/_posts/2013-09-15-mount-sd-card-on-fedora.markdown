---
layout: post
title: Mount SD Card on Fedora
date: 2013-09-15 14:44
comments: true
categories: fedora raspberry_pie kmod sd-card
---

I attended `Raspberry Pie` meetup this weekend. We had lot of fun
there playing with Pie. After coming home, i wanted to try all those
things on my Pie. But first step is to prepare a image for it.

I decided to use `pidora`, the fedora remix for Raspberry Pie. Fedora
comes with `fedora-arm-installer` package. After installing it on F19
machine and starting it, i inserted SD card and it didn't get
detected.

Without SD card, i couldn't do anything. After googling, i found out
that you have to install `kmod-staging` package. It is available as
part of `rpmfusion` repo.

After that, next part was to find out your driver for SD card reader

``` sh
lsusb | grep Reader
Bus 001 Device 005: ID 0bda:0139 Realtek Semiconductor Corp. RTS5139
Card Reader Controller
```

So it told me that my SD card driver is RTS5139.
Most of the google results also had same driver so i tried next step.

``` sh
modprobe rts5139
```

And i got error that rts5139 does not exist. The issue here is the
`kernel version` for which you install `kmod-staging` matters.
So i needed to install the version of `kmod-version` that matches with
my kernel version. Otherwise it won't work.

``` sh
uname -r
```

gave me `3.9.8-300.fc19.x86_64`

Then

``` sh
yum whatprovides \*/rts5139.ko
```

provided list of all packages having `rts5139`.

I found the appropriate version with my kernel

``` sh
yum install kmod-staging-3.9.8-300.fc19.x86_64-3.9.2-2.fc19.7.x86_64
```

Then

``` sh
modprobe rst5139
```
And it works and SD card gets mounted automatically. :)

Right not `pidora` installation is going on :) I will post more about
my Raspberry Pie experiments soon.





Some links -

[Arduino/Raspberry Pie meetup from Pune](http://www.meetup.com/The-Internet-of-Things/)

[IOT tutorial for the meetup](https://github.com/iot-pune/raspberrypi)
