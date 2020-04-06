---
id: 1882
title: 'Randomized ipv6 delegation verify'
date: 2019-01-11T07:00:26+01:00
author: ax
excerpt: |
  <p>Esempio di script bash per assegnare ad un customer delega ipv6 partendo da un prefisso statico ipv6.</p>
layout: post
guid: https://linuax.altervista.org/blog/?p=1882
permalink: /2019/01/11/randomized-ipv6-delegation-with-assign-verify-ra-ipv6-verifier-sh/
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'false'
avopt_banners_on_page:
  - 'true'
categories:
  - Ipv6
  - Linux
  - Scripts
  - Slackware
  - terminal
  - Tips
tags:
  - bash
  - delegation
  - ipv6
  - script
  - tb
  - tunnel
---
Questo è un esempio di script bash per assegnare una delega ipv6 partendo da un prefisso statico ipv6. Si può utilizzare con openvpn, in modo classico o come vi pare!

Nella fattispecie questo è lo script che avevo codato ed utilizzato per l'assegnazione delle classi private ai customer del TB. Dal momento che il tb non c'è più e se c'è io non ne faccio più parte (se ve lo state chiedendo non è importante come e perchè ma il post) e dal momento che me lo chiedono in tanti: eccovi serviti. Pulito "abbastanza...non molto a dire il vero, per come la vedo io", semplice "sicuramente", efficace "anche!".

