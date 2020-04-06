---
id: 805
title: 'Compilazione "tradizionale" kernel 2.6.x su Arch linux'
date: 2009-09-12T22:07:50+01:00
author: ax
excerpt: |
  <p>Direttive principali ed esempio di compilazione "tradizionale" di un nuovo kernel serie 2.6.x su Arch Linux.</p>
layout: post
guid: http://linuax.wordpress.com/?p=805
permalink: /2009/09/12/compilazione-tradizionale-kernel-2-6-x-su-arch-linux/
image_url:
  - http://linuax.wordpress.com/files/2009/09/load-config.png
  - http://linuax.wordpress.com/files/2009/09/load-config.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/09/load-config.png" class="alignnone size-full wp-image-806" title="load-config"   alt="load-config"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/09/load-config.png" class="alignnone size-full wp-image-806" title="load-config"   alt="load-config"    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:3:"702";i:1;s:3:"446";i:2;s:1:"3";i:3;s:24:"width="702" height="446"";s:4:"bits";s:2:"16";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"702";i:1;s:3:"446";i:2;s:1:"3";i:3;s:24:"width="702" height="446"";s:4:"bits";s:2:"16";s:4:"mime";s:9:"image/png";}'
categories:
  - Arch
  - How-to
  - Kernel
  - Linux
tags:
  - How-to
  - kernel
  - Linux
  - slackware
---
Nel post sono elencate le direttive di compilazione di un kernel vanilla della serie 2.6.x. Le direttive e i metodi di compilazione restano gli stessi da distro a distro con i comandi che ne conseguono (come già detto in altri post). E' pur vero,che ci sono delle piccole differenze (accorgimenti). Siccome le richieste di aiuto sulla mail  non si fanno mai aspettare, vediamo in specifico come compilare un kernel ufficiale su Arch Linux.

Esempio -  **ARCH** Linux & kernel 2.6.31

**NB:** Il nuovo kernel 2.6.31 introduce parecchie migliorie e modifiche. Se ci fossero dubbi riguardo: compatibilità, modifiche driver,  implementazioni e moduli particolari, etc) si consiglia di consultare il <a href="http://www.kernel.org/pub/linux/kernel/v2.6/ChangeLog-2.6.31" target="_blank">changelog</a>.

Scarichiamo il source dal sito ufficiale:

<pre>~$ wget http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.31.tar.bz2</pre>

**NB**: La directory di posizionamento del kernel-source è _libera_ se **non** c'è espressa richiesta di  ricompilazione futura. Si può scegliere **_/home/utente/_** o **/usr/src/** come da metodologia classica. Nell'esempio tutti i passi saranno effettuati dalla directory di partenza **/home/user/ (~)**.

<span style="color: #ff0000;">IMPORTANTE:</span> E' prassi, specie se non si ha particolare dimestichezza con compilazioni kernel, evitare di cancellare la directory ospitante il kernel source scaricato, una volta finita la compilazione; il tutto al fine di evitare di rifare tutto da capo in caso di non corretto funzionamento. Vige sempre su linux il detto: tutto è recuperabile se non largamente compromesso.

<pre>~$ tar jxvf linux-2.6.31.tar.bz2
~$ cd linux-2.6.31/

Prepariamo l'ambiente:
~/linux-2.6.31 $ make mrproper</pre>

**NB**: Se scegliamo di partire da un .config preesistente copiamolo nella directory del kernel da compilare. Se scegliamo di partire dal config del kernel in uso sulla nostra distribuzione arch eseguire i passi in verde.

<pre><span style="color: #99cc00;">~/linux-2.6.31 $ zcat /proc/config.gz &gt; .config</span>
<span style="color: #99cc00;">~/linux-2.6.31 $ make oldconfig </span></pre>

**NB**: Attualmente arch utilizza il kernel 2.6.30.5 e/o 2.6.30.6 per chi  ha aggiornato. Il richiamo di un config preesistente tra i due kernel di serie diversa potrebbe generare una serie di richieste da parte del sistema: quali moduli implementare e in che modo (Y,N,m). In tal caso, se siete pigri (come me ) e/o vi scoccia ripetere a mano tutte i moduli da implementare, evitate di richiamare il **make oldconfig**. Il richiamo di tale config si effettuerà nella direttiva menuconfig successiva.

