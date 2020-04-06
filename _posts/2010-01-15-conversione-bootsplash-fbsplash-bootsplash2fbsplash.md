---
id: 994
title: 'Conversione tema Bootsplash in Fbsplash: bootsplash2fbsplash'
date: 2010-01-15T07:00:03+01:00
author: ax
excerpt: |
  <p>Dettagli e procedura da seguire per convertire un tema pensato per la "vecchia" patch Bootsplash in un tema per fbsplash attraverso l'ausilio dell'utility: bootsplash2fbsplash.</p>
layout: post
guid: http://linuax.wordpress.com/?p=994
permalink: /2010/01/15/conversione-tema-bootsplash-in-fbsplash-bootsplash2fbsplash/
categories:
  - Arch
  - Boot
  - bootsplash
  - How-to
  - Linux
  - Slackware
  - terminal
  - Tips
tags:
  - Arch
  - boot
  - bootsplash
  - fbsplash
  - How-to
  - Linux
  - Tips
---
Non troppo tempo fà sono stati publicati su questo blog gli articoli relativi ad alcuni temi per Bootsplash ed un nuovo tema dedicato per Arch Linux (questa volta per **fbsplash**). Ultimamente un utente della comunità di <a href="http://www.kde-look.org" target="_blank">kde-look </a> mi ha fatto notare _(come già sapevo)_ che i temi publicati da me per Bootsplash non sono compatibili con **fbsplash** e/o **gensplash**, di conseguenza non si possono utilizzare con tale splash grafico, chiedendomi di riconfigurare tutto il path relativo i config e i percorsi delle immagini <a href="http://www.kde-look.org/content/show.php/Tattoo%27s+Girl?content=61403" target="_blank">(per chi volesse seguire la discussione originale mediante commenti)</a>. In realtà però questa cosa non è necessaria. Difatti l'installazione di **fbsplash** prevede l'automatica installazione di un utility creata appositamente per convertire un tema creato per la vecchia patch Bootsplash in un tema per il più moderno fbsplash: **bootsplash2fbsplash**.

**NB**: siccome la discussione è nata con un tema specifico **"tattoo's girl"**, vediamo di seguito come convertirlo da bootsplash a **Fbsplash** con pochi e semplici passaggi.

Per prima cosa scarichiamo il tema per Bootsplash e lo scompattiamo in una directory temporanea, come da esempio anche la home utente va bene:

<pre>~$ wget <strong><a href="http://www.kde-look.org/CONTENT/content-files/61403-girltattoo.tar.bz2" target="_blank">girltattoo</a></strong>
~$ tar jxvf 61403-girltattoo.tar.bz2</pre>

A questo punto creeremo il path _"percorso"_ nel sistema che si riferiva al vecchio utilizzo di Bootsplash come se lo stessimo utilizzando come da esempio:

<pre>~$ su -
Passowrd:

~# mkdir /etc/bootsplash</pre>

**NB**: chiaramente discorso identico per chi preferisce l'utilizzo di sudo.

A questo punto copiamo il nostro tema nella directory creata che risultava essere quella di riferimento per la vecchia patch Bootsplash:

<pre>~# cp -r girltattoo/ /etc/bootsplash/</pre>

**NB**: come descritto nel post relativo per <a href="http://linuax.altervista.org/blog/2009/01/20/bootsplash-grafico-su-slackware-gnulinux/" target="_blank">Bootsplash su Slackware</a> ****in realtà la directory di riferimento dei temi risultava essere **/etc/bootsplash/themes/** per comodità. L'utility **bootsplash2fbsplash** di conversione di cui si parla però utilizza un percorso preimpostato che linka verso **/etc/bootsplash.** Motivo per cui molti temi necessitano di un ulteriore semplice modifica anche nel config. Quindi come da esempio:

<pre>~# vi /etc/bootsplash/girltattoo/config/girltattoo.cfg</pre>

sarà modificata la sezione:

<pre><span style="color: #ff0000;"><span style="color: #000000;"># name of the picture file (full path recommended)</span> <span style="color: #990000;">jpeg=/etc/bootsplash/themes/girltattoo/images/verbose.jpg silentjpeg=/etc/bootsplash/themes/girltattoo/images/silent.jpg</span></span></pre>

che diventerà come da esempio:

<pre><span style="color: #99cc00;"><span style="color: #000000;"># name of the picture file (full path recommended)</span> <span style="color: #8cbb00;">jpeg=/etc/bootsplash/girltattoo/images/verbose.jpg silentjpeg=/etc/bootsplash/girltattoo/images/silent.jpg</span></span></pre>

**NB**: Salvati i cambiamenti con il nostro editor preferito, dovremo ancora una volta modificare il nome del file di config. Questa operazione, è necessaria visto che come accadeva precedentemente, l'utility **bootsplash2fbsplash** per riconoscere i file config di tema ricerca un nome default che è _**(bootsplash)**_ seguito dalla risoluzione prescelta, nel nostro caso **(-1024x768)**; quindi come da esempio rinominiamo il file config del tema:

<pre>~# mv /etc/bootsplash/girltattoo/config/girltattoo.cfg /etc/bootsplash/girltattoo/config/bootsplash-1024x768.cfg</pre>

A questo punto non resta che lanciare l'utility che convertirà il tema, dopo aver avuto le accortenze prescritte fino ad ora:

<pre><span style="color: #000000;">~# bootsplash2fbsplash girltattoo </span>
<span style="color: #000000;">o Parsed bootsplash-1024x768.cfg (1024x768</span>)</pre>

**NOTA GENERALE**: la conversione di un tema da Bootsplash a Fbsplash non comporta cambiamenti di nessun tipo sulle immagini di base utilizzate dal tema. In specifico, il nome comprensivo di risoluzione è necessario all'utility per riconoscere il tema e non per effettuare cambiamenti di risoluzione sullo stesso.

**NB**: l'output ci notifica che è stato convertito il tema avente tale risoluzione. E' l'utility stessa che si preoccuperà di trasferire il tema convertito nella directory di riferimento di **fbsplash**. Per verificare infatti:

<pre>~# ls -al /etc/splash/girltattoo/
-rw-r--r-- 1 root root 1210 14 gen 20:06 1024x768.cfg
drwxr-xr-x 2 root root 4096 14 gen 18:57 <span style="color: #0000ff;">images</span></pre>

**NB**: da notare che non solo sono state create directory e file ma sono cambiati anche i path degli stessi per essere utilizzati da fbsplash. Un banale _cat_ infatti ci farà capire meglio:

<pre>~# cat /etc/splash/girltattoo/1024x768.cfg

...
# name of the picture file (full path recommended)
pic=/etc/splash/girltattoo/images/verbose-1024x768.jpg
silentpic=/etc/splash/girltattoo/images/silent-1024x768.jpg
...</pre>

A questo punto non resta che utilizzare **fbsplash** in modo standard ricreando l'immagine init del kernel con l'immagine splash scelta. <a href="http://linuax.altervista.org/blog/2009/09/15/arch-linux-bootsplash-fbsplash/" target="_blank">Esempio su Arch</a> _(parte relativa agli HOOKS e mkinitcpio). Stesso discorso teorico riguardo ad un immagine init per altre distribuzioni)._ Ovviamente al termine di queste operazioni bisogna aggiornare il boot manager (Lilo e/o GRUB).

#  End