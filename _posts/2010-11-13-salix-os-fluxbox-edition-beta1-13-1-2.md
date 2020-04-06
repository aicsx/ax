---
id: 1342
title: 'Salix OS Fluxbox edition - beta1 13.1.2'
date: 2010-11-13T08:00:38+01:00
author: ax
excerpt: |
  <p>Rilasciata Salix OS fluxbox release "devel" by me (ax)/kerd. La distribuzione Salix OS (Slackware based) per adesso basata su sistemi 32bit sarà al più presto disponibile per X64 e in LIVE CD. Tempo al tempo e sarà disponibile insomma in tutte le varianti utili.</p>
layout: post
guid: http://linuax.wordpress.com/?p=1342
permalink: /2010/11/13/salix-os-fluxbox-edition-beta1-13-1-2/
jabber_published:
  - "1289632518"
  - "1289632518"
email_notification:
  - "1289632519"
  - "1289632519"
categories:
  - Fluxbox
  - Linux
  - Salix
  - Slackware
  - Wm
  - Xorg
tags:
  - Linux
  - salix
  - slackware
---
## \# Pre/post

Sono molto felice di riscrivere dopo alcuni mesi qui sul blog. Era molto tempo che non lo facevo, un pò per mancanza di stimoli in ambito "non sapevo più di cosa scrivere" ed un pò per via del tempo che si è rimpicciolito. Sappiate però che leggo e rispondo sempre a tutti i commenti e/o difficoltà degli utenti e che **non** sono scomparso.

Ad ogni modo oggi vorrei parlarvi dell'ultimo progetto in cantiere. Tutti voi frequentarori di questo blog conoscete ormai la mia sempre "conservatrice" ed ossessiva passione per Slackware. Proprio a tal proposito, ci tenevo quindi a condivedere con tutti voi il frutto del mio ultimo lavoro. Sto parlando di **Salix OS Fluxbox release** , ovviamente slackware based.

Innanzitutto, è bene partire dal principio. Cos'è Salix ?

## \# Panoramica di base

<a href="http://www.salixos.org/wiki/index.php/Home" target="_blank"><strong>Salix</strong></a> è una distro GNU/Linux basata sulla celebre **Slackware**, che è quindi totalmente compatibile in tutte le sue parti alla sua madre nativa. Si parte dai pacchetti *.txz che sono compatibili, fino ad arrivare ad i tools principali. La differenza sostanziale è che **Salix** OS si avvale di alcuni strumenti _"nuovi"_ integrati per fare il lavoro "sporco" che su Slackware andava fatto a mano. Chiaramente capite tutti che si sta parlando della compilazione dei pacchetti. Ebbene si... questo argomento oggi tocca anche me. Perchè lo dico ?

Lo dico perchè con il passare dei giorni/mesi/anni ... c'è sempre una maggiore richiesta di strumenti che facilitino e/o aumentino le potenzialità di sistema attraverso la più giusta compilazione dei pacchetti source. Essendo Slackware altamente basata sulla compilazione manuale degli stessi quindi, diciamo che molto spesso diventa urgente un aiutino. Soprattutto per quella fascia di utenza che non riesce ad avere le competenze necessarie (per tempo e/o per esperienza)per sopperire a tale capacità.

## \# Differenziazione da Slackware

Venendo al dunque... Salix è dotata di due cose fondamentali nativamente:

**1) SLKBUILD**

**2) SLAPT-GET**

**-** Per quanto riguarda **SLKBUILD** possiamo semplicisticamente dire che si tratta di uno script come lo è un PKGBUILD in ambito ARCH che si occupa di descrivere: versione, nome, revisione e fix, autore, parametri di compilazione, architettura, source link e parametri aggiuntivi (dove si intendono comandi essenziali dopo la compilazione del pacchetto). Ogni singola app può essere scritta e compilata attravero il lancio degli script **SLKBUILD**. Sono una sorta di Slackbuilds, che hanno in aggiunta una meticolosa e scrupolosa attenzione da parte dei devel della distribuzione. Ogni singolo script e quindi pacchetto viene guardato e/o nel caso rettificato attraverso comunicazione ufficiale. Ci sarebbe tanto e tanto da discutere sull'uso e sull'introduzione degli **SLKBUILD**, per l'enorme riuscita della cosa. Ma finirei per risultare dispersivo. Motivo per cui il miglior consiglio da parte mia "slackware user" da una vita ormai... è quello di provare per credere!

