---
id: 326
title: Immagine bootsplash su tutti i terminali di shell.
date: 2009-01-21T13:27:09+01:00
author: ax
excerpt: Piccolo tip per abilitare l’immagine di sfondo bootsplash a tutte le console.
layout: post
guid: http://linuax.wordpress.com/?p=326
permalink: /2009/01/21/immagine-bootsplash-su-tutti-i-terminali-di-shell/
categories:
  - Boot
  - bootsplash
  - Linux
  - Slackware
  - Tips
tags:
  - bootsplash
  - How-to
  - Linux
  - slackware
  - Tips
---
A chi non è capitato di spostarsi da una shell all'altra con i stasti "F2-F3-F4 etc..." ed accorgersi che il bootsplash è abilitato di default solo sul login shell predefinito.

Abilitare il bootsplash su tutti i terminali di shell è molto semplice. Basta modificare il file:

<pre>/etc/rc.d/rc.local</pre>

e aggiungere queste semplici stringhe:

<pre># Bootsplash
for i in 1 2 3 4 5 6 ;do
/sbin/splash -n -s -u $i /etc/bootsplash/themes/girltattoo/config/girltattoo.cfg &gt; /dev/null
done</pre>

**NB:** Ovviamente il percorso del file **"configurazione.cfg"** sarà relativo al tema prescelto.

#