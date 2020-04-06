---
id: 887
title: 'Problematiche comuni legate ad irssi: bindkeys'
date: 2009-10-26T09:00:49+01:00
author: ax
excerpt: |
  Irssi implementa molte funzioni, per "i più" nascosti. Tra queste, bisogna sicuramente ricordare quella relativa al bind keys. E' un dato di fatto infatti che siano proprio queste le cose che scoraggiano il nuovo utente all'utilizzo permanente di questo magnifico client testuale. In questo mini-post si descrive come semplificare l'esistenza di chi non è stato attento a leggere la documentazione relativa con gli stessi semplici comandi che irssi ci mette a disposizione.
layout: post
guid: http://linuax.wordpress.com/?p=887
permalink: /2009/10/26/problematiche-comuni-legate-ad-irssi-bindkeys/
image_size:
  - ""
  - ""
categories:
  - How-to
  - irssi
  - Linux
  - terminal
  - Tips
tags:
  - bindkeys
  - irc
  - irssi
  - Linux
  - script
---
Mi sono accorto, dopo la scrittura del post relativo all'utilizzo generale di **irssi**, che molto spesso possono verificarsi degli errori da parte del client nel bindare in modo automatico i tasti di esecuzione relativi alle finestre da selezionare con le combinazioni di tasti **mod1 + []** .

**NB**: **mod1** nel **keybinding** si riferisce al tasto "**ALT**" seguito dalla combinazione associata prescelta. Un esempio di facile comprensione può essere il file **~/.fluxbox/keys** utilizzato nella customizzazione di **Fluxbox** che serve proprio ad associare combinazioni di tasti a programmi e/o funzioni.

In particolare uno dei problemi comuni diffusi risulta essere da parte del client **irssi** il mancato selezionamento di finestre che superano il limite default dei 19 "canali e/o finestre aperte". Noterete infatti che di default il client associa la combinazione di tasti:

<pre>ALT + [0-9]</pre>

**NB**: 0-9 si riferisce al range di numeri da 0 a 9. Il client associa questo range alle prime 10 finistre aperte.

ALT + [q-o]

**NB**:  Si riferisce alle lettere selezionate nel range, in specifico: q, w, e, r, t, y, u, i, o . Il client associa a questo range le successive 9 finestre aperte.

In casi particolari, per superamento di questo _"limite"_ il client smette di associare i tasti assegnati alle finestre. Di conseguenza risulta impossibile spostarsi all'interno delle finestre dalla 20 a salire.

Per ovviare a questo _"limite"_, che per inciso (limite non è, visto che è solo una configurazione settata in maniera default del client, ma che può essere largamente modificata), si possono associare manualmente i tasti che più si preferiscono con un semplice comando direttamente dal client in esecuzione.

<pre>/bind meta-a change_window 20</pre>

In questo modo non si fa altro che associare la finestra numero 20 di _**chat-query-window**_ alla combinazione di tasti **ALT + a**.

Allo stesso modo è consigliato quindi associare le successive finestre:

<pre>/bind meta-s change_window 21
/bind meta-d change_window 22
/bind meta-f change_window 23
...
/bind meta-k change_window 27</pre>

Il risultato sarà l'associazione del range:

<pre>ALT + [a-k]</pre>

**NB**: per le finestre 20-27. Si può continuare ovviamente, e si può scegliere la combinazione di tasti che si preferisce.

Per rendere permanenti le nostre modifiche non resta che salvare prima di chiudere irssi:

<pre>/save</pre>

\# End