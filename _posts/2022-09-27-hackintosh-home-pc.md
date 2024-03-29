---
layout: post
title: "hackintosh: i5 8400 Asus Prime H310M-K R2.0"
date: 2022-09-27 00:26:45 +0200
categories: blog
---
# Hackintosh Asus Prime H310M-K R2.0 i5 8400 coffee lake with macOS Monterey 12.3.1

![About This Mac](https://raw.githubusercontent.com/aicsx/Hackintosh-Asus-Prime-H310M-K-R2.0/main/screenshot/Schermata%202022-04-22%20alle%2015.38.54.png)

**This is personal EFI for the installation of macOS 12.3.1 Monterey on an ASUS H310M-K R2.0 motherboard**

## Hardware Configuration

-Intel Core i5 8400 8th Generation Core\u2122
-Asus Prime H310M-K R2.0
-Realtek\u00ae RTL8111H, 1 x Gigabit LAN
-Realtek\u00ae ALC887 8-Channel High Definition Audio CODEC 
-8GB 2666Mhz Kingston DDR4 RAM
-Crucial BX500 240 GB Serial ATA III Solid State Drive
-500GB Seagate Barracuda ST500DM002 SATA 
-Intel UHD Graphics 630

### Links, version and references

-[Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
-[OpenCore Bootloader v0.8.0](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.0)
-[ProperTree](https://github.com/corpnewt/ProperTree)
-[GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
-Python 3.10.x

#### Note

If like in my case your video card does not have HDMI output please use DVI. VGA causes black lock screen. It is not a tutorial. The steps and things to do are not mentioned. Please read the official Dortania's guide.

-**EFI** is first step **DEBUG** with boot -v enable. Everything works except the screen. It does not turn on again after suspension. 
-**EFI-RELEASE** is final version after tests without boot kernel messages "only apple logo". Rename it in EFI if you want use it. Fix added in config.plist to resume screen after sleep mode.    

##### Disclaimer

This is my personal home project. I have no responsibility for any damage caused by the release of this project. 

### WARNINGS

**Before using EFI remember that it is necessary to generate a serial number with GenSMBIOS for iMac19.1 and apply the serials generated with ProperTree**.

#### Pictures

<img src="https://raw.githubusercontent.com/aicsx/Hackintosh-Asus-Prime-H310M-K-R2.0/main/screenshot/Schermata%202022-04-22%20alle%2022.10.01.png" width="600" height="400">

## Update Monterey 12.4

![Update](https://raw.githubusercontent.com/aicsx/Hackintosh-Asus-Prime-H310M-K-R2.0/main/screenshot/Update.png)

I have updated to the latest version xcode, python and macOS Monterey 12.4. After 24 minutes everything is ok! you can upgrade without additional steps.

## UPDATE2 Monterey at latest SEPTEMBER 2022

I have updated to the latest version of Monterey. NO PROBLEMS WITH APPLE AUTO-UPDATE. IT'S ALL OK.

### (NO) Bugfix 

The latest update enables FaceTime and iMessage. Work fully!