<pre><div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;height:550px;">
  <div class="bash codecolorer">
    <span class="co0">#!/bin/bash</span><br />
    <span class="co0"># ra-ipv6-verifier.sh</span><br />
    <span class="co0"># https://bitbucket.org/a1cs/bash/raw/8e333e6099a5df7f46c6cd7203eee59e4891b593/ra-ipv6-verifier.sh</span><br />
    <span class="co0"># ax - ax[at]slackware.eu </span><br />
    <span class="co0"># release 1.0p1 </span><br />
    <br />
    <span class="kw3">echo</span><br />
    <span class="kw3">echo</span> <span class="st0">"# By ax &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;#"</span> <br />
    <span class="kw3">echo</span> <span class="st0">"# Sa. for &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; IPV6 TB#"</span><br />
    <span class="kw3">echo</span> <span class="st0">"# Cp. by&nbsp; &nbsp; ax - ax@slackware.eu&nbsp; &nbsp;#"</span><br />
    <span class="kw3">echo</span><br />
    <br />
    <br />
    <span class="co0"># ISTRUZIONI:</span><br />
    <span class="co0"># Ricordati che per rendere operativo il processo di verifica </span><br />
    <span class="co0"># tutti i file .sh generati devono essere spostati nella cartella /var/Deleghe.</span><br />
    <span class="co0"># IMPORTANTE: La cartella deve avere i permessi di lettura per gruppo users!</span><br />
    <span class="co0"># Setta la variabile per cambiare la cartella di destinazione.</span><br />
    <br />
    <span class="co0"># Lo script genera di base classi ::/80 partendo da un prefisso stabilito</span><br />
    <span class="co0"># Modificare i parametri commentati successivi per cambiare il comportamento dello script!</span><br />
    <br />
    <span class="re2">array</span>=<span class="br0">&#40;</span> <span class="nu0">1</span> <span class="nu0">2</span> <span class="nu0">3</span> <span class="nu0">4</span> <span class="nu0">5</span> <span class="nu0">6</span> <span class="nu0">7</span> <span class="nu0">8</span> <span class="nu0">9</span> <span class="nu0"></span> a b c d e f <span class="br0">&#41;</span> <span class="co0"># caratteri contenuti in indirizzi ipv6</span><br />
    <span class="re2">MAXCOUNT</span>=<span class="nu0">1</span> <span class="co0"># numero di ipv6 da generare alla fine</span><br />
    <span class="re2">count</span>=<span class="nu0">1</span><br />
    <span class="re2">network</span>=<span class="nu0">2001</span>:FFFF:a1c5:f1c4 <span class="co0"># ipv6 network prefix</span><br />
    <span class="re2">cartella</span>=<span class="sy0">/</span>var<span class="sy0">/</span>Deleghe <span class="co0"># cartella di destinazione delle deleghe</span><br />
    <span class="re2">face</span>=eth0 <span class="co0"># interfaccia da cui prelevare l'ip local del server</span><br />
    <br />
    <span class="kw3">echo</span> <span class="st0">"CIAO,"</span><br />
    <span class="kw3">echo</span> <span class="st0">"sono l'oracolo dei server *nix.."</span><br />
    <span class="kw3">echo</span> <span class="st0">"puoi chiamarmi... ax"</span><br />
    <span class="kw3">echo</span> <span class="st0">"facciamoci una chiacchierata!"</span><br />
    <span class="kw3">echo</span><br />
    <br />
    rnd_ip_block <span class="br0">&#40;</span><span class="br0">&#41;</span><br />
    <span class="br0">&#123;</span><br />
    &nbsp; &nbsp; <span class="re2">a</span>=<span class="co1">${array[$RANDOM%16]}</span><span class="co1">${array[$RANDOM%16]}</span><span class="co1">${array[$RANDOM%16]}</span><span class="co1">${array[$RANDOM%16]}</span><br />
    <br />
    &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re1">$network</span>:<span class="re1">$a</span>::&nbsp; <br />
    <span class="br0">&#125;</span><br />
    <br />
    <span class="kw3">echo</span> <span class="st0">"<span class="es2">$MAXCOUNT</span> IPv6 ::/80"</span><br />
    <span class="kw3">echo</span> <span class="st0">"*------------------------*"</span><br />
    <span class="kw1">while</span> <span class="br0">&#91;</span> <span class="st0">"<span class="es2">$count</span>"</span> <span class="re5">-le</span> <span class="re1">$MAXCOUNT</span> <span class="br0">&#93;</span> &nbsp; &nbsp; &nbsp; &nbsp;<br />
    <span class="kw1">do</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; rnd_ip_block<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">let</span> <span class="st0">"count += 1"</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">done</span> <span class="sy0">&</span>gt; sub.ipv6<br />
    &nbsp; &nbsp; <span class="kw2">cat</span> sub.ipv6 <br />
    <span class="kw3">echo</span> <span class="st0">"*------------------------*"</span> <br />
    <br />
    <span class="co0"># istruzioni per continuare dopo!</span><br />
    <span class="kw3">echo</span> <span class="st0">""</span><br />
    <span class="kw3">echo</span> <span class="re5">-n</span> <span class="st0">"ax: Vuoi creare lo script di delega con il prefisso generato? [yes or no]: "</span><br />
    <span class="kw2">read</span> scelta<br />
    <span class="kw1">case</span> <span class="re1">$scelta</span> <span class="kw1">in</span><br />
    <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#91;</span>yY<span class="br0">&#93;</span> <span class="sy0">|</span> <span class="br0">&#91;</span>yY<span class="br0">&#93;</span><span class="br0">&#91;</span>Ee<span class="br0">&#93;</span><span class="br0">&#91;</span>Ss<span class="br0">&#93;</span> <span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Ok procedo, ti farÃ² un paio di domande."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Avvio script interattivo con i dati della delega </span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">read</span> <span class="re5">-p</span> <span class="st0">"Quale interfaccia vuoi creare? (nome del tunnel): "</span> iface<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">read</span> <span class="re5">-p</span> <span class="st0">"Qual'Ã¨ end-point? (ipv4 dell'user): "</span> remote<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Perfetto! procedo con la creazione dello script"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Creo il file config con tutti i dati all'interno</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">touch</span> <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"#!/bin/bash"</span> <span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">""</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Cerco e setto la variabile per l'ipv4 locale della macchina in base alle interfacce presenti</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="re2">LOCAL</span>=$<span class="br0">&#40;</span><span class="kw2">ip</span> <span class="re5">-o</span> addr show dev <span class="re1">$face</span> <span class="sy0">|</span> <span class="kw2">sed</span> <span class="re5">-e</span><span class="st_h">'s/^.*inet \([^ ]*\)\/.*$/\1/;t;d'</span> <span class="sy0">|</span> <span class="kw2">head</span> -<span class="nu0">1</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="re2">ifconfig</span>=$<span class="br0">&#40;</span><span class="kw2">ifconfig</span> <span class="re5">-s</span> <span class="sy0">|</span> <span class="kw2">awk</span> <span class="st_h">'{ print $1 }'</span> <span class="sy0">|</span> <span class="kw2">grep</span> <span class="re1">$face</span> <span class="sy0">|</span> <span class="kw2">head</span> -<span class="nu0">1</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#91;</span><span class="br0">&#91;</span> <span class="re1">$ifconfig</span> == <span class="re1">$face</span> <span class="sy0">||</span> <span class="re4">$?</span> == <span class="re4">$0</span> <span class="br0">&#93;</span><span class="br0">&#93;</span>; <span class="kw1">then</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"local addr: [<span class="es2">$LOCAL</span>]"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ATTENZIONE: L'interfaccia local <span class="es2">$face</span> non esiste!"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Non posso continuare a generare la delega..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"In ogni caso puoi visionare il prefisso generato."</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"CMD: 'cat sub.ipv6'"</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"# End"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">fi</span> &nbsp;&nbsp; &nbsp; <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Aggiungo le variabili alla fine della stringa per la subnet creata in precedenza</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="re2">SUBNET</span>=$<span class="br0">&#40;</span><span class="kw2">cat</span> sub.ipv6 <span class="sy0">|</span> <span class="kw2">sed</span> <span class="re5">-e</span> <span class="st_h">'s/$/\/80/g'</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="re2">ADDRESS</span>=$<span class="br0">&#40;</span><span class="kw2">cat</span> sub.ipv6 <span class="sy0">|</span> <span class="kw2">sed</span> <span class="re5">-e</span> <span class="st_h">'s/$/1/g'</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="re2">ROUTE</span>=$<span class="br0">&#40;</span><span class="kw2">cat</span> sub.ipv6 <span class="sy0">|</span> <span class="kw2">sed</span> <span class="re5">-e</span> <span class="st_h">'s/$/2/g'</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Aggiungo i comandi per il mode-sit che andranno nello script</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ip tunnel add <span class="es2">$iface</span> mode sit remote <span class="es2">$remote</span> local <span class="es2">$LOCAL</span> ttl 255"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ip link set <span class="es2">$iface</span> up"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ip -6 a a <span class="es2">$ADDRESS</span> dev <span class="es2">$iface</span>"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0">#echo "ip -6 r a $ROUTE dev $iface" &gt;&gt; $iface.conf</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0">#echo "ip -6 r a $SUBNET via $ROUTE dev $iface metric 256" &gt;&gt; $iface.conf</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ip -6 r a <span class="es2">$SUBNET</span> dev <span class="es2">$iface</span> metric 256"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"sysctl -w net.ipv6.conf.<span class="es2">$iface</span>.forwarding=1"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">""</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"# END "</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Agisco sulle estensioni del file finale!</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ax: converto lo script in bash per eseguirlo via ./<span class="es2">$iface</span>.sh"</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">sleep</span> <span class="nu0">3</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">mv</span> <span class="re1">$iface</span>.conf <span class="re1">$iface</span>.sh<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ax: FATTO! Script generato: <span class="es2">$iface</span>.sh"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"Ricordati di visionarlo prima di lanciarlo per verificare i dati."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"Magari hai premuto qualche tasto in modo accidentale..."</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"Oppure sei/eri sotto l'effetto di una sostanza allucinogena... ;&gt;"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># creo lo script da dare all'utente</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">touch</span> <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"#!/bin/bash"</span> <span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">""</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ip tunnel add <span class="es2">$iface</span> mode sit remote <span class="es2">$LOCAL</span> local <span class="es2">$remote</span> ttl 255"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ip link set <span class="es2">$iface</span> up"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ip -6 a a <span class="es2">$ROUTE</span>/80 dev <span class="es2">$iface</span>"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ip -6 r a <span class="es2">$ADDRESS</span> dev <span class="es2">$iface</span>"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"ip -6 r a default via <span class="es2">$ADDRESS</span> &nbsp;dev <span class="es2">$iface</span> metric 256"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf &nbsp; &nbsp; &nbsp; <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"sysctl -w net.ipv6.conf.<span class="es2">$iface</span>.forwarding=1"</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">""</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"# END "</span> <span class="sy0">&</span>gt;<span class="sy0">&</span>gt; <span class="re1">$iface</span>.user-conf<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="re5">-e</span> <span class="st0">"Ho creato lo script client per l'utente finale [<span class="es2">$iface</span>.user-conf]"</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#91;</span>nN<span class="br0">&#93;</span> <span class="sy0">|</span> <span class="br0">&#91;</span>nN<span class="br0">&#93;</span><span class="br0">&#91;</span>Oo<span class="br0">&#93;</span> <span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Hai deciso di non continuare. Tutt APPOST!"</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"In ogni caso puoi visionare il prefisso generato."</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"CMD: 'cat sub.ipv6'"</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">*</span><span class="br0">&#41;</span> &nbsp; &nbsp; &nbsp;<span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Scus ma nun sai legger? O vir che si GNURANT! Scegli: Y/N"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Puoi comunque visionare il prefisso generato."</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"CMD: 'cat sub.ipv6'"</span>; <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span> <br />
    <span class="kw1">esac</span><br />
    <br />
    <span class="co0"># istruzioni per la verifica del suffisso generato!</span><br />
    <span class="kw3">echo</span> <br />
    <span class="kw3">echo</span> <span class="re5">-n</span> <span class="st0">"ax: Vuoi verificare se la subnet creata esiste giÃ ? [yes or no]: "</span><br />
    <span class="kw2">read</span> verifica<br />
    <span class="kw1">case</span> <span class="re1">$verifica</span> <span class="kw1">in</span><br />
    <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#91;</span>yY<span class="br0">&#93;</span> <span class="sy0">|</span> <span class="br0">&#91;</span>yY<span class="br0">&#93;</span><span class="br0">&#91;</span>Ee<span class="br0">&#93;</span><span class="br0">&#91;</span>Ss<span class="br0">&#93;</span> <span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">""</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: OK procedo con la verifica."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># Riprendo i parametri della subnet generata e effettuo la verifica nella directory specificata</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="re2">VERIFY</span>=$<span class="br0">&#40;</span><span class="kw2">grep</span> <span class="re5">-r</span> <span class="re5">-i</span> <span class="st0">"<span class="es2">$SUBNET</span>"</span> <span class="re1">$cartella</span> <span class="sy0">|</span> <span class="kw2">awk</span> <span class="st_h">'{ print $5 }'</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#91;</span> <span class="st0">"<span class="es2">$SUBNET</span>"</span> == <span class="st0">"<span class="es2">$VERIFY</span>"</span> <span class="br0">&#93;</span>; <span class="kw1">then</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ATTENZIONE: la subnet creata Ã¨ giÃ  attiva"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Premi un tasto per continuare ..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">read</span> <span class="co0"># blocco il continuo dello script fino alla selezione di un tasto a caso</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Le probabilitÃ  dovevano essere rare in teoria."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"A quanto pare, invece, l'improbabile Ã¨ dietro l'angolo..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Devi necessariamente ricominciare il processo di creazione della subnet!"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Lo script successivo potrebbe tornarti utile..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Nessuna subnet con lo stesso prefisso ;&gt;"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Vai liscio come l'olio! PROCEDI!"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">fi</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">rm</span> sub.ipv6 <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    &nbsp; &nbsp; <span class="br0">&#91;</span>nN<span class="br0">&#93;</span> <span class="sy0">|</span> <span class="br0">&#91;</span>nN<span class="br0">&#93;</span><span class="br0">&#91;</span>Oo<span class="br0">&#93;</span> <span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Hai deciso di fregartene..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Se esiste giÃ : cazzi tuoi!"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"In ogni caso puoi visionare lo script *.sh generato."</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">rm</span> sub.ipv6<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">*</span><span class="br0">&#41;</span> &nbsp; &nbsp; &nbsp;<span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Tasto non consentito ..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Taviss mbarÃ  a legger nu poc meglio..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"In ogni caso puoi visionare lo script *.sh generato."</span>;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ADDIO!"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">rm</span> sub.ipv6<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span> <br />
    <span class="kw1">esac</span><br />
    <br />
    <span class="co0"># Scelta delle azioni finali sul file.sh generato!</span><br />
    <span class="kw3">echo</span><br />
    <span class="re2">PS3</span>=<span class="st0">"ax: Scegli se/come agire su [<span class="es2">$iface</span>.sh] oppure premi [ CTRL+C ] per terminare lo script: "</span><br />
    <span class="kw1">select</span> azione <span class="kw1">in</span> <span class="st0">"Visualizza"</span> <span class="st0">"Cambia permessi"</span> <span class="st0">"Sposta"</span> <span class="st0">"Elimina"</span> <span class="st0">"Esci"</span>; <span class="kw1">do</span><br />
    <br />
    &nbsp; &nbsp; <span class="kw1">case</span> <span class="re1">$azione</span> <span class="kw1">in</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="st0">"Visualizza"</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: informazioni pe GNURANT! :"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Tasti direzionali [â—€â–¼â–²â–¶ | pagSU-pagGIU]"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Quit [q]"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Premi un tasto per continuare nella visualizzazione..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">read</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">less</span> <span class="re1">$iface</span>.sh<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="st0">"Cambia permessi"</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">chmod</span> +x <span class="re1">$iface</span>.sh<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Ho cambiato i permessi di [<span class="es2">$iface</span>.sh] per l'esecuzione."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Adesso puoi lanciarlo con [sudo|su ./<span class="es2">$iface</span>.sh]"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="st0">"Sposta"</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: Verifico se ci sono le condizioni per spostare il file..."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="co0"># setto varibile di controllo dell'esistenza del file nella directory di destinazione</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#91;</span><span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$cartella</span><span class="sy0">/</span><span class="re1">$iface</span>.sh <span class="br0">&#93;</span><span class="br0">&#93;</span>; <span class="kw1">then</span> <span class="co0"># se esiste il file $cartella/$iface.sh fai questo </span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ATTENZIONE: il file <span class="es2">$iface</span>.sh esiste giÃ !"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Probabilmente hai giÃ  assegnato una delega con questo nome."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Non posso copiare il file per ragioni di sicurezza!."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"A questo punto ti consiglio di eliminarlo prima di uscire."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">elif</span> <span class="br0">&#91;</span><span class="br0">&#91;</span> <span class="re5">-x</span> <span class="re1">$iface</span>.sh &nbsp;<span class="br0">&#93;</span><span class="br0">&#93;</span>; <span class="kw1">then</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">mv</span> <span class="re1">$iface</span>.sh <span class="re1">$cartella</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: <span class="es2">$iface</span>.sh Ã¨ stato spostato nella dir delle deleghe: [<span class="es2">$cartella</span>]"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: <span class="es2">$iface</span>.sh non Ã¨ eseguibile!"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"Devi prima cambiare i permessi."</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">fi</span> &nbsp;<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span> <br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="st0">"Elimina"</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">rm</span> <span class="re1">$iface</span>.sh<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: ho eliminato [<span class="es2">$iface</span>.sh]"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span class="st0">"Esci"</span><span class="br0">&#41;</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">echo</span> <span class="st0">"ax: ADDIO"</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">exit</span> <span class="nu0">1</span><br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="sy0">;;</span><br />
    &nbsp; &nbsp; <span class="kw1">esac</span><br />
    <span class="kw1">done</span>
  </div>
</div>

</pre>

\# End
