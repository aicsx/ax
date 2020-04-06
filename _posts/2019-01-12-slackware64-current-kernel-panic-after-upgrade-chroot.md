---
id: 1925
title: 'Slackware64-current kernel panic after upgrade - chroot!'
date: 2019-01-12T07:00:08+01:00
author: ax
excerpt: |
  <p>Processo sintetico di ripristino kernel panic su ramo current mediante chroot.</p>
layout: post
guid: https://linuax.altervista.org/blog/?p=1925
permalink: /2019/01/12/slackware64-current-kernel-panic-after-upgrade-chroot/
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'false'
avopt_banners_on_page:
  - 'false'
categories:
  - Bash
  - Boot
  - How-to
  - Linux
  - 'Recovery &amp; Backup'
  - Slackware
  - terminal
  - Tips
tags:
  - chroot
  - fix
  - issue
  - kernel
  - kernel panic
  - slackpkg
  - upgrade
---
Come molti anni fa, per una semplice svista, dimenticanza, chiamatela come volete ... mi sono trovato a dover ripristinare il boot sulla mia slackbox current dopo l'upgrade generale compreso di kernel, moduli e source. In realtà è una cosa molto comune, la maggior parte delle richieste di supporto proviene sempre da un errore banale che genera un kernel panic al successivo riavvio.

Nel mio caso la svista è stata molto più che banale, decisamente da "babbo" perchè stavo facendo troppe cose contemporaneamente in tty1-2-3 compreso: "codare il mio prossimo progettino in C". Motivo per cui ho aggiornato il kernel e i pkg relativi al changelog del 10/01/2019 ed ho dimenticato di  ricreare initrd e linkare il kernel appena aggiornato. E siccome il vecchio post su chroot di qualche anno fa risulta abbastanza datato o se preferite anche parecchio approssimativo vi scrivo in maniera veloce come ripristinare l'initrd relativo al kernel aggiornato per evitare che abbiate anche voi la sorpresa... questa volta essendo fresca fresca sarà utile sicuramente per altri 10 anni ... olè!!!

**Nota**: parto dal presupposto e do per scontato che siamo in possesso di  un cd di installazione di slackware 14.2. Avviamo il cd, scegliamo il keymap "IT", login: root, e si comincia.

<pre>1. ~# cd mnt
2. ~# mkdir Slackware
3. ~# mount -t ext4 /dev/sda2 /mnt/Slackware
4. ~# mount -o bind /proc /mnt/Slackware/proc
5. ~# mount -o bind /dev /mnt/Slackware/dev
6. ~# mount -o bind /sys /mnt/Slackware/sys
7. ~# choot /mnt/Slackware /bin/bash

slackware-chroot #</pre>

**Nota bene**: nell'esempio riportato io monto solo la root dir / , se supponiamo di avere un sistema con /boot oppure /home oppure /var separate in partizioni differenti, andranno opportunamente montate con il FS dedicato ad esse prima di effettuare i links a proc e dev. Quindi ovviamente aggiungerete dopo il passaggio 3. le altre partizioni come da esempio:

<pre>3. Quello di sopra ...

~# mount -t ext3 /dev/sda3 /mnt/Slackware/boot
~# mount -t ext4 /dev/sda4 /mnt/Slackware/home/

4. procedo con il resto ... come sopra.</pre>

Completato il chroot siamo nella nostra slackbox così come l'avevamo lasciata. Se avete per qualche ragione fatto il mio stesso errore di distrazione, dovrete ricreare l'initrd ed assicurarvi che i links siano corretti come da esempio. A tal proposito possiamo sfruttare l'utility dedicata che ci risparmia tempo e salute.

<pre>~# /usr/share/mkinitrd/mkinitrd_command_generator.sh -l /boot/vmlinuz-generic-4.19.14</pre>

L'utility genererà automaticamente la stringa da utilizzare per il kernel appena installato... fa sempre bene riscriverla in ogni caso:

<pre>~# mkinitrd -c -k 4.19.14 -f ext4 -r /dev/sda2 -m jbd2:mbcache:crc32c-intel:ext4 -u -o /boot/initrd.gz</pre>

**Nota ovvia**: attenzione alla partizione root, questo è solo un esempio.

A questo punto non ci resta che verificare ed eventualmente modificare il boot loader. Lilo nel mio caso:

<pre>~# vim /etc/lilo.conf
# End LILO global section
# Linux bootable partition config begins 
image = /boot/vmlinuz-generic-4.19.14
  initrd = /boot/initrd.gz
  root = /dev/sda2
  label = Linux
  read-only
# Linux bootable partition config ends</pre>

Non ci resta che rivviare lilo per verificare che non ci siano errori. Fatto questo smontiamo tutte le partizoni montate per il chroot e ravviamo.

<pre>slackware-chroot # lilo -v
...
...

slackware-chroot # exit
~# umount -l /mnt/Slackware/sys
~# umount -l /mnt/Slackware/dev
~# umount -l /mnt/Slackware/proc
~# umount /mnt/Slackware</pre>

**Nota**: se ci sono altre partizioni montate vanno chiaramente smontate!

<pre>~# reboot</pre>

\# END