---
layout: post
title:  "Teclast F7 Plus - Unix/Linux"
date: 2022-01-06 23:58:10 +0200
categories: blog 
---
{% highlight bash %}
$ cat HISTORY.md
{% endhighlight %}

I restored my brother's laptop after only 6 months of purchase. Out of factory with windows 10. I decided to open it to be able to restore the bios. The only wise thing to do was not to throw it in the trash. My brother had already done it! Turned off properly the night before. The next day, when turned on, a black screen returned: completely dead! (problem encountered by many users who obviously cursed this purchase). Disconnected the battery I was able to reset the bios and then turn it on. At that point I decided that the best thing was a substantial change of course for this laptop. I decided to install my usual ecosystem. Unix and Linux.

{% highlight bash %}
$ cat TF7P_hardware.md
{% endhighlight %} 

    Full HD IPS display (1920 x 1080px)
    Intel Celeron N4100 CPU (4 cores @ 1.10GHz, Gemini Lake)
    8GB of soldered RAM (not expandable)
    Intel UHD Graphics 600 video card
    256GB HS-SSD-E100N SSD (M.2 SATA)
    Intel Dual Band Wireless-AC 3165 card (no RJ45 port)
    2x USB 3 ports and a backlight US keyboard

{% highlight bash %}
$ cat experiment.md
{% endhighlight %}

## UNIX - OpenBSD/FreeBSD 
I was fascinated by the build quality of the laptop but very skeptical of hardware support being a very low cost laptop. First thing i thought: probable huge problems with bluetooth and wireless. After a short search on the net I found someone who has tried the same thing before me. Reference: [OPENBSD on Teclast F7 PLUS]. At this point it is mandatory to try for yourself.
- First step: OpenBSD installed. The current version is OpenBSD 7.0 released on October 14, 2021.
- Second step: FuguIta (Swiss Army Knife) with small differences in firmware management. Recognition of system peripherals in live with USB boot and second data disk key. No problem.
- Third step: Freebsd 13.
The result was surprising. The hardware support almost completely reflects the evaluations of the cited article.

## Linux
I usually use other distributions on my ideal day. It is known that I don't like systemd centralized but for this test I needed very up-to-date software. I also checked the Teclast's behavior with adapters, extensions, cables, usb2 type external hard drives and type C external sound card. Compulsory comparison for me to do with my very old Asus laptop. 
- I have installed arch linux without problems and in a fully functional way. I've changed some firmware-related boot and kernel options, but I'll talk about that later. Everything works out of the box including energy management.
## Result 
I found a great laptop for myself at a very low cost and suitable for everyday use. Exception for audio. It is really poor. For this reason I have connected a Steinberg UR22 sound card and it seems to work well for my business needs.

## And ?
it is the best wrong buy finished well for me. I can only consider it a great purchase for those who want a cheap laptop to devote to unix/linux. The best solution of 2021 just ended. Highly recommended.

{% highlight bash %}
$ cat README.md
{% endhighlight %} 
To enable proper use of Enhanced SpeedStep in *BSD, the OS selection must be set to Linux in the BIOS.Press the ESC key when the AMI logo appears on the screen. Search on BIOS/Chipset/South Bridge/Operating System Selection. And select Linux.

[OPENBSD on Teclast F7 PLUS]: https://www.tumfatig.net/2020/openbsd-on-teclast-f7-plus/
