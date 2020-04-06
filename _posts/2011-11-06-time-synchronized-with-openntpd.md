---
id: 1445
title: 'Time: Synchronized with OpenNTPD'
date: 2011-11-06T00:57:05+01:00
author: ax
excerpt: "Il passaggio dall'ora locale all'ora legale e viceversa è molto spesso vista come una cosa di poco conto. Le cose però possono cambiare quando abbiamo in background servizi di sistema che per essere lanciati necessitano dell'orario corretto."
layout: post
guid: http://linuax.wordpress.com/?p=1445
permalink: /2011/11/06/time-synchronized-with-openntpd/
jabber_published:
  - "1320537426"
  - "1320537426"
email_notification:
  - "1320537426"
  - "1320537426"
categories:
  - Arch
  - How-to
  - Linux
  - terminal
  - Tips
tags:
  - Arch
  - daemon
  - Linux
  - ntp
  - OpenNTPD
  - Tips
---
Il passaggio dall'ora locale all'ora legale e viceversa è molto spesso vista come una cosa di poco conto. Le cose però possono cambiare quando abbiamo in background servizi di sistema che per essere lanciati necessitano dell'orario corretto.

&nbsp;

Ecco quindi che le cose si complicano ancora una volta se un demone come ad esempio servizio _ntp_ nello specifico **ntpd** risulta in stato_ **"deprecated"**_. Per chi non lo sapesse, il demone ntpd si occupa appunto della sincronizzazione dell'orario di sistema attraverso una serie di server differenziati per regione/stato/nazione etc... Su Arch linux questo demone richiede l'installazione del pacchetto: ntp.

Chi utilizzava il demone ntpd si sarà sicuramente reso conto che il demone non funziona più in modo corretto. Motivo per cui vi elenco brevemente le due soluzioni veloci ed efficaci. Ogni soluzione ha dei pro e dei contro. Vediamo perchè!

## **1. NTP**

La prima è semplicemente di smettere di utilizzare il demone ntpd tra i servizi di avvio. Normalmente infatti la norma sarebbe di seguire i passi citati:

<pre>installare il pacchetto
# pacman -S ntp

configurare le specifiche e il server
# vi /etc/ntp.conf 

avvio del demone
# /etc/rc.d/ntpd start

Blacklistare nel caso sia abilitato al boot !hwclock (il ! serve appunto a questo) e aggiunta del demone
in autostart al reboot mediante la modifica di /etc/rc.conf con un editor di testo preferito (vi,nano,etc.)
per fare in modo che risulti come da esempio:
DAEMONS=(... !hwclock <strong>ntpd</strong> ...)</pre>

**NOTA BENE:** essendo il demone deprecato questa soluzione _"normale"_ non funzionerà. Per cui chi utilizza già ntp installato nel sistema può semplicemente optare per la sincronizzazione manuale alla fine degli script di avvio del sistema. Per farlo ovviamente ci basterà aggiungere il comando seguente al file **/etc/rc.local** come da esempio seguente:

<pre># vi /etc/rc.local
<strong>ntpd -qg &</strong></pre>

In questo modo al riavvio sarà avviata la procedura di boot e alla fine subito prima del login sarà effettuata la sincronizzazione. Come si nota lo script sarà lanciato in background e quindi come il vero demone presente in rc.d. Inutile dire che chi opta per questa soluzione può anche eliminare ntpd  da DAEMONS in  **/etc/rc.conf**  essendo non più necessario. Il vantaggio sarà di continuae ad utilizzare il pacchetto ntp già installato senza eventuali nuove modifiche di pacchetti al sistema.

**NOTA MOLTO  BENE:** questa soluzione non si consiglia a colori i quali necessitano della sincronizzazione di sistema prima del termine di tutti gli script di avvio. Come dicevo ad inizio post, se devo lanciare un altro demone che è a sua volta presente nei DAEMONS all'avvio.... devo assicurarmi che l'orologio sia corretto prima. Pena: potrebbero non essere lanciati gli altri demoni con questa soluzione. Inutile elencarvi quali (certamente quelli che utilizzano scripts di network che si localizzano su time).

## **2. OpenNTPD**

OpenNTPD non è altro che un implementazione di Network Time Protocol  su OpenBSD. Da qui deriva appunto il nome. Sostanzialmente non dovremo fare altro che sostituire il nostro vecchio demone ntpd con il nuovo pacchetto **OpenNTPD** con relativo servizio e demone.

<pre>rimuoviamo il pacchetto obsoleto:
# pacman -R ntp

installiamo il nuovo pacchetto:
# pacman -S openntpd

modifichiamo il conf con un editor di testo a scelta:
# vi /etc/ntpd.conf

ci assicuriamo che risulti configurato il server di sincronizzazione.
<strong>NOTA BENE:</strong> tutte le stringhe commentate non sono necessarie per il nostro scopo. Vanno listati gli indirizzi
nel caso si scelga di fare da server time. Non è il nostro caso. Per cui l'unica cosa che ci interessa è
la stringa dove è definito il server.

# $OpenBSD: ntpd.conf,v 1.7 2004/07/20 17:38:35 henning Exp $
# sample ntpd configuration file, see ntpd.conf(5)

# Addresses to listen on (ntpd does not listen by default)
#listen on 0.0.0.0
#listen on 127.0.0.1
#listen on ::1

# sync to a single server
#server ntp.example.org

# use a random selection of 8 public stratum 2 servers
# see http://twiki.ntp.org/bin/view/Servers/NTPPoolServers
<strong>servers pool.ntp.org</strong>

Non ci resta che lanciare il servizio e l'orario sarà sincronizzato:
# /etc/rc.d/openntpd start

Ovviamente ci interessa anche avviarlo ad ogni boot di sistema. Per questo modifichiamo la lista DAEMONS
nel file <em><strong>/etc/rc.conf</strong></em> come da prassi in modo che risulti come l'esempio seguente:
DAEMONS=(syslog-ng network <strong>openntpd</strong> ...)

oppure per metterlo in avvio di background:

DAEMONS=(syslog-ng network <strong>@openntpd</strong> ...)</pre>

**Considerazioni:** a volte le cose apparentemente più futili sono anche le più utili in termini pratici. Nel mio caso mi impediva il corretto avvio degli script di sistema in background con anche un rallentamento generale nel tentativo di stabilire una connessione che non trovava mai risposta adeguata per via dell'orario non corretto. Ecco perchè ho deciso di scrivere di questo piccolo tips... magari può risultare utile ad altri come lo è stato per me. La procedura è descritta per Arch Linux essendo appunto la box su cui ho avuto e risolto il mini_"problema"_ ehehhe :) ... ma inutile dire che non ci sono differenze su Slackware se non che per la compilazione manuale come da prassi di sistema del pacchetto openntpd.

\# END

&nbsp;