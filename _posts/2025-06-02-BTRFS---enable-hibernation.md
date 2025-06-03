---
layout: post
title: "BTRFS - enable hibernation"
date: 2025-06-02 20:31:42 +0200
categories: blog
---

# BTRFS - enable hibernation

> For a number of reasons that I don’t explain, I changed my distribution and opted to use [Arch Linux](https://archlinux.org/) on a BTRFS partition. This choice allows me to have change snapshots in addition to backups. I make it short: I had the need to enable hibernation because I find it very useful especially on a laptop. Often I have to stop work and prefer to find everything as it was before my return. The documentation exists and I consider it dispersive. For this I will limit myself to write small notes for the achievement of the purpose. This is not a how-to!

### Subvolumes 

I created the following subvolumes in the partition: @, @home, @swap, @cache, @log 

> NOTE: This procedure requires the creation of a dedicated sub-volume for swaps. 
>
> if it is not present you can easily create it with the next command **Subvolumes.1** otherwise drop the command

### Subvolumes.1 

```
~# btrfs subvolume create /swap
```

*Tip: Consider creating the subvolume directly below the top-level subvolume, e.g. @swap. Then, make sure the subvolume is mounted to /swap (or any other accessible location).*

### Generate the swapfile

```
~# pacman -S btrfs-progs
~# btrfs filesystem mkswapfile --size <your ram size>G --uuid clear /swap/swapfile
```

> NOTE: To make hibernation work you need to ensure that the swapfile space is equal to your RAM size. 

### Activate the swapfile:

```
~# swapon /swap/swapfile
```

> now you can check that it is up and running. Later you can add the config to fstab to make the change permanent!

```bash
~ 
❯ free -h
               total        used        free      shared  buff/cache   available
Mem:            31Gi       2,8Gi        25Gi       564Mi       3,7Gi        28Gi
Swap:           35Gi          0B        35Gi
```

### Fstab - permanent mount  

```
~# echo "/swap/swapfile none swap defaults 0 0" | tee -a /etc/fstab
```

> the result will be similar to the following

```bash
~ 
❯ cat /etc/fstab
# Static information about the filesystems.
# See fstab(5) for details.

# <file system> <dir> <type> <options> <dump> <pass>
# /dev/nvme0n1p6
UUID=47e97784-1585-46ff-ba9b-e07ec8173824	/         	btrfs     	rw,relatime,ssd,discard=async,space_cache=v2,subvol=/@	0 0

# /dev/nvme0n1p6
UUID=47e97784-1585-46ff-ba9b-e07ec8173824	/home     	btrfs     	rw,relatime,ssd,discard=async,space_cache=v2,subvol=/@home	0 0

# /dev/nvme0n1p6
UUID=47e97784-1585-46ff-ba9b-e07ec8173824	/swap     	btrfs     	rw,relatime,ssd,discard=async,space_cache=v2,subvol=/@swap	0 0

# /dev/nvme0n1p6
UUID=47e97784-1585-46ff-ba9b-e07ec8173824	/var/cache	btrfs     	rw,relatime,ssd,discard=async,space_cache=v2,subvol=/@cache	0 0

# /dev/nvme0n1p6
UUID=47e97784-1585-46ff-ba9b-e07ec8173824	/var/log  	btrfs     	rw,relatime,ssd,discard=async,space_cache=v2,subvol=/@log	0 0

/swap/swapfile none swap defaults 0 0
```

> it's time for REBOOT!

### Edit mkinitcpio.conf

```
~# sed -i 's/\(filesystems\)/\1 resume/' /etc/mkinitcpio.conf
```

> NOTE: the command will add the resume parameter to the HOOKS list. Make sure it looks like the following example. 

```bash
~ 
❯ cat /etc/mkinitcpio.conf | grep "HOOKS=" | tail -n 1      
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block filesystems resume fsck grub-btrfs-overlayfs)
```

If everything has worked properly so far, the last two parameters are missing. I refer to the **UUID** of the root partition and **resume_offset** referring to the swapfile. These parameters will be added in grub to allow the kernel to restore at boot time. 
Let’s move them!

### UUID - root 

```bash
~ 
❯ df -h | grep "/$"
/dev/nvme0n1p6  440G   73G    367G  17% /
```

> NOTE: is an example of my use case. Your partition will be mapped to the correct device, otherwise, worry about checking which is the root partition. 

```bash
~ 
❯ blkid -o value /dev/nvme0n1p6 | head -n1 
47e97784-1585-46ff-ba9b-e07ec8173824
```

**UUID=47e97784-1585-46ff-ba9b-e07ec8173824**

### resume_offset - swapfile

```bash
~ 
❯  sudo btrfs inspect-internal map-swapfile -r /swap/swapfile

[sudo] password di ax: 
12952736
```

**resume_offset=12952736**

### Edit /etc/default/grub

Open /etc/default/grub with vim and append resume=UUID=<your mapped root UUID> resume_offset=<number you got> to the kernel arguments in the string "GRUB_CMDLINE_LINUX_DEFAULT=".  The result will be as follows:

```bash
~ 
❯ cat /etc/default/grub | grep "GRUB_CMDLINE_LINUX_DEFAULT="
GRUB_CMDLINE_LINUX_DEFAULT="resume=UUID=47e97784-1585-46ff-ba9b-e07ec8173824 resume_offset=12952736 loglevel=3 quiet"
```

All that remains is to reload the grub configuration with the correct parameters:

```bash
~ 
❯ sudo grub-mkconfig -o /boot/grub/grub.cfg 
```

### That's it!

---

(END)
