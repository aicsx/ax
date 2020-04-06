---
id: 1320
title: 'Fbsplash theme: archax 2.0-0'
date: 2010-03-08T00:33:19+01:00
author: ax
excerpt: |
  <p>Seconda release del tema creato da me per fbsplash utility su patch del kernel fbcondecor per ARCHlinux.</p>
layout: post
guid: http://linuax.wordpress.com/?p=1320
permalink: /2010/03/08/fbsplash-theme-archax-2-0-0/
email_notification:
  - "1268004809"
  - "1268004809"
image_size:
  - 'a:6:{i:0;s:4:"1024";i:1;s:3:"768";i:2;s:1:"3";i:3;s:25:"width="1024" height="768"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:4:"1024";i:1;s:3:"768";i:2;s:1:"3";i:3;s:25:"width="1024" height="768"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
image_colors_fg:
  - 'a:11:{i:0;s:6:"404040";s:2:"+1";s:6:"404040";s:2:"+2";s:6:"404040";s:2:"+3";s:6:"404040";s:2:"+4";s:6:"404040";s:2:"+5";s:6:"404040";i:-1;s:6:"404040";i:-2;s:6:"000000";i:-3;s:6:"000000";i:-4;s:6:"ffffff";i:-5;s:6:"ffffff";}'
  - 'a:11:{i:0;s:6:"404040";s:2:"+1";s:6:"404040";s:2:"+2";s:6:"404040";s:2:"+3";s:6:"404040";s:2:"+4";s:6:"404040";s:2:"+5";s:6:"404040";i:-1;s:6:"404040";i:-2;s:6:"000000";i:-3;s:6:"000000";i:-4;s:6:"ffffff";i:-5;s:6:"ffffff";}'
image_colors_bg:
  - 'a:11:{i:0;s:6:"ffffff";s:2:"+1";s:6:"ffffff";s:2:"+2";s:6:"ffffff";s:2:"+3";s:6:"ffffff";s:2:"+4";s:6:"ffffff";s:2:"+5";s:6:"ffffff";i:-1;s:6:"d9d9d9";i:-2;s:6:"bfbfbf";i:-3;s:6:"808080";i:-4;s:6:"404040";i:-5;s:6:"1a1a1a";}'
  - 'a:11:{i:0;s:6:"ffffff";s:2:"+1";s:6:"ffffff";s:2:"+2";s:6:"ffffff";s:2:"+3";s:6:"ffffff";s:2:"+4";s:6:"ffffff";s:2:"+5";s:6:"ffffff";i:-1;s:6:"d9d9d9";i:-2;s:6:"bfbfbf";i:-3;s:6:"808080";i:-4;s:6:"404040";i:-5;s:6:"1a1a1a";}'
image_url:
  - http://linuax.files.wordpress.com/2010/03/archax-1024x768-silent.png
  - http://linuax.files.wordpress.com/2010/03/archax-1024x768-silent.png
image_tag:
  - '<img src="http://linuax.files.wordpress.com/2010/03/archax-1024x768-silent.png" http://linuax.files.wordpress.com/2010/03/archax-1024x768-silent.png  />'
  - '<img src="http://linuax.files.wordpress.com/2010/03/archax-1024x768-silent.png" http://linuax.files.wordpress.com/2010/03/archax-1024x768-silent.png  />'
categories:
  - Arch
  - Boot
  - How-to
  - Linux
tags:
  - Arch
  - Linux
  - theme
  - yaourt
---
Seconda release del tema **archax** per patch fbcondecor del progetto **fbsplash**. Così come nella release precedente i link sono disponibili.

Come di rito in questi casi il **Changelog** include le seguenti modifiche**:**

<pre>Modalità silent:
* Silent bar modificata/colore bianco come da logo di sfondo.
Modalità verbose:
* Risoluzione 1024x768 allargata per una migliore visione  dei caratteri e delle applicazioni in shell.
* Colore dei bordi della shell e sfondo interni resi trasparenti.</pre>

Spero che le modifiche risultino di vostro gradimento, a voi le screen:

<pre><strong>SILENT MODE:</strong></pre>

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-silent.png"><img class="alignnone size-full wp-image-1515" title="archax-1024x768-silent" src="http://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-silent.png" alt="" width="1024" height="768" srcset="https://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-silent.png 1024w, https://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-silent-300x225.png 300w" sizes="(max-width: 1024px) 100vw, 1024px" /></a></pre>

<pre><strong>VERBOSE MODE:</strong>
<a href="http://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-verbose.png"><img class="alignnone size-full wp-image-1517" title="archax-1024x768-verbose" src="http://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-verbose.png" alt="" width="1024" height="768" srcset="https://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-verbose.png 1024w, https://linuax.altervista.org/blog/wp-content/uploads/2011/11/archax-1024x768-verbose-300x225.png 300w" sizes="(max-width: 1024px) 100vw, 1024px" /></a> <strong></strong></pre>

Per motivi di aggiornamento e di policy ho aggiornato il source e il **PKGBUILD** per ARCHlinux direttamente su <a href="http://aur.archlinux.org/packages.php?ID=30314" target="_blank"><strong>AUR</strong></a>. Come sempre il tema è di facile installazione/aggiornamento attraverso **<a href="http://wiki.archlinux.org/index.php/Yaourt" target="_blank">Yaourt</a> :**

<pre>~# yaourt fbsplash-theme-archax</pre>

**NB**: al termine dell'installazione bisogna ovviamente ricreare l'immagine di init del kernel con **mkinitcpio**. Se preferite il vecchio tema con annessi colori non aggiornate altrimenti sovrascriverete il path con il nuovo tema. Se non sapete di cosa si parla, non sapete ricreare l'init nuovo del kernel e/o in ogni caso volete chiarimenti si consiglia di leggere i post precedenti:

  * **<a href="http://linuax.altervista.org/blog/2009/09/15/arch-linux-bootsplash-fbsplash/" target="_blank">ARCHlinux fbsplash </a>**
  * **<a href="http://linuax.altervista.org/blog/2009/09/21/archax-fbsplash-theme/" target="_blank">Archax fbslash theme 1.0-0 </a>**

Se non siete archmunuti ma il tema è di vostro gradimento anche in questo caso è disponibile il link ufficiale aggiornato per kde-look.org. <a href="http://www.kde-look.org/content/show.php/Archax+fbsplash+theme?content=121236" target="_blank"><strong>Archax 2.0-0</strong></a>

\# End