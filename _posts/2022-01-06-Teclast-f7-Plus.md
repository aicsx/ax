---
layout: post
title:  "Teclast F7 Plus - Unix/Linux"
date: 2022-01-06 23:58:10 +0200
categories: blog 
---
{% highlight bash %}
$ cat HISTORY.md
{% endhighlight %}

I was definitely skeptical. Not so much for the build quality that recalls a first series of mac OS ... but for the hardware specifications. I had a pleasant denial.I had to recover this laptop owned by my brother after 6 months from the purchase. Released from the factory with windows 10 I decided to open it to do bios recovery because afterwards the screen was off after starting. By disconnecting the motherboard battery I was able to hard reset the bios and then turn on the machine. At that point I decided that the best thing was a substantial change of course. I therefore decided to first install unix in various versions and then linux to understand how to recover the laptop for my daily use.

## Hardware - 14 inch laptop. 

    Full HD IPS display (1920 x 1080px)
    Intel Celeron N4100 CPU (4 cores @ 1.10GHz, Gemini Lake)
    8GB of soldered RAM (not expandable)
    Intel UHD Graphics 600 video card
    256GB HS-SSD-E100N SSD (M.2 SATA)
    Intel Dual Band Wireless-AC 3165 card (no RJ45 port)
    2x USB 3 ports and a backlight US keyboard

{% highlight bash %}
$ cat experience.md
{% endhighlight %}

## UNIX - OpenBSD/FreeBSD/FuguITA/NomadBSD 
I was fascinated by the build quality so after researching I relied on what I had been looking for before doing the tests. Starting from my research my reference was: [OPENBSD on Teclast F7 PLUS]
Regarding UNIX: I gladly installed openbsd 7.0. The current version is OpenBSD 7.0. released October 14, 2021. This is the 51st version. Same result with FuguIta (also called Swiss Army Knife of Pork) which is very operational even if with very small differences. I tried and stressed the hard disk also trying with Freebsd 13 and with the respective live nomadbsd. The result was amazing. Different from my expectations.

Hardware support overview

Most of the hardware works but there are two main issues:

    The touchpad doesn’t work.
    Sleep & Suspend half work. The laptop never really suspends (keyboard and power light are still on) and never wakes up. I have to savagely power down the machine using the Power button. (i´m testing solution)


{% highlight bash %}
Audio:            ok  Managed by sndioctl and mixerctl.
Battery status:   ok  Reported by apm.
Ethernet:        n/a  No ethernet port.
Keyboard:       near  Backlight kbd works with Fn+F5.
                      Backlight kbd unavailable in wsconsctl.
                      Volume keys work.
                      Screen backlight fails with Fn keys.
                      Screen backlight works with xbacklight.
Hibernation:      no  ZZZ starts but never achieved.
                      Laptop don’t wake up.
SSD:              ok  HD-SSD-E100N attaches to scsibus.
Suspend / Resume: no  zzz starts but never achieved.
                      Laptop don’t resume.
Touchpad:         no  HTIX5288 not responding.
USB:             yes  Both USB3 ports work.
Video:           yes  Intel UHD 600 attaches to inteldrm(4).
Webcam:          yes  fswebcam(1) can get pictures from it.
Wireless:        yes  Intel AC 3165 attached to iwm(4).
{% endhighlight %}

## And on Linux

I have installed artix & arch linux without problems and in a completely functional way. I've changed some boot and kernel options but I'll talk about that later. In any case it was functional from the first start.

## Result 

I will not repeat the things already said in the cited article. I find it realistic. I found a great support at a very low cost and suitable for everyday use. Except for the audio which is very poor. For this reason I have attached a Steinberg UR22 MKII sound card USB Audio Interface ... recognized as basic and 
seems to work fine for my needs. I will talk about this at another time.

## therefore?

it is the best wrong purchase for me ... but it has posed new questions and solutions to me. I can only consider it my best low cost purchase for those who want a usable full unix / linux laptop. The best solution of 2021 as far as I'm concerned. Highly recommended as long as you are willing to remove the bottom screw panel every time you persist in changing the boot options on linux. Unnecessary thing. I am currently writing about arch current with a Windows base boot option. And everything seems to work the same. PROMOTED!

[OPENBSD on Teclast F7 PLUS]: https://www.tumfatig.net/2020/openbsd-on-teclast-f7-plus/
