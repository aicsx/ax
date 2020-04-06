---
id: 312
title: 'Bootsplash " slackware'
date: 2009-01-20T17:21:16+01:00
author: ax
excerpt: 'How-to che spiega come installare e configurare correttamente il bootsplash con la configurazione "base" nella nostra linuxbox. La distribuzione presa in esame è un Slackware 12.1 con kernel 2.6.24* (kernel default).'
layout: post
guid: http://linuax.wordpress.com/?p=312
permalink: /2009/01/20/bootsplash-grafico-su-slackware-gnulinux/
categories:
  - Boot
  - bootsplash
  - How-to
  - Kernel
  - Linux
  - Slackware
tags:
  - bootsplash
  - How-to
  - Linux
  - script
  - slackware
---
Questo How-to spiega come installare e configurare correttamente il bootsplash nella nostra linuxbox. La distribuzione presa in esame è un Slackware 12.1 con kernel 2.6.24-* (kernel default).

Bootsplash è un’applicazione che permette di visualizzare un’immagine all’avvio della nostra linuxbox nascondendo in modalità "silent" i messaggi di caricamento del kernel. Permette inoltre di avere un’immagine di sfondo nelle console in modalità "verbose", con l’ausilio del framebuffer.

Bisogna tener presente che il progetto: "bootsplash" è stato piano piano emarginato e sostituito da sistemi di customizzazione di shell login grafici differenti.

