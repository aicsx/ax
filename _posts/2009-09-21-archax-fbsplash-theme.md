---
id: 829
title: Archax fbsplash theme
date: 2009-09-21T07:00:27+01:00
author: ax
excerpt: Tema per fbsplash/fbcondecor.
layout: post
guid: http://linuax.wordpress.com/?p=829
permalink: /2009/09/21/archax-fbsplash-theme/
image_url:
  - http://linuax.wordpress.com/files/2009/09/silent-1024x768.jpg
  - http://linuax.wordpress.com/files/2009/09/silent-1024x768.jpg
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/09/silent-1024x768.jpg" class="alignnone size-medium wp-image-830" title="silent-1024x768_archax"   alt="silent-1024x768_archax"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/09/silent-1024x768.jpg" class="alignnone size-medium wp-image-830" title="silent-1024x768_archax"   alt="silent-1024x768_archax"    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:7:{i:0;s:4:"1024";i:1;s:3:"768";i:2;s:1:"2";i:3;s:25:"width="1024" height="768"";s:4:"bits";s:1:"8";s:8:"channels";s:1:"3";s:4:"mime";s:10:"image/jpeg";}'
  - 'a:7:{i:0;s:4:"1024";i:1;s:3:"768";i:2;s:1:"2";i:3;s:25:"width="1024" height="768"";s:4:"bits";s:1:"8";s:8:"channels";s:1:"3";s:4:"mime";s:10:"image/jpeg";}'
categories:
  - Arch
  - Boot
  - Linux
tags:
  - Arch
  - archax
  - boot
  - bootsplash
  - fbsplash
  - theme
---
Ebbene si, il primo pacchetto creato da me per il repository **AUR** di **arch**linux non è un kernel, non è una patch, ma semplicemente un tema grafico minimalistico (come piacciono a me) per fbsplash e/o fbcondecor :) .

Il pacchetto si chiama:

<a href="http://aur.archlinux.org/packages.php?ID=30314" target="_blank"><strong>fbsplash-theme-arcax</strong></a> ed è disponibile su **<a href="http://aur.archlinux.org/index.php?setlang=it" target="_blank">AUR</a>** appunto, alla voce di ricerca nome utente:_ **"aics"**_ visto che _(ax)_ non era consentito dalle policy del request users.

**NB**: La risoluzione è **1024x768** in modalità **vga=792**.

Screen config:

<pre><img class="alignnone size-medium wp-image-830" title="silent-1024x768_archax" src="http://linuax.files.wordpress.com/2009/09/silent-1024x768.jpg?w=300" alt="silent-1024x768_archax" width="300" height="225" />
<strong>Modalità silent senza barra di avanzamento</strong>

<img class="alignnone size-medium wp-image-831" title="verbose-1024x768_archax" src="http://linuax.files.wordpress.com/2009/09/verbose-1024x768.jpg?w=300" alt="verbose-1024x768_archax" width="300" height="225" />
<strong>Modalità verbose senza box </strong>

<strong>Risultato finale:</strong>
<img class="alignnone size-full wp-image-833" title="silent-archax" src="http://linuax.files.wordpress.com/2009/09/silent-archax.png" alt="silent-archax" width="455" height="341" />

<img class="alignnone size-full wp-image-832" title="verbose-archax" src="http://linuax.files.wordpress.com/2009/09/verbose-archax.png" alt="verbose-archax" width="455" height="341" /></pre>

Installazione diretta tramite <a href="http://wiki.archlinux.org/index.php/Yaourt_%28Italiano%29" target="_blank"><strong>Yaourt</strong></a> su Archlinux:

<pre>~# yaourt -S fbsplash-theme-archax</pre>

Se il pacchetto vi piace, potete votarlo tramite <a href="http://aur.archlinux.org/packages.php?ID=9845" target="_blank"><strong>aurvote</strong></a> per far si che passi nel repository ufficiale. Grazie in anticipo.

**NB**: prevede una configurazione minima dopo l'installazione attraverso il file ~/.aurvote come da esempio seguente:

<pre>~# yaourt -S aurvote
~# exit
~$ vi .aurvote
user=vostro_username
password=vostra_password</pre>

Per chi non fosse **arch**munito c'è cmq il link disponibile di **kde-look**: <a href="http://www.kde-look.org/content/show.php/ARCHax?content=112346" target="_blank">Download</a>

**NB**: tutti i temi fbsplash possono essere convertiti per essere utilizzati con la vecchia patch **Bootsplash** classica e viceversa. Per dubbi consultare la documentazione nell'apposita sezione <a href="http://linuax.wordpress.com/category/boot/" target="_blank"><em><strong>BOOT</strong></em> </a> del blog.

\# End