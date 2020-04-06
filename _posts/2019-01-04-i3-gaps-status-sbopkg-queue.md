---
id: 1845
title: 'i3 / gaps / status - SBoPKG queue'
date: 2019-01-04T08:30:15+01:00
author: ax
excerpt: |
  <p>Queue: scarica, compila ed installa nell'ordine prestabilito i pacchetti indicati necessari al funzionamento di i3 con relativa patch gaps</p>
layout: post
guid: https://linuax.altervista.org/blog/?p=1845
permalink: /2019/01/04/i3-gaps-status-sbopkg-queue/
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'false'
avopt_banners_on_page:
  - 'true'
aa_post_meta:
  - 'a:1:{s:5:"where";s:6:"before";}'
categories:
  - i3
  - Linux
  - Slackware
  - Tips
  - Wm
tags:
  - i3
  - Linux
  - queue
  - sbopkg
  - slackware
  - X
---
**Requisiti:**  [SBoPKG](https://sbopkg.org/downloads.php)

<pre>~# wget <a href="https://linuax.altervista.org/blog/wp-content/uploads/2019/01/i3-gaps-status.sqf">i3-gaps-status.sqf</a>
~# cp i3-gaps-status.sqf /var/lib/sbopkg/queues/
~# sbopkg</pre>

  * <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">Q</span></span>ueue 
      * <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">L</span></span>oad > [*] i3-gaps-status 
          * <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">P</span></span>rocess 
              * <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">I</span></span>nstall

**Nota**: Scarica, compila ed installa nell'ordine prestabilito i pacchetti indicati necessari al funzionamento di i3 con relativa patch gaps a patto che nella release di partenza ci sia tutto il necessario. Io parto dal presupposto che avete installato la versione base di Slackware compresa di compilatori e librerie varie. In ogni caso se il processo dovesse interrompersi per colpa di qualche dipendenza non risolta avrete sempre la possibilità di modificare la queue cercando i pacchetti mancanti ed aggiungerli mediante sort nell'ordine che preferite, sempre in sbopkg.

**Metodo 2.** Vi segnate i pacchetti di base e li cercate 1 alla volta in SBoPKG e li aggiungete in queue secondo la vostra sort list e/o aggiungendo e rimuovendo quello che non ritenete necessario. Vi incollo quindi di seguito la mia queue per facilitare la ricerca.

**Nota Bene:** così come da Slackbuild il pacchetto principale richiede solo alcuni dei pkg citati. Ma per ovvi motivi, gli altri sono dipendenze associate e necessarie per il funzionamento generale di tutte le features.

<pre>ax /var/lib/sbopkg/queues $ cat i3-gaps-status.sqf 
<span style="color: #ff0000;">libev</span>
<span style="color: #ff0000;">yajl</span>
<span style="color: #ff0000;">dmenu</span>
xcb-util-xrm
libxkbcommon
json-glib
perl-AnyEvent
perl-Canary-Stability
perl-AnyEvent-HTTP
perl-common-sense
perl-JSON
perl-JSON-MaybeXS
perl-Types-Serialiser
perl-JSON-XS
confuse
i3status
<span style="color: #ff0000;">i3</span>
i3-gaps</pre>

Al termine non resta che selezionare l'init del vostro Tiling wm preferito sempre tramite comodissimo tool che ci viene fornito in gentile concessione da Slackware:

<pre>~$ xwmconfig</pre>

  * <pre> <span style="color: #ff0000;">x</span>initrc.i3</pre>

**Metodo 2.** ve lo modificate a mano. Esempio:

<pre><div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="bash codecolorer">
    <span class="co0">#!/bin/sh</span><br />
    <br />
    <span class="re2">userresources</span>=<span class="re1">$HOME</span><span class="sy0">/</span>.Xresources<br />
    <span class="re2">usermodmap</span>=<span class="re1">$HOME</span><span class="sy0">/</span>.Xmodmap<br />
    <span class="re2">sysresources</span>=<span class="sy0">/</span>etc<span class="sy0">/</span>X11<span class="sy0">/</span>xinit<span class="sy0">/</span>.Xresources<br />
    <span class="re2">sysmodmap</span>=<span class="sy0">/</span>etc<span class="sy0">/</span>X11<span class="sy0">/</span>xinit<span class="sy0">/</span>.Xmodmap<br />
    <br />
    <span class="co0"># Merge in defaults and keymaps</span><br />
    <span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$sysresources</span> <span class="br0">&#93;</span> <span class="sy0">&&</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span>xrdb <span class="re5">-merge</span> <span class="re1">$sysresources</span><br />
    <span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$sysmodmap</span> <span class="br0">&#93;</span> <span class="sy0">&&</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span><span class="kw2">xmodmap</span> <span class="re1">$sysmodmap</span><br />
    <span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$userresources</span> <span class="br0">&#93;</span> <span class="sy0">&&</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span>xrdb <span class="re5">-merge</span> <span class="re1">$userresources</span><br />
    <span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$usermodmap</span> <span class="br0">&#93;</span> <span class="sy0">&&</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span><span class="kw2">xmodmap</span> <span class="re1">$usermodmap</span><br />
    <br />
    <span class="co0"># Start i3</span><br />
    <span class="kw1">if</span> <span class="br0">&#91;</span> <span class="re5">-z</span> <span class="st0">"<span class="es2">$DESKTOP_SESSION</span>"</span> <span class="re5">-a</span> <span class="re5">-x</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span>ck-launch-session <span class="br0">&#93;</span>; <span class="kw1">then</span><br />
    &nbsp; &nbsp; <span class="kw3">exec</span> ck-launch-session dbus-launch <span class="re5">--exit-with-session</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span>i3<br />
    <span class="kw1">else</span><br />
    &nbsp; &nbsp; <span class="kw3">exec</span> <span class="sy0">/</span>usr<span class="sy0">/</span>bin<span class="sy0">/</span>i3<br />
    <span class="kw1">fi</span>
  </div>
</div>

</pre>

<pre>~$ startx</pre>

\# End