---
id: 787
title: Ricompilazione Kernel 2.6.x
date: 2009-08-04T07:00:15+01:00
author: ax
excerpt: |
  <p>Direttive principali ed esempio di ricompilazione di un kernel serie 2.6.x precedentemente compilato.</p>
layout: post
guid: http://linuax.wordpress.com/?p=787
permalink: /2009/08/04/ricompilazione-kernel-2-6-x/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Kernel
  - Linux
  - Slackware
tags:
  - How-to
  - kernel
  - Linux
  - module
  - slackware
  - X
---
Di seguito sono elencati con una breve spiegazione i passi necessari per la ricompilazione di un kernel serie 2.6.x precedentemente installato , nello specifico, la distro di riferimento è  Slackware.

**NB**: Indifferentemente dalla distro, i comandi utili risultano essere sempre gli stessi, come da esempio seguente.

In seguito alla compilazione di un nuovo kernel, spesso è necessario provare e riprovare ad effettuare cambiamenti sul kernel in uso. Questo procedimento si chiama **ricompilazione** e risulta semplice, come lo è la compilazione, se si tengono a mente delle piccole attenzioni. Vediamo in dettaglio.

**NB**: Se ci fossero dubbi durante la ricompilazione si consiglia la lettura precedente del post "<a href="http://linuax.altervista.org/blog/2009/08/03/compilazione-nuovo-kernel-2-6-x/" target="_blank">compilazione nuovo kernel 2.6.x</a>". In questo post infatti sono presenti commenti che possono chiarire le idee riguardo ad alcuni punti.

Ci spostiamo nella directory che ospita il kernel 2.6.x precedentemente compilato che regolarmente dovrebbe avere già il link simbolico alla directory _**/usr/src/linux**_:

<pre>~$ su
Password:
~# ls -al /usr/src/linux
lrwxrwxrwx 1 root root 14 2008-09-04 03:38 /usr/src/linux -&gt; linux-2.6.30/</pre>

**NB**: Così facendo saremo certi che il link simbolico sia correttamente settato al kernel in uso da ricompilare.

Accertato che il link e la directory sono corrette. Procediamo con i comandi utili alla ricompilazione e modifica del kernel in uso:

<pre>~# cd /usr/src/linux
~# make mrproper</pre>

Procediamo con la modifica della voci:

<pre>~# make menuconfig</pre>

**NB**: così come per la compilazione, si può scegliere xconfig e/o gconfig.

Terminati i cambiamenti apportati alle voci kernel da abilitare, disabilitare etc... procediamo con la ricompilazione dell anuova immagine kernel:

<pre>~# make -j5 bzImage</pre>

Poi sarà la volta dei moduli:

<pre>~# make -j5 modules</pre>

**NB**: Prima di reinstallare i moduli è buona norma salvare quelli precedenti in un file archivio. Così da evitare di perderli in casi particolari di errore. I moduli sono installati nella directory _**/lib/modules/versione-kernel**_. Procederemo quindi a zipparli e poi continueremo con l'installazione di quelli nuovi come da esempio:

<pre>~# tar cf /lib/modules/2.6.versione.tar /lib/modules/2.6.nome-directory-moduli-in-uso</pre>

**NB**: Questo comando creerà un archivio nella dir /lib/modules/ che si chiamerà 2.6.versione.tar contenente i nostri moduli attuali prima della ricompilazione.

Procediamo quindi con l'installazione dei moduli  attuali:

<pre>~# make modules_install</pre>

Avremo quindi l'immagine kernel e i relativi file .config e System.map da salvare in **/boot/** .

**NB**: Essendo queste modifiche ad un kernel precedentemente compilato è buona norma salvare i seguenti file e spostarli nella directory /boot/ sotto un nome riconoscibile come attuale.

<pre>~# cp /usr/src/linux/arch/i386/boot/bzImage /boot/vmlinuz-2.6.x-new
~# cp /usr/src/linux/.config /boot/config_vmlinuz-2.6.x-new
~# cp /usr/src/linux/System.map /boot/System.map-2.6.x-new</pre>

Non resta che modificare il boot loader "LILO e/o GRUB" aggiungendo il nuovo kernel a quelli già presenti:

<pre>~# vi /etc/lilo.conf

#  Start Config Image kernel 2.6.x ricompilata
  image = /boot/vmlinuz-2.6.x-new                                          =&gt; nome e percorso dell'immagine ricompilata
  root = "/dev/hdaX"                                                       =&gt; device corrispondente alla partizione radice /
  label = "Kernel2.6.x-new"                                                =&gt; nome che apparirà al boot lilo
  vga = 791                                                                =&gt; modalità di framebuffer della console scelto
  read-only
#  End Config Image kernel 2.6.x ricompilata</pre>

Aggiunto il nuovo kernel salviamo e restartiamo lilo in modalità verbose così da controllare eventuali errori:

<pre>~# lilo -v</pre>

Non resta che riavviare e godere delle modifiche fatte al nostro kernel ricompilato.

\# End