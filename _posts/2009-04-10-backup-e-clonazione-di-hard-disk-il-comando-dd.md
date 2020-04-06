---
id: 602
title: 'Backup e Clonazione di un hard disk; il comando "dd"'
date: 2009-04-10T13:58:22+01:00
author: ax
excerpt: 'dd è un comando che prende un file di input (parametro if) e lo copia, in un file di output (parametro of). La copia attraverso dd avviene bit per bit senza alcuna modifica, il che lo rende un eccezionale strumento di clonazione x=x. '
layout: post
guid: http://linuax.wordpress.com/?p=602
permalink: /2009/04/10/backup-e-clonazione-di-hard-disk-il-comando-dd/
categories:
  - How-to
  - Linux
  - 'Recovery &amp; Backup'
  - terminal
tags:
  - backup
  - dd
  - How-to
  - Linux
---
Molto spesso capita di avere l'esigenza di salvare il proprio sistema allo stato attuale. Un normale amministratore casalingo capisce bene che dopo giorni e giorni di smanettamento sulla propria linux box è buona norma salvare il tutto prima di effettuare pesanti modifiche sul sistema stesso. Questo tipo di processo può avvenire mediante backup comuni e/o anche mediande clonazione dell'hard disk e/o intera partizione. Clonare un hard disk può servire quando si vuole cambiare hd e metterne uno più grande ad esempio, senza dover reinstallare tutto (sistema operativo, programmi, etc..) oppure per fare una copia di backup per mettere al sicuro dati sensibili utilizzabili in un secondo momento.

Sono molti gli applicativi (richiedenti e non richiedenti licenza) disponibili, come ad esempio:

**[Clonezilla](http://www.clonezilla.org/)**: Live CD basato su Linux. Può essere avviato da CD, DVD e USB. Non clona lo spazio vuoto.

**[PC Inspector clone maxx](http://www.pcinspector.de/Sites/clone_maxx/info.htm?language=4)**:  un immagine da montare su floppy. Clona l’intero HD. Ha la possibilità di saltare gli errori. Basato su DOS.

La lista di questi applicativi è ben più lunga; quelli citati sono solo un esempio di come ottenere gli stessi risultati utilizzando applicativi differenti. Esiste un altra strada però "che io personalmente predirigo".

Su linux esiste un comando il cui scopo è lo stesso citato precedentemente e che con qualche accorgimento risulta essere indispensabile nella clonazione e/o messa in sicurezza di un hard disk: **dd** .  **dd** è un comando che prende un file di input (parametro **if**) e lo copia, in un file di output (parametro _**of**_). La copia attraverso **dd** avviene bit per bit senza alcuna modifica, il che lo rende un eccezionale strumento di clonazione x=x.

Per prima cosa è buona norma effettuare la clonazione dell'hard disk partendo da da una distro live su cui collegare un eventuale hard disk esterno che conterrà il backup dei dati.In questo modo avremmo visione sia della partizione da dover clonare che dell'hard disk che ospiterà il backup. Detto questo passiamo in specifico alla sintassi del comando.

**NB**: un corretto uso del comando **dd** prevede i privilegi di **root** user.

<pre>~$ su
Password:
~#</pre>

Supponendo che l’hard disk e/o partizione che vogliamo clonare sia **_<span style="color:#0070c0;">hda1</span>_** e l’hard disk (usb) esterno su cui vogliamo fare la copia sia **_<span style="color:#0070c0;">sda</span>_**, digiteremo:

<pre>~# dd if=/dev/hda1 of=/dev/sda</pre>

Stessa discorso in caso di device differente. Come ad esempio una partizione presente nell'hard disk **HDA** che si chiamerà _**hda2**_ dedita ad essere utilizzata come ricevente del backup del sistema presente su hda1:

<pre>~# dd if=/dev/hda1 of=/dev/hda2</pre>

Per ripristinare la copia del disco basta sostituire il parametro **if** con il parametro **of** e viceversa:

<pre class="sourcecode">~# dd if=/dev/sda of=/dev/hda</pre>

**NB**: Chiaramente le dimensioni dell'hard disk ospite dovranno essere uguali o superiori a quelle dell'hd clonato.

  * **Clonazione dell'HD in file immagine**

Stesso identico risultato è ottenibile nel caso in cui vorremmo clonare l'hard disk in un file immagine:

<pre>~# dd if=/dev/hda1 of=/home/user/file.img</pre>

**NB**: Chiaramente il file deve essere creato su una partizione diversa del disco che si intende clonare.

E' possibile anche spezzare l'immagine creata in più file magari per masterizzare o per superare i limiti imposti ad esempio su partizioni con file system FAT (4 giga) :

<pre>~# dd if=/dev/hda1 of=/home/user/file_01.img bs=1024 count=1000000</pre>

**NB**: Con la sintassi seguente si chiede a **dd** di creare un file da **1GB** scrivendo un'immagine contenente 1000000 blocchi da 1024 byte. Il file successivo continuerà la clonazione del disco dal limite imposto tramite sintassi:

<pre>~# dd if=/dev/hda1 of=/home/user/file_02.img bs=1024 count=1000000 skip=1000000</pre>

**NB**: Tramite il parametro **_skip_** chiederemo a **dd** di saltare i primi 1000000 blocchi prima di iniziare a copiare i dati sul secondo file. Continueremo in questo modo fino a quando l'intero disco verrà copiato. Il risultato finale sarà quello di aver messo al sicuro il nostro hard disk tramite una serie di file immagine _**x.img**_ contenenti l'intero disco senza superare i limiti imposti da noi in base alle esigenze.

**NB**: Se pensiamo che l’hard disk da clonare possa avere qualche errore allora il comando da digitare è:

<pre>~# dd if=/dev/hda1 of=/dev/sda conv=noerror</pre>

**NB**: Con la sintassi _conv=noerror_ si dice a _**dd**_ di continuare il suo lavoro anche dopo la lettura di un errore.

Per altre info vi rimando al man:

<pre>~$ man dd</pre>

\# End

**__**

**_  
_**