NOTA (personale, e umile): io ritengo che la vecchia cara patch del progetto bootsplash originale (ossia questa presa in esame nell'how-to) sia sempre il miglior metodo di customizzazione di shell login grafico.

**NB:** procedura valida con tutti i kernel 2.6.x

#

Per prima cosa ci procuriamo i sorgenti del kernel in uso, questo nel caso in cui avessimo deciso di non installarli di default all'installazione della nostra cara slackware. Si può decidere di usare i pacchetti precompilati nel disco slack oppure scaricarli manualmente da <a href="http://www.kernel.org" target="_blank">http://www.kernel.org</a>.

**NB:** Dopo aver scaricato il source del kernel desiderato (nel nostro caso 2.6.24-5) lo scompattiamo nella directory /usr/src/ e creeremo il link simbolico "linux" al kernel di riferimento, esempio:

<pre>~# cp kernel-source-scaricato_versione_2.6.24-5.* /usr/src/
~# cd /usr/src/
~# tar jxvf kernel-source-2.6.24-5.tar.bz2
~# ln -s /usr/src/linux-2.6.24.5/ linux</pre>

Risolta la questione "kernel source" procediamo a scaricare la patch kernel:

<pre>~# cd /usr/src/linux
~# wget <a href="http://www.jaguarlinux.com/pub/patches/bootsplash/linux-2.6/bootsplash-2.6.24.diff" target="_blank">bootsplash-2.6.24.diff.gz</a>
<span style="color:#ff0000;"><strong>
NB:</strong></span> <span style="color:#000000;">i links per reperire le patch sono cambiati; essendo il vecchio sito jaguarlinux non più disponibile. 
Quindi come da segnalazione a fine post di <strong>jpage89</strong> "che ringrazio per l'avviso" le patch adesso sono 
disponibili all'url <a href="http://x-softsi.com.br/site/?p=26#more-26"><strong>QUI</strong></a>.
Il sito linkato per le patch le fornisce già estratte,quindi potete evitare il passaggio successivo
relativo a gunzip<a href="http://x-softsi.com.br/site/?p=26#more-26"><strong></strong></a>.
</span>
</pre>

  * ~# gunzip -d bootsplash-2.6.24.diff.gz

<pre><strong>NOTA:</strong> la patch cambia chiaramente da versione a versione a seconda del kernel scelto.</pre>

**NB:** scaricata e estretta la patch proceremeno quindi ad applicarla al kernel:

<pre>~# patch -p1 &lt; bootsplash-2.6.24.diff</pre>

otterremo un output simile a questo:

<pre>patching file drivers/char/console.c
patching file drivers/char/keyboard.c
patching file drivers/char/n_tty.c
patching file drivers/video/Config.in
patching file drivers/video/Makefile
patching file drivers/video/fbcon-jpegdec.c
patching file drivers/video/fbcon-jpegdec.h
patching file drivers/video/fbcon-splash.c
patching file drivers/video/fbcon-splash.h
patching file drivers/video/fbcon-splash16.c
patching file drivers/video/fbcon.c
patching file include/video/fbcon.h
patching file kernel/panic.c</pre>

Patchato il kernel (accertatevi che non vi restituisca errori), non resta che compilare. Potete utilizzare un config già in uso presente nella directory **/boot/** e copiarlo in **/usr/src/linux/.config** per partire da un configurazione attuale e/o in uso oppure procedere col kernel nudo e crudo. Quindi procediamo con la compilazione del kernel abilitando il supporto al boot grafico e accertandoci che i moduli richiedenti siano attivi in maniera statica, con i tool classici: (menuconfig,xconfig ecc).

<pre>~# make menuconfig</pre>

Modificate queste impostazioni, attivandoli come **statici** (ovvero selezionateli non come moduli):

<pre>Device Drivers -&gt; Block devices -&gt; Initial RAM disk (initrd) support
 Device Drivers -&gt; Graphics support -&gt; support for framebuffer devices
 Device Drivers -&gt; Graphics support -&gt; VESA VGA graphics support
 Device Drivers -&gt; Graphics support -&gt; Console display driver support -&gt; Framebuffer Console support
 Device Drivers -&gt; Graphics support -&gt; Bootsplash Configuration -&gt; Boot splash screen</pre>

**NB:** **Eliminate** invece il boot classico "tux logo":

<pre><strong>Logo Configuration -&gt; BootupLogo</strong></pre>

Modificate le opzioni, salviamo e usciamo.

Prima di compilare possiamo volendo, customizzare la versione kernel in uso modificando **EXTRAVERSION,** esempio:

<pre>~# pico Makefile
<strong>VERSION = 2</strong>
<strong>PATCHLEVEL = 6</strong>
<strong>SUBLEVEL = 24</strong>
<strong>EXTRAVERSION = .5-bootsplash</strong>
<strong>NAME = Err Metey! A Heury Beelge-a Ret!
</strong></pre>

Dopo aver salvato le modifiche procediamo con la compilazione:

<pre>~# make -j5 bzImage
~# make -j5 modules
~# make modules_install</pre>

Compilato il kernel spostiamo il nuovo kernel image e i file di config in _/boot/_.

**NB:** Il tempo necessario alla compilazione e/o ricompilazione kernel è vario a seconda della box in uso "30 minuti come possono essere 3 ore".

<pre>~# mv arch/i386/boot/bzImage /boot/image2624_splash
~# mv System.map /boot/System2624_splash.map
~# mv .config /boot/config2624_splash</pre>

Terminata la parte più "difficile" , procediamo con l'installazione di **_bootsplash-utils_** necessario per creare l'intrd image del nuovo kernel grafico; quindi:

<pre>~$ cd /home/user/
~$ wget <a href="ftp://ftp.tu-chemnitz.de/.SAN0/pub/linux/sunsite.unc-mirror/distributions/zenwalk/i486/zenwalk-5.2/source/a/bootsplash/bootsplash-3.1.tar.bz2">bootsplash-3.1.tar.bz2</a>
~$ tar jxvf bootsplash-3.1.tar.bz2
~$ cd bootsplash-3.1/Utilities
~$ make splash
~$ strip splash</pre>

Provvederemo dopo averlo scompattato e compilato  in una directory esempio, nel nostro caso _/home/user/_ a copiare l'eseguibile in _/sbin/_:

<pre>~$ su -
~# cp splash /sbin/</pre>

Terminato anche questo passaggio, non resta che installare un tema bootsplash grafico sul quale creeremo la nostra immagine initrd grafica.

**NB:** Il pacchetto usato è un mio tema per bootsplash un pò datato ma che pare aver riscosso molto successo. Nel caso non sia abbastanza gradito vi rimando ad altri temi disponibili su <a href="http://www.kde-look.org" target="_blank">http://www.kde-look.org</a> sezione bootsplash.

<pre>~# cd /etc/
~# mkdir bootsplash/ && mkdir bootsplash/themes/
~# cd /etc/bootsplash/themes/
~# wget <a title="Download bootsplash themes" href="http://www.kde-look.org/content/download.php?content=61403&id=1&tan=47897671" target="_blank">Tatto's Girl</a>
~# tar jxvf 61403-girltattoo.tar.bz2</pre>

Estratto il tema nella directory _themes_ procediamo alla creazione dell'initrd image del tema prescelto, in questo caso girltattoo:

<pre>~# /sbin/splash -s -f /etc/bootsplash/themes/girltattoo/config/girltattoo.cfg &gt; /boot/initrd.tattoo</pre>

**NB**: il percorso e il  file di config è relativo al tema prescelto, chiaramente potrebbe variare in base al tema.

Ultima cosa da fare, è la modifica del boot loader, nel nostro esempio lilo:

<pre>~# vi /etc/lilo.conf</pre>

aggiungeremo i link al nuovo kernel da startare all'avvio:

<pre># Kernel image Bootsplash start
image = /boot/image2624_splash
initrd = /boot/initrd.tattoo
append = "splash=verbose"
root = "/dev/hdaX"
label = "SlackSplash"
vga = 791
read-only
# Kernel image Bootsplash end</pre>

**NB** i parametri aggiunti sono:

  * **image** = percorso dell'immagine kernel patchata e compilata precedentemente.
  * **initrd** = percorso immagine initrd grafico creato con _/sbin/splash_.
  * **append** = modalità di avvio bootsplash "verbose" (shell di sfondo senza progress barr).
  * **root** = device root relativo alla propria configurazione di sistema. Sostituire X con /dev/hda1 ad esempio (dove **1** è il device root **su cui è installato il sistema**).
  * **label** = Nome da visualizzare nel menù di lilo.
  * **vga** = risoluzione schermo all'avvio grafico.

Salviamo e restartiamo lilo per rendere permanenti le modifiche:

<pre>~# lilo -v</pre>

Non resta che riavviare e godersi il boot grafico.

\# End