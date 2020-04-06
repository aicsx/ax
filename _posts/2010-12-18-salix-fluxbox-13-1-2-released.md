---
id: 1360
title: Salix Fluxbox 13.1.2 released!
date: 2010-12-18T08:00:24+01:00
author: ax
excerpt: |
  <p>Anche se con qualche giorno di ritardo dovuto ai troppi impegni lavorativi da parte di tutti… sono lieto di annunciare che è stata rilasciata la versione stable ufficiale di Salix OS Fluxbox Edition 13.1.2 by ax/kerd Il progetto è nato appunto dalla voglia di fornire una distribuzione che fosse basata su Slackware e che avesse tutti i requisiti per essere amata da utenti più o meno esperti senza dimenticare tutte le ottime caratteristiche che noi da sempre amiamo di Slackware. E’ stata scelta appunto Salix perchè è stata ritenuta grazie all’innovazione e al binomio: Slapt-get + slkbuild un ottima distribuzione capace di sopperire a tutte le carenze esplicitamente richieste dagli utenti di Slackware, specie lato pacchetti e compilazione degli stessi. Questi sono i principali motivi che ci hanno spinto “me e kerd” ed invogliato nel lavorare a questa iso … che al momento sembra essere anche molto apprezzata dagli utenti. Cosa di cui andiamo molto fieri. Spero come sempre che il lavoro fatto sia di vostro gradimento. Saluti!</p>
layout: post
guid: http://linuax.wordpress.com/?p=1360
permalink: /2010/12/18/salix-fluxbox-13-1-2-released/
jabber_published:
  - "1292655731"
  - "1292655731"
email_notification:
  - "1292655733"
  - "1292655733"
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
## **\# Open post**

Anche se con qualche giorno di ritardo dovuto ai troppi impegni lavorativi da parte di tutti… sono lieto di annunciare che è stata rilasciata la versione stable ufficiale di Salix OS Fluxbox Edition 13.1.2 by ax/kerd.

La news ufficiale è stata postata il 10/12 attraverso questo blog nella sezione realtiva, mentre l'annuncio ufficiale tramite forum di riferimento è stato fatto il 7/10 che potete visionare **<a href="http://www.salixos.org/forum/viewtopic.php?f=17&t=1648" target="_blank">QUI</a>**.

Il progetto è nato appunto dalla voglia di fornire una distribuzione che fosse basata su Slackware e che avesse tutti i requisiti per essere amata da utenti più o meno esperti senza dimenticare tutte le ottime caratteristiche che noi da sempre amiamo di Slackware.  
E’ stata scelta appunto Salix perchè è stata ritenuta grazie all’innovazione e al binomio: Slapt-get + slkbuild un ottima distribuzione capace di sopperire a tutte le carenze esplicitamente richieste dagli utenti di Slackware, specie lato pacchetti e compilazione degli stessi.

Questi sono i principali motivi che ci hanno spinto “me e kerd” ed invogliato nel lavorare a questa iso … che al momento sembra essere anche molto apprezzata dagli utenti. Cosa di cui andiamo molto fieri.

## **\# Premessa ...** 

Mi scuso per il grande ritardo da parte mia nel postare questo articolo nei confronti  dell'utenza affezionata a questo blog "grazie a voi di esistere" ... senza di voi non ci sarei nemmeno io ... siete uno stimolo e una voglia continua di fare le cose per bene.... oltre che il mio supporto nel ricordarmi tutti i giorni che le cose si fanno per passione e non per altri fini.

## **\# Changelog:**

Con il rilascio della release stable sono cambiate un bel pò di cose.... innanzitutto la release è stata rilasciata in due versioni ovvero: x86 e x86_64 ed è come tutte le altre release fornita di tripla scelta all'installazione... vediamo in dettaglio:

  1. **Full:** include un ambiente desktop completo con rispettive applicazioni annesse per l'ambiente. Nel nostro caso trattandosi di Fluxbox sono state scelte applicazioni leggere, veloci e con un minimo utilizzo di ram. Come rispecchiato da tutti gli utenti, l'iso comprende molte applicazioni per audio,video,web,ufficio; per questo lato la scelta obbligata al momento è ricaduta su Openoffice, incluso nella iso.
  2. **Basic:** questo tipo di installazione prevede solo l'ambiente di base del wm, incluse alcune applicazioni necessarie per tale ambiente tra cui gslapt e firefox.
  3. **Core:** solo il minimo necessario, l'ambiente desktop compreso Xorg non sarà installato.Questo tipo di installazione è l'ideale per gli utenti esperti che intendono partire da una distribuzione ordinata e selezionata di base Slackware lato server.

