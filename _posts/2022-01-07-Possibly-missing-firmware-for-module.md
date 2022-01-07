---

layout: post
title: "WARNING: Possibly missing firmware for module"
date: 2022-01-07 19:58:10 +0200
categories: blog

---

After the recent compilation of the kernel-rt for my Teclast F7 Plus laptop on arch linux I noticed some annoying alerts from mkinitcpio.
<p>&nbsp;</p>
In detail, the alerts are as follows:
<p>&nbsp;</p>
<mark>==> WARNING: Possibly missing firmware for module: aic94xx</mark>
<mark>==> WARNING: Possibly missing firmware for module: wd719x</mark>
<p>&nbsp;</p>
The solution is to simply.  Compile and install the firmware.
```bash
~$ git clone https://aur.archlinux.org/aic94xx-firmware.git
~$ cd aic94xx-firmware
~$ makepkg -sri
```
<p>&nbsp;</p>
```bash
~$ git clone https://aur.archlinux.org/wd719x-firmware.git
~$ cd wd719x-firmware
~$ makepkg -sri
```
### Update kernel image:
```bash
~$ sudo mkinitcpio -p linux
```

### Note:
option -p calls a default preset to be used for building the kernel image. If you have other needs read the documentation about mkinitcpio.

#### Alternative for AUR use:
```bash
~$ yay aic94xx-firmware wd719x-firmware
```
the pkgbuild will do the rest by updating the kernel image after installation.
