---
id: 349
title: 'Gestione avanzata dei log con tail: Multitail'
date: 2009-02-09T21:07:04+01:00
author: ax
excerpt: Grazie a questo tool è possibile visualizzare più file contemporaneamente. Una volta lanciato MultiTail è in grado di dividere il proprio terminale in tante finestre (attraverso utilizzazione di ncurses). Inoltre è possibile usare colori differenti per ogni finestra, è consentito il merge tra più file di log e il filtraggio attraverso espressioni regolari.
layout: post
guid: http://linuax.wordpress.com/?p=349
permalink: /2009/02/09/gestione-avanzata-dei-log-con-tail-multitail/
categories:
  - How-to
  - Linux
  - log
tags:
  - Linux
  - log
  - tail
---
Se usate spesso **tail** per tenere sotto controllo i file di log del sistema allora non potete non provare una versione del programma potenziata: **[MultiTail](http://www.vanheusden.com/multitail/index.html)**.

Grazie a questo tool è possibile visualizzare più file contemporaneamente. Una volta lanciato MultiTail è in grado di dividere il proprio terminale in tante finestre (attraverso utilizzazione di ncurses). Inoltre è possibile usare colori differenti per ogni finestra, è consentito il merge tra più file di log e il filtraggio attraverso espressioni regolari.

Il programma gira su tutte le distribuzioni Linux e i sistemi *BSD (compreso MacOSX).  
L’ultima versione stable 5.2.2, è scaricabile dalla [pagina di download](http://www.vanheusden.com/multitail/download.html) del progetto.

MultiTail risulta essere uno strumento  necessario per l'amministrazione comoda da shell della propria linuxbox e/o del proprio server privato.

<img class="alignnone size-full wp-image-351" title="multitail1" src="http://linuax.files.wordpress.com/2009/02/multitail1.jpg" alt="multitail1" width="455" height="329" /> 

L'installazione è estremamente semplice, basta compilare il source e/o installare il precompilato pacchettizzato per la propria distribuzione GNU/linux.

Esempio:

<pre>~$ wget <a href="http://www.vanheusden.com/multitail/multitail-5.2.2.tgz" target="_blank">multitail-5.2.2.tgz</a>
~$ tar zxvf multitail-5.2.2.tgz
~$ cd multitail-5.2.2/
~$ make
~$ su
Password:
~# make install</pre>

Installato l'applicativo, ci sono varie possibilità di avvio, come negli esempi seguenti:

<pre>~# multitail /var/log/messages /var/log/syslog</pre>

oppure:

<pre>~# multitail /var/log/iptables.log -l "ping server.esempio.it"</pre>

o  anche:

<pre>~# multitail /var/log/httpd.log -l "netstat -nat"</pre>

E' possibile avviare più di due file log da visualizzare in un unica schermata:

<pre>~# multitail -s 2  /var/log/messages /var/log/syslog /var/log/httpd/access_log</pre>

Per ulteriori info:

<pre>~$ man multitail</pre>

\# End