---
layout: post
title: "fix: screen tearing Intel [UHD Graphics 600]"
date: 2022-01-12 23:30:45 +0200
categories: blog
---
As already mentioned in a previous article, I use Teclast F7 Plus on a daily basis. This laptop uses Intel GeminiLake [UHD Graphics 600]. The reference module on Linux is [i915]. I had the displeasure to taste a problem with screen flickering. Sometimes it moves from left to right, as if it tears.
#### What and when it happens?
- Boot completed > "no tearing"
- Display manager > "no tearing"
- Login screen > User selection > "first soft tear"
- Password > "at this point the misalignment of the image is very visible. Hard tear"\
**Fake solution (use case: no time to waste with xorg)**
Close the screen or activate the screen lock. Energy management will do the rest. Reopen the screen, unlock it and there will be no more tear issues until the next reboot. "Used initially for a few days. I had urgent work to do. I didn't have time to devote to the problem. Passable with trick. Until it got boring and I remembered I didn't fix it". 
#### Fix
**NOTE: initramfs and kms steps described for arch (I'm using it on Teclast).**\
Enable early kms:
```bash
~$ sudo vim /etc/mkinitcpio.conf
MODULES=(i915) # ADD i915
```
Build initramfs:
```bash
~$ sudo mkinitcpio -P
```
Xorg configuration:
```bash
~$ sudo vim /etc/X11/xorg.conf.d/20-intel.conf
Section "Device"
	Identifier  "Intel Graphics"
	Driver      "intel"
	Option      "TearFree" "true"
EndSection
```
Reboot:
```bash
~$ sudo reboot
```
#### Resolved