- Per quanto riguarda **SLAPT-GET** invece .... tutti noi lo conosciamo credo. Si tratta di una sorta di apt-get di debian per Slackware che si appoggia a dei mirror. La cosa strabiliante è che la community **Salix** sta crescendo giorno dopo giorno ed allo stesso modo crescono i pacchetti disponibili e i packagers dediti a questo compito. Di conseguenza fin dal primo avvio la distribuzione viene predisposta all'installazione e all'utilizzo di slapt-get integrato per i pacchetti dai mirror ufficiali si Salix. E' ovvio che essendo Slackware in tutte le sue parti. I mirror sono fixati per reperire anche pacchetti ufficiali di Slackware e/o di terzi attraverso la modifica dei conf in pochi e semplici passi. Insomma, proprio nulla da invidiare dal punto di vista di pacchetti alle distribuzioni famose per questo. Con tutto ciò che si sta parlando di una distribuziuone relativamente nuova.

## \# Considerazione

In ultimo e non meno importante c'è l'aspetto community, che tanto è mancato a Slackware negli ultimi tempi. Questa distribuzione è pronta ad ospitare e dare attenzione alle persone più e/o meno competenti che ritengono di poter dare un contributo per lo sviluppo della stessa e/o anche delle semplici impressioni ed info. Ci sono persone pronte ad ascoltare e confrontarsi. Questa è una delle cose che più mi è mancata negli ultimi anni per esperienze dirette passate. Adesso invece "constatate" alcune differenze da questo punto di vista, specie in ambito Slackware _**based**_, sono molto felice.

## **\# Pre/post OFF**

**  
** 

## **\# P**ost

Detto tutto questo veniamo alla _notizia._ Tutti voi avrete notato dai mesi trascorsi il link e le new verso il progetto **<a href="http://www.fluxbox.it" target="_blank">Fluxbox.it</a>** _"Italian Fluxbox Community"_. Ci siamo accorti che molti utenti avevano difficoltà in merito di conoscenza di questo fantastico wm e di conseguenza ci siamo attivati per cercare di migliorare le info e tutto ciò che ne consegue. Per questa ragione quindi abbiamo _"io ed un particolare e stimato amico: **kerd**"_ deciso di cercare di non soffermarci solo sulla community ma di dare più spazio al wm.

Proprio sulla questione quindi è nata **Salix OS fluxbox version** "devel". Devel, perchè appunto l'annuncio è stato dato solo ieri per fonti ufficializzate tramite Link e forum di riferimento da parte dei developments nativi della distribuzione.

La versione fluxbox rilasciata da me "**ax**" e da **kerd** è appunto una Salix in tutto e per tutto. L'ambiente scelto è **Fluxbox** per i motivi elencati (ed oltre).

La distribuzione per adesso basata su sistemi **32bit** sarà al più presto disponibile per X64 e in LIVE CD. Tempo al tempo e sarà disponibile insomma in tutte le varianti utili. Nel frattempo vi elenco alcune delle caratterisitiche:

- kernel ufficiale Slackware 13.1: **kernel 2.6.33-4**

- Wm: **Fluxbox** 

**-** Avvio in init4 (grafico): **gdm**

**-** Scelta del tipo di  installazione all'avvio:

**1) core**: sarà installato solo il sistema di base con i programmi essenziali (no X )

 **2) basic:** sarà installato il sistema di base core più X, più il wm Fluxbox ( in aggiunta solo le applicazioni essenziali: pcmanfm, firefox, rxvt-unicode (emulatore di shell).

**3) full:** sarà installato il sistema di base core più X, più Fluxbox, più l'albero dei pacchetti previsti, tra cui: pcmanfm, firefox, claws-mail, rxvt-unicode, leafpad, openoffice (full), exaile, parcellite, wicd, etc...

Una screen per rendersi conto:

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2010/11/images.jpe"><img class="alignnone size-full wp-image-1525" title="images" src="http://linuax.altervista.org/blog/wp-content/uploads/2010/11/images.jpe" alt="" width="259" height="194" /></a></pre>

C'è da definire alcune cose essenziali... ovvero che:

essendo una devel è appunto in sviluppo e come tale ogni commento e/i considerazione propositiva sarà presa in considerazione, attraverso me, il team di lavoro e quello ufficiale. Sarei molto felice di avere pareri ed opinioni da parte vostra. Detto tutto questo, vi rilascio al link ufficiale per testare e provare la distribuzione.

**Download: <a href="http://www.salixos.org/forum/viewtopic.php?f=17&t=1546" target="_self">Salix OS fluxbox version</a> - _annuncio della release devel sul forum._**  
_\# Post/considerazioni_

Spero che il lavoro fatto fino ad ora sia di vostra approvazione e gradimento. In ogni caso, non esitate a dire la vostra e a proporre cambiamenti. La distro può essere testata anche su Virtual Machine come Virtualbox e/o emulatori di terzi.

Che altro dire: restate in campana, a breve release stable ufficiale e altre iniziative. Saluti a tutti!

p.s. scusate il ritardo ...

**\# END**