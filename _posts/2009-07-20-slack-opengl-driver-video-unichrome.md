---
id: 773
title: 'Slack OpenGL driver video UniChrome'
date: 2009-07-20T16:42:50+01:00
author: ax
excerpt: |
  Piccolo how-to d'installazione dei driver proprietari di schede video integrate "VIA S3,Pro IGP, Chrome9" UniChrome  su Slackware e configurazione di Xorg per l'abilitazione Rendering utile alla visualizzazione degli effetti grafici 3D.
layout: post
guid: http://linuax.wordpress.com/?p=773
permalink: /2009/07/20/slackware-xorg-vs-3d-opengl-driver-proprietari-schede-video-unichrome/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Linux
  - Slackware
  - Xorg
tags:
  - 3d
  - driver
  - How-to
  - Linux
  - opengl
  - slackware
  - xorg
---
Nell'ultima settimana sono stato sommerso da richieste di aiuto per la configurazione di **Xorg**.  I problemi principali riguardano nella maggior parte dei casi la compilazione e l'abilitazione del **Rendering** e l'accellerazione grafica su schede video integrate _**"VIA S3,Pro IGP,Chrome9"**_ della **UniChrome** decisamente diffuse su pc _Intel Pentium 4_.

Nella maggior parte dei casi è sufficiente compilare i driver proprietari. Il passo successivo è di dire a Xorg quale modulo usare come driver video, ma come molti utenti lamentano, spesso non si arriva ad una conclusione felice.

Il progetto di riferimento e la documentazione sono disponibili sul sito ufficiale **<a href="http://www.openchrome.org/" target="_blank">OpenChrome</a>**.

**-Compilazione classica**

Scarichiamo via svn l'ultima release disponibile dei driver:

<pre>~$ svn checkout <a href="http://svn.openchrome.org/svn/trunk" target="_blank">http://svn.openchrome.org/svn/trunk</a> openchrome
~$ cd openchrome</pre>

**NB**: Prima di compilare avviamo l'autogen.sh e settiamo l'opzione prefix con la directory corretta di installazione.

<pre>~$ ./autogen.sh --prefix=/usr
~$ make
~$ su
Password:
~# make install
~# make clean</pre>

Compilato e installato il driver, dovremo abilitare l'accellerazione grafica. Per abilitarla è necessario cambiare i driver video in uso ed abilitare **_DRI_** nel file di configurazione di xorg.

**NB**: Se non fosse presente nella home utente, evidentemente non è stato configurato nemmeno Xorg in precedenza. Effettueremo quindi con il nostro editor preferito le modifiche sul file generale _**/etc/X11/xorg.conf**_ "chiaramente da utente root visto che richiede tali permessi" :

<pre>~$ vi /etc/X11/xorg.conf</pre>

ricerchiamo all'interno del file la stringa Driver nella sezione Device:

<pre>Section "Device"
 Identifier  "* Generic VESA compatible"
 <span style="color:#ff0000;">Driver      "vesa"</span>
 #VideoRam    1024
 # Insert Clocks lines here if appropriate
EndSection</pre>

e la modificheremo come da esempio:

<pre>Section "Device"
 Identifier  "* Generic VESA compatible"
 <span style="color:#339966;">Driver      "via"</span>
 #VideoRam    1024
 # Insert Clocks lines here if appropriate
EndSection</pre>

Cambiati i driver video in uso, abilitiamo DRI, aggiugendo al file di configurazione le seguenti stringhe:

<pre>Section "DRI"
 Mode 0666
EndSection

Section "Extensions"
 Option "Composite" "Enable"
EndSection</pre>

Non resta che riavviare il server X.

**NB**: alcuni chipset, come ad esempio "_**VIA Chrome9**_"  possono non riconoscere il modulo via appena compilato. In alternativa è possibile utilizzare un precompilato disponibile sul fedelissimo  <a href="http://www.linuxpackages.net/" target="_blank">linuxpackages</a>, testato e perfettamente funzionante. Non è il massimo, ma può risolvere parecchie rogne, specie per gli utenti meno ferrati in materia.

**-Installazione driver da precompilato**

<pre>~$ wget <a href="http://lp.slackwaresupport.com/Slackware/Slackware-12.1/frias/system/xf86-video-openchrome-0.2.903-i486-1mfb.tgz" target="_blank">xf86-video-openchrome</a>
~$ su
Password:
~# installpkg xf86-video-openchrome-0.2.903-i486-1mfb.tgz</pre>

**NB**: Installato il driver si passano le solite modifiche a Xorg. Questa volta però il precompilato ha come modulo il driver proprietario sotto il nome di "openchrome" e quindi modificheremo come da esempio:

<pre>Section "Device"
 Identifier  "* Generic VESA compatible"
 <span style="color:#339966;">Driver      "openchrome"</span>
 #VideoRam    1024
 # Insert Clocks lines here if appropriate
EndSection

Section "DRI"
 Mode 0666
EndSection

Section "Extensions"
 Option "Composite" "Enable"
EndSection</pre>

Riavviato **Xorg**, ci accertiamo che tutto funzioni come voluto:

<pre>~$ glxinfo | grep rendering                                              
direct rendering: Yes</pre>

\# End