Sono stati corretti molti fix rispetto alla release beta e alle successive RC1-2-3; lo spazio è stato ridotto al minimo indispensabile necessario al corretto funzionamento dell'iso. Sono stati inclusi i link a tutti i riferimenti ufficiali del progetto e di supporto compresi: jabber,irc, forum, wiki.

Sono stati "felicemente" corrette le esecuzioni dei programmi occorrenti di d-bus e degli script di avvio (vedi temi e script di esecuzione lato desktop di fluxbox).

Sono stati corretti percorsi e relative dipendenze di molti pacchetti; nel frattempo molti altri sono stati eliminati dalla distribuzione di base non essendo necessari.

Ovviamente il menu è stato riorganizzato secondo le ultime prospettive della iso creata. Ci tengo a ribadire che Fluxbox non essendo dotato di script nativi di aggiornamento di menu dinamico ci ha costretto a partire con un menu predefinito costruito da noi... nelle release successive saranno valutati ultertiori cambiamenti eventualmente.

Tutto quello che è stato fatto è nato pensando di dare un ambiente semplice, completo, funzionale, grafico da principio e soprattutto Slackware based.

Tanto per essere più chiari allego una screen della ultima iso x86 ... noterete fin da subito le differenze ... ma vale secondo me la pena di provarla ed eventualmente di sceglierla in pianta stabile per moltissime ragioni "e non perchè l'ho fatta io con kerd" :P ... ad ogni modo eccovi la screen di rito:

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2010/12/salix-fluxbox-01.png"><img class="alignnone size-full wp-image-1522" title="salix-fluxbox-01" src="http://linuax.altervista.org/blog/wp-content/uploads/2010/12/salix-fluxbox-01.png" alt="" width="640" height="400" srcset="https://linuax.altervista.org/blog/wp-content/uploads/2010/12/salix-fluxbox-01.png 640w, https://linuax.altervista.org/blog/wp-content/uploads/2010/12/salix-fluxbox-01-300x187.png 300w" sizes="(max-width: 640px) 100vw, 640px" /></a></pre>

## \# Link:

Sono disponibili appunto due versioni per mono e doppio processore. Allego i link:

Salix Fluxbox 13.1.2 (32-bit):  
(size: 566 MB, md5: 19af22eef8e34dd1d0396a9b479b7c59)  
Sourceforge: <a href="http://sourceforge.net/projects/salix/files/13.1/salix-fluxbox-13.1.2.iso/download" target="_blank">http://sourceforg...o/download<br /> </a> torrent: <a href="http://salix.enialis.net/i486/13.1/iso/salix-fluxbox-13.1.2.iso.torrent" target="_blank">http://salix.enia...so.torrent<br /> </a>  
Salix64 Fluxbox 13.1.2 (64-bit):  
(size: 546 MB, md5: 96f52aeb27f58c6f01a629d3483fb158)  
Sourceforge: <a href="http://sourceforge.net/projects/salix/files/13.1/salix64-fluxbox-13.1.2.iso/download" target="_blank">http://sourceforg...o/download<br /> </a> torrent: <a href="http://salix.enialis.net/x86_64/13.1/iso/salix64-fluxbox-13.1.2.iso.torrent" target="_blank">http://salix.enia...so.torrent</a>

Pagina ufficiale del progetto: <a href="http://www.salixos.org/wiki/index.php/Home" target="_blank"><strong>Salix OS</strong></a>

Pagina ufficiale del <a href="http://www.salixos.org/wiki/index.php/Salix_OS:Team" target="_blank"><strong>Team Salix OS</strong></a>

## **\# Conclusioni:**

Stiamo lavorando alla release Live CD e contiamo tempo permettendo di rilasciare la iso il più presto possibile... per il momento è da definire come in cantiere. Il riscontro attuale è stato molto gradito dagli utenti e questo non può che fare piacere... ma personalmente... io ho bisogno di un riscontro da parte di tutti "vostra" ... e spero ovviamente che proviate la distribuzione.

Sono qui per rispondere eventualmente a tutte le vostre domande e "SCUSATE IL RITARDO" :P .

## **\# END**