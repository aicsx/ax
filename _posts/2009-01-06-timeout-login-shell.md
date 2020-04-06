---
id: 213
title: Timeout login shell
date: 2009-01-06T22:08:23+01:00
author: ax
excerpt: 'Amministrazione server: logout automatico'
layout: post
guid: http://linuax.wordpress.com/?p=213
permalink: /2009/01/06/timeout-login-shell/
categories:
  - How-to
  - Linux
  - Tips
tags:
  - How-to
  - Linux
  - Tips
---
Spesso nei server remoti, soprattutto se pieni di utenza capita che un gran numero di shell via ssh rimangano aperte senza alcun uso, per pigrizia o per disattenzione. In entrambi i casi, ma special modo nel secondo, il sistema tiene memoria di queste connessioni lasciando processi  attivi anche se realmente morti sulla linuxbox. Per ovviare a questo, è possibile settare un timeout che automaticamente scolleghi l'utente se esso non ha effettuato alcuna attività entro il termine prescelto.

Nei sistemi attuali questa configurazione esiste di default in openssh che tramite la configurazione nativa di  sshd.conf provvede a chiudere le shell inutilizzate dopo tot tempo. Ma anche in questo caso il processo risulta rimanere in pending.

Il sistema più banale e semplice è settare il timeout direttamente per la shell in uso e bypassare il sistema di controllo del demone ssh.

I modi sono due a seconda della shell di riferimento; per la Bash (sh) e la Korne Shell (ksc) viene utilizzata la variabile d'ambiente **TMOUT** impostata con il valore di idle desiderato espresso in secondi.

Esempio:

<pre>~# export TMOUT=60</pre>

Per la C shell (csh) invece si usa la variabile d'ambiente autologout, dove a differenza della prima il tempo di logout viene espresso in minuti.

Esempio:

<pre>~# set autologout=1</pre>

#

<pre></pre>