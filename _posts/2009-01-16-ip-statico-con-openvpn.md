---
id: 300
title: IP statico con openvpn
date: 2009-01-16T02:13:04+01:00
author: ax
excerpt: 'Semplici passi per la costruzione di una vpn con delega di un ip statico per proprio uso. '
layout: post
guid: http://linuax.wordpress.com/?p=300
permalink: /2009/01/16/ip-statico-con-openvpn/
categories:
  - How-to
  - Linux
tags:
  - How-to
  - ip
  - openvpn
  - static
---
Come avere un ip statico sulla propria box sfruttando le potenzialità di openvpn:

**NB:** dopo aver installato openvpn in base alla vostra distro e risolte le dipendenze;

Generate una key su un server e copiatela anche sull'altro sempre nella dir _/etc/openvpn_ :

<pre>~# openvpn --genkey --secret /etc/openvpn/my.key</pre>

Ora create i file di configurazione in _/etc/openvpn_ e chiamateli rispettivamente **client.conf** e **server.conf** ovviamente tenendo presente la destinazione ad uso server o client.

**Client.conf:  
** 

<pre>mode p2p

proto udp

port 12345

remote mydns.com // dns server

dev vpnesempio //nomeinterfaccia

dev-type tun

ifconfig 115.55.5.1 10.0.5.1 // ip delegato 

persist-tun

keepalive 10 60

daemon

verb 3

secret /etc/openvpn/my.key</pre>

seguite questi semplici passi:

<pre>~# pico /etc/iproute2/rt_tables</pre>

e aggiungete:

<pre>200 vpnesempio</pre>

Dare questi ultimi 2 comandi:

<pre>~# ip rule add from 115.55.5.1 table 200 // ip delegato

~# ip route add default dev vpnesempio table 200 // interfaccia vpn</pre>

Ora sul **Server** creare il file **server.conf** contenente :

**Server.conf:**

<pre>mode p2p

proto udp

port 12345

float

dev vpnesempio01 // nome interfaccia

dev-type tun

ifconfig 10.0.5.1 155.55.5.1 //ip da delegare 

route 155.55.5.1 //ip da delegare 

persist-tun

#daemon vpn-esempio

log /var/log/vpn-esempio.log

verb 3

secret /etc/openvpn/my.key</pre>

e prima di startare il servizio lato server:

<pre>~# echo 1 &gt; /proc/sys/net/ipv4/conf/all/proxy_arp

~# echo 1 &gt; /proc/sys/net/ipv4/ip_forward</pre>

Non resta che dare il comando:

<pre>~# /etc/rc.d/rc.openvpn start</pre>

sulle 2 box.

\# END

How-to by devnull

IRCnet: #unixzone

mail: fix.null@gmail.com

link: <a href="http://www.unixzone.org" target="_blank">www.unixzone.org</a>