---
id: 765
title: 'Samba security tips: binding - hosts - interfaces'
date: 2009-06-29T10:00:04+01:00
author: ax
excerpt: Samba dispone di alcune semplici direttive che ci consentono di condividere file,cartelle e servizzi nella rete con un ottimo livello di sicurezza.
layout: post
guid: http://linuax.wordpress.com/?p=765
permalink: /2009/06/29/samba-security-tips-binding-hosts-interfaces/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Linux
  - Reti LAN
  - Samba
  - Tips
tags:
  - How-to
  - interfaces
  - Linux
  - samba linux
  - slackware
---
Samba è uno strumento perfetto per condividere cartelle,file e servizzi in rete, ma soffre chiaramente se soggetto agli attacchi _"comuni"_ provenienti dalla rete pubblica. Il modo più efficace per prevenire questi attacchi e mettere Samba in **_sicurezza_** è di effettuare alcuni piccoli accorgimenti sul config generale _**/etc/samba/smb.conf**_ ;

Le direttive principali di sicurezza sono quelle dell'esclusione di tutti i range di **IP.** Sono esclusi dal parametro i tentativi di connessione da parte degli ip provenienti della rete interna marcati in **hosts allow** come da esempio:

<pre><span><span style="direction:ltr;text-align:left;">hosts allow = 127.0.0.1 192.168.2.0/24 192.168.3.0/24</span> </span> <span><span style="direction:ltr;text-align:left;">
hosts deny = 0.0.0.0/0</span></span></pre>

 <span></span>

<span>o anche </span>

<pre>hosts allow = localhost, 10.0.0.6, 10.0.0.7<span>
hosts deny = ALL </span></pre>

<span>Un altro tip di sicurezza può essere quello di ascoltare il traffico e i tentativi di connessione solo da determinate interfacce:</span> <span></span>

<pre><span><span style="direction:ltr;text-align:left;">bind interfaces only = yes</span></span>
interfaces = eth0 lo</pre>

<span>o anche </span>

<pre><span><span style="direction:ltr;text-align:left;">bind interfaces only = yes</span></span>
interfaces = eth* lo</pre>

<span><strong>NB: </strong>nell'ultimo caso samba ascolterà su tutte le interfacce di rete nominate eth*, esempio: eth0,eth1, etc ... Chiaramente il nome delle interfacce cambia in base al sistema operativo (freebsd,linux,etc)<br /> </span>

<span><strong>NB</strong>: E' sempre consigliato l'utilizzo di un Firewall (iptables su linux), tenendo a mente che Samba utilizza le seguente porte TCP/UDP:</span>

<pre><span>UDP/137 - utilizzata da nmbd</span><span><span style="direction:ltr;text-align:left;">
</span>UDP/138 - utilizzata da nmbd</span>
<span>TCP/139 - utilizzata da smbd</span>
<span><span style="direction:ltr;text-align:left;">TCP/445 - utilizzata da smbd</span></span></pre>

**NB:** Tutte le modifiche vanno  inserite nella sezione principale del file **smb.conf** sotto la direttiva **[global]** .

\# End