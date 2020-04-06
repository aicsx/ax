---
id: 739
title: 'Console di ripristino linux e recupero del sistema: chroot'
date: 2009-06-01T18:30:30+01:00
author: ax
excerpt: "Ripristino di un sistema GNU/linux su cui sono state effettuate modifiche che ne impediscono il normale avvio attraverso l'uso dei comandi di base dello stesso: mount, chroot, etc."
layout: post
guid: http://linuax.wordpress.com/?p=739
permalink: /2009/06/01/console-di-ripristino-linux-e-recupero-del-sistema-chroot/
image_size:
  - ""
  - ""
  - ""
categories:
  - How-to
  - Linux
  - 'Recovery &amp; Backup'
  - terminal
tags:
  - chroot
  - Linux
  - recovery
  - slackware
  - Tips
---
Chi smanetta continuamente è cosciente del fatto che molto spesso una modifica approssimativa al **fs** "_file system_" e/o una modifica errata di un singolo **modulo** del kernel e/o anche una semplice modifica al gestore di avvio della distribuzione in uso (_**GRUB**_ o _**LILO**_) può impedire il normale avvio e il successivo login di una _**linux box**_.

In linea generale i metodi comuni di ripristino del sistema sono due.:

  1. Inserire il disco di una distribuzione che permette di suo il ripristino partendo dal menù generale del primo avvio: un esempio, può essere _**mandriva**_, che inserisce nel boot manager una scelta sotto il nome di **_RESCUE._**
  2. Utilizzare i metodi _classici_ di gestione e ripristino del sistema. Inserendo un qualsiesi disco di una qualsiesi distribuzione di linux senza preoccuparci troppo di cosa stiamo utilizzando e/o di cosa utilizzavamo precedentemente e utilizzare i comandi che lo stesso linux ci mette a disposizione, tra cui: **_chroot_**.

Il primo metodo risulta così ovvio da evitare di parlarne. Il secondo se pur comune ai più, necessita di qualche accortenza.

Supponiamo di aver messo le mani sul kernel ( il **come** non sarà trattato in questo post ) e di aver accertato che al successivo riavvio il sistema restituisca un bel: **Kernel panic** ...

Senza bestemmiare ( cosa che sarebbe condivisibile alla massa ) inseriamo il disco della prima distribuzione che abbiamo sotto mano e dopo averlo inserito riavviamo manualmente la macchina modificando al successivo riavvio la fase di boot nel **bios**. Chiaramente modificheremo in base alle esigenze la fase di boot dicendogli di leggere prima il disco **cd-rom** e poi di avviarel'**hard disk**.

**NB**: per effettuare il ripristino dobbiamo chiaramente usare come disco di ripristino quello di una distribuzione che **non** abbia un installer grafico all'avvio ma piuttosto una shell testuale. Esempio: slackware, gentoo, una distro live con modalità testuale etc...

Avviato ad esempio il disco di slackware (come esempio) ed avviati i servizzi di base precedenti all'installazione della stessa ci ritroveremo al classico

<pre>login:</pre>

Una volta loggati, basta seguiere pochi semplici passi in ordine:

> _1) Montare le partizioni su cui è presente il fs da recuperare._
> 
> _2) Chrootare le partizioni_ 
> 
> _3) Effettuare le dovute modifiche_ 
> 
> _4) Riavviare_

In specifico alcuni esempi:

**- Montare le partizioni** -

**NB**: supponiamo che il nostro sistema da recuperare sia stato installato come _**/ "root"**_ in **hda1** , la _**/home**_ in **hda3 , _/boot_** in **hda4** .

Quindi per amor di chiarezza l'albero delle partizioni esistenti sarà il seguente:

(_ ****_<span style="color:#000000;"><strong>df -h</strong><em><strong> </strong></em></span>)

<pre>/dev/hda1              12G  8,3G  2,8G  76% /
/dev/hda3              33G   14G   18G  44% /home
/dev/hda4              100m  69m   29m  78% /boot</pre>

Creeremo il mount point di _**/**_ nella directory  di sistema **/mnt/** e quindi da esempio:

<pre>~# mkdir /mnt/linux
~# mount /dev/hda1 /mnt/linux</pre>

**NB**: Volendo effettuare modifiche a **Grub** e/o **Lilo** dovremmo montare anche la partizione di boot dedicata e quindi:

<pre>~# mkdir /mnt/linux/boot
~# mount /dev/hda4 /mnt/linux/boot</pre>

**- Chroot -**

Montate le partizioni provvederemo al chroot:

<pre>~# chroot /mnt/linux /bin/bash</pre>

**NB**: da questo momento ogni modifica sarà effettuata sul sistema come se fossimo effettivamente in esso anzichè da una generica shell di login.

**- Modifiche -** 

Come detto all'inizio avevamo supposto che il problema fosse un **kernel panic** e quindi provvederemo alle modifiche del kernel:

<pre>~# cd /usr/src/linux
~# make menuconfig</pre>

etc...

sistemati i moduli necessari e ricompilato il kernel e copiata la nuova immagine e i relativi config del kernel in **/boot** potremmo aver bisogno di sistemare il boot manager; ( **lilo** ad esempio ):

<pre>~# vi /etc/lilo.conf
image = /boot/vmlinuz_rescue
 root = /dev/hda1
 label = Slackware
 read-only</pre>

Effettuate le dovute modifiche non resterà che riavviare il sistema.

**- Riavvio -** 

<pre>~# exit
~# umount /mnt/linux/boot
~# umount /mnt/linux
~# reboot</pre>

**- Conclusioni ovvie -**

La natura di questo post è solo un esempio _**"****dovuto"**_ dopo una serie di richieste ricevute via e-mail riguardo l'argomento, che può far capire meglio il funzionamento di **chroot** e quale è la sua funzione in materia di ripristino. La grandezza  di Linux da questo punto di vista sta nel concetto di base: _"tutto purchè non largamente danneggiato è recuperabile"_.

#