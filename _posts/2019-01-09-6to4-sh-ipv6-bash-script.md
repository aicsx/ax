---
id: 1876
title: '6to4.sh - ipv6 bash script'
date: 2019-01-09T08:00:15+01:00
author: ax
excerpt: |
  <p>Script bash aggiornato per la configurazione automatica dell'indirizzo ipv6 via 6to4</p>
layout: post
guid: https://linuax.altervista.org/blog/?p=1876
permalink: /2019/01/09/6to4-sh-ipv6-bash-script/
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'false'
avopt_banners_on_page:
  - 'false'
categories:
  - Bash
  - Ipv6
  - Linux
  - Scripts
  - Slackware
  - terminal
  - Tips
tags:
  - 6to4
  - bash
  - ipv6
  - Linux
  - terminal
---
Semplicissimo script bash aggiornato,semplificato e corretto per la configurazione automatica dell'indirizzo ipv6 via 6to4:

<pre><div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="bash codecolorer">
    <span class="co0"># ax,ax@slackware.eu 6to4 tunnel NAT </span><br />
    <span class="co0"># private test version - contribute &gt; ALL </span><br />
    <span class="co0"># https://bitbucket.org/a1cs/bash/raw/8e333e6099a5df7f46c6cd7203eee59e4891b593/6to4.sh</span><br />
    <br />
    <span class="kw2">ip tunnel</span> del 6to4 <span class="nu0">2</span><span class="sy0">&gt;/</span>dev<span class="sy0">/</span>null<br />
    <span class="kw1">if</span> <span class="br0">&#91;</span> <span class="st0">"<span class="es3">${1}</span>"</span> = <span class="st0">"stop"</span> <span class="br0">&#93;</span>; <span class="kw1">then</span><br />
    &nbsp; <span class="kw3">exit</span><br />
    <span class="kw1">fi</span><br />
    <span class="re2">ip4</span>=<span class="st0">"<span class="es5">`wget -qO- ifconfig.co`</span>"</span>;<br />
    <span class="co0"># Set default route</span><br />
    <span class="re2">relay</span>=192.88.99.1<br />
    <span class="co0"># modify dev var for your kernel. </span><br />
    <span class="re2">localip</span>=$<span class="br0">&#40;</span><span class="kw2">ip</span> <span class="re5">-o</span> addr show dev eth0 <span class="sy0">|</span> <span class="kw2">sed</span> <span class="re5">-e</span><span class="st_h">'s/^.*inet \([^ ]*\)\/.*$/\1/;t;d'</span><span class="br0">&#41;</span><br />
    <br />
    <span class="kw3">echo</span> <span class="re1">$ip4</span> <span class="re1">$localip</span><br />
    <span class="re2">prefix</span>=$<span class="br0">&#40;</span><span class="kw3">printf</span> <span class="st_h">'%02x%02x:%02x%02x\n'</span> $<span class="br0">&#40;</span><span class="kw3">echo</span> <span class="re1">$ip4</span> <span class="sy0">|</span> <span class="kw2">sed</span> <span class="st_h">'s/\./ /g'</span><span class="br0">&#41;</span><span class="br0">&#41;</span><br />
    <span class="kw1">if</span> <span class="br0">&#91;</span> <span class="re5">-z</span> <span class="st0">"<span class="es2">$localip</span>"</span> <span class="br0">&#93;</span>; <span class="kw1">then</span><br />
    &nbsp; <span class="kw2">ip tunnel</span> add 6to4 mode sit ttl <span class="nu0">64</span> remote any <span class="kw3">local</span> <span class="re1">$ip4</span><br />
    <span class="kw1">else</span><br />
    &nbsp; <span class="kw2">ip tunnel</span> add 6to4 mode sit ttl <span class="nu0">64</span> remote any <span class="kw3">local</span> <span class="re1">$localip</span><br />
    <span class="kw1">fi</span><br />
    <span class="kw2">ip link</span> <span class="kw1">set</span> dev 6to4 mtu <span class="nu0">1280</span><br />
    <span class="kw2">ip link</span> <span class="kw1">set</span> dev 6to4 up<br />
    <span class="kw2">ip addr</span> add <span class="nu0">2002</span>:<span class="re1">$prefix</span>::<span class="nu0">1</span><span class="sy0">/</span><span class="nu0">16</span> dev 6to4<br />
    <span class="kw2">ip</span> <span class="re5">-6</span> route add <span class="nu0">2000</span>::<span class="sy0">/</span><span class="nu0">3</span> via ::<span class="re1">$relay</span> dev 6to4 metric <span class="nu0">1</span>
  </div>
</div>

</pre>

\# End