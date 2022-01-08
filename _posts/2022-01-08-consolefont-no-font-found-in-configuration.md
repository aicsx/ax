---
layout: post
title: "WARNING: consolefont: no font found in configuration"
date: 2022-01-08 00:40:15 +0200
categories: blog
---
As mentioned for a [possible missing firmware] in the previous article I noticed another annoying warning from mkinitcpio always on arch linux. I quote:
<p>&nbsp;</p>
<mark>-> Running build hook: [consolefont]</mark>
<mark>==> WARNING: consolefont: no font found in configuration</mark>
<p>&nbsp;</p>
```bash
~$ cat README.NOTE
# The boring message refers to tty font and key combination [Ctrl+Alt+1/2/3/...] If you are an automatic desktop login user such as Gnome or Kde this article is not necessary for you but I suggest you do the same because sooner or later you might need it. If you are a user with login shell and use a 14 inch laptop, then in this case you should set your font and size to your taste or you may not see basic characters from your shell login.
```
<p>&nbsp;</p>
* login in your terminal (Ctrl+Alt+1/2/3/...)
<p>&nbsp;</p>
* check the default system fonts installed
```bash
~$ cd /usr/share/kbd/consolefonts && ls
161.cp.gz                     lat9-10.psf.gz
162.cp.gz                     lat9-12.psf.gz
163.cp.gz                     lat9-14.psf.gz
164.cp.gz                     lat9-16.psf.gz
```
<p>&nbsp;</p>
* set your consolefont (view preview) 
```bash
~$ setfont lat9u-16.psfu.gz  
```
<p>&nbsp;</p>
**Note: you can add other fonts. Example: terminus-font. Use AUR or otherwise ... it makes no difference. It is a question of "locale" and taste.**
<p>&nbsp;</p>
* check your consolefonts (view all recognized characters)
```bash
~$ showconsolefont 
```
<p>&nbsp;</p>
*  found what you like, it's time to add your chosen font to the system
```bash
~$ vim /etc/vconsole.conf
FONT=lat9u-16 # psfu.gz is the extension and should not be inserted in the config
```
<p>&nbsp;</p>
* final step: rerun kernel imaging
```bash
~$ sudo mkinitcpio -P
```
### No other alert. 

[possible missing firmware]: https://aicsx.github.io/ax/blog/2022/01/07/Possibly-missing-firmware-for-module.html
