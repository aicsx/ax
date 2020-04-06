---
id: 331
title: 'Boot "tux" logo'
date: 2009-01-28T18:36:20+01:00
author: ax
excerpt: |
  Tip per personalizzare l'immagine logo di boot del kernel (il tux logo in alto a sinistra "default")  con un immagine a nostra scelta.
layout: post
guid: http://linuax.wordpress.com/?p=331
permalink: /2009/01/28/personalizzazione-boot-up-logo/
categories:
  - Boot
  - How-to
  - Kernel
  - Linux
  - Slackware
  - terminal
  - Tips
tags:
  - boot
  - logo
  - Tips
  - tux
---
La personalizzazione grafica della shell  è una delle malattie dell'utente linuxiano comune.

Come già visto in altri post, uno dei metodi è l'utilizzo del bootpslash grafico. Diversamente da bootsplash però potrebbe interessare per gli amanti della shell predefinita, cambiare solo il logo di bot; per intenderci il **_tux_** (in alto a sinistra) che all'avvio viene caricato dal kernel, con un immagine personale.

**NB:** prima di cominciare assicuriamoci di avere i sorgenti del kernel installati e il link simbolico alla versione del kernel in uso verso la dir **_/usr/src/linux_**. Nel caso non ci sia basterà quindi creare il link simbolico:

<pre>~# ln -s /usr/src/linux-2.6.x.x /usr/src/linux</pre>

Sistemati link e sorgenti kernel procederemo a salvare la nostra immagine (quella scelta per sostituire il tux logo)  in formato png: con un programma di grafica, esempio: Gimp (o quello che più preferite)

Ci basterà fare il solito "Salva con nome" e selezionare l'estensione giusta. Una volta ottenuta l'immagine _**png**_, click destro e scegliamo "Image -> Mode -> Indexed", naturalmente per la versione italiana sarà una cosa del tipo: "Immagine -> Modalità -> Indicizzata".

**NB**: Scegliamo **224** colori, che e' il numero massimo di colori che il kernel riesce a gestire. Non ci resta che salvarla sempre in formato png.

Ora che avremo l'immagine non resta che convertirla in formato *.ppm . Tale conversione risulta estremamente facile grazie ai tool di default presenti nel sistema. In specifico grazie a: **pngtopnm e pnmtoplainpnm.** La sintassi è:

<pre>~# pngtopnm immagine.png | pnmtoplainpnm &gt; /usr/src/linux/drivers/video/logo/logo_linux_clut224.ppm</pre>

**NB**: _**immagine.png**_ sarà l'immagine scelta da noi precedentemente convertita in formato *.png

Non resta che effettuare le normali operazioni di compilazione del kernel:

<pre>~# make menuconfig</pre>

**NB**: controlliamo prima nel caso non lo avessimo fatto di avere abilitati:

<pre><span style="color:#000000;">Graphics Support -&gt;</span>
<span style="color:#000000;">        [*] Support for frame buffer devices</span>
<span style="color:#000000;">        [*] VESA VGA graphics support</span>

<span style="color:#000000;">    Console display driver support -&gt;</span>
<span style="color:#000000;">        [*] Video mode selection support</span>

<span style="color:#000000;">        &lt;*&gt; Framebuffer Console support</span>

<span style="color:#000000;">        [*]Select compiled-in fonts</span>
<span style="color:#000000;">        [*]VGA 8x16 font</span>

<span style="color:#000000;">    Logo configuration-&gt;</span>
<span style="color:#000000;">        [*]Bootup logo</span>
<span style="color:#000000;">        [*] Standard 224-color Linux logo</span></pre>

Dopo le necessarie verifiche:

<pre>~# make -j5 bzImage
~# make -j5 modules
~# make modules_install</pre>

Completata la compilazione non ci resta che copiare la nuova immagine kernel nella directory _**/boot**_ e modificare lilo stando attenti che sia abilitata la modalità grafica __**vga=791**. Esempio:

<pre>~# cp /usr/src/linux/arch/i386/boot/bzImage /boot/image-linux
~# vi /etc/lilo.conf</pre>

**NB**: decommentiamo la modalità framebuffer scelta e commentiamo le altre:

<pre># VESA framebuffer console @ 1024x768x64k
vga=791
# VESA framebuffer console @ 1024x768x32k
# vga=790
# VESA framebuffer console @ 1024x768x256
# vga=773</pre>

**NB**:  modifichiamo chiaramente il percorso dell'immagine kernel.

<pre>image = /boot/image-linux   # immagine kernel compilata con il nuovo logo
  root = /dev/hdaX          # percorso device root 
  label = Slack
  read-only</pre>

Salvate le modifiche non resta che restartare lilo:

<pre>~# lilo -v</pre>

\# End