<pre>~/linux-2.6.31 $ <span style="color: #99cc00;">zcat /proc/config.gz &gt; .config-arch </span>
~/linux-2.6.31 $ make menuconfig</pre>

Richiamiamo il .config salvato precedentemente.

&nbsp;

**NB**:  E' chiaro che per abilitare alcune delle nuove funzionalità inserite nel kernel 2.6.31 si dovrà poi agire manualmente in base ai casi e scegliere se e cosa aggiungere etc.

<span style="color: #ff0000;">IMPORTANTE</span>: Il richiamo del config preesistente prevede di default la customizzazione  depmod del kernel. In breve: il nostro kernel 2.6.30.5 e/o 2.6.30.6 preesistente in arch è customizzato come 2.6.30-ARCH . E' quindi di fondamentale importanza cambiare il depmod del nuovo kernel prima di effettuare la compilazione. In caso contrario verranno sovrascritti i moduli preesistenti del kernel in uso. Quindi come da esempio, cambiamo il local version;

<pre>General Setup ---&gt;
     (-ARCH)  Local Version</pre>

&nbsp;

Cambieremo il Local Version  con il nome che preferiamo. Esempio:  <span style="color: #0000ff;">extra-ARCH</span> .

**NB**: In Arch Linux il kernel default ha i moduli che fanno riferimento alla directory **_/lib/modules/2.6.30-ARCH_** . Cambiando il <span style="color: #0000ff;">Local Version</span> al nostro nuovo kernel, si creerà una nuova directory in <span style="color: #0000ff;">/lib/modules/2.6.31-extra-ARCH</span> contenente i moduli relativi al nuovo kernel.

Terminata la pre-fase si passa alla compilazione tradizionale:

<pre>~/linux-2.6.31 $ make clean
~/linux-2.6.31 $ make dep
~/linux-2.6.31 $ make bzImage
~/linux-2.6.31 $ make modules
~/linux-2.6.31 $ sudo make modules_install<span style="color: #99cc00;"> depmod 2.6.31-extra-ARCH</span></pre>

Al termine ci verrà mostrato il **depmod** della nuova immagine che sposteremo, rinominandola, nella directory **/boot:**

<pre>~/linux-2.6.31 $ sudo cp arch/x86/boot/bzImage /boot/vmlinuz2631-extra-ARCH</pre>

Spostata l'immagine kernel dovremo provvedere a creare l'initrd (_ramdisk_ image), per il nuovo kernel.

**NB**: Arch utilizza il sistema **mkinitcpio**. Quindi, con depmod precedente alla mano:

<pre>~/linux-2.6.31 $ sudo mkinitcpio -k 2.6.31-extra-ARCH -g /boot/kernel2631-extra-ARCH.img</pre>

Dopo i relativi controlli sugli **HOOKS** (chi non lo sà fa bene a leggersi un minimo su mkinitcpio) opportunamente settati nel file **_/etc/mkinitcpio.conf_**, verrà creata l'immagine init del nostro nuovo kernel.

Non resta che modificare il boot manager; **GRUB** è di default in **ARCH**, come da esempio:

<pre>~$ sudo vi /boot/grub/menu.lst
# (1) Arch Linux 2.6.31
title  Arch Linux 2.6.31                 ---&gt; titolo che comparirà nel menu di grub
root   (hd1,0)                                   ---&gt; device della partizione /boot
kernel /vmlinuz2631-extra-ARCH root=/dev/sdb2 ro ---&gt; nome e percorso immagine kernel + device corrispondente a / "root"
initrd /kernel2631-extra-ARCH.img                ---&gt; nome e percorso ramdisk image creato</pre>

**NB**:  E' un esempio, spero non sia strettamente necessario specificare troppo in dettaglio come si setta grub :>

\# End