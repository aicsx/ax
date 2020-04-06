---
id: 376
title: 'Most: pagine man a colori'
date: 2009-02-23T21:32:20+01:00
author: ax
excerpt: "I colori possono essere una mano sacra, in special modo per l'utente che cerca di districarsi nella miriade di applicativi e di comandi della linux box. Lo strumento che ci permette appunto di colorare le pagine di documentazione di man è  most."
layout: post
guid: http://linuax.wordpress.com/?p=376
permalink: /2009/02/23/most-pagine-man-a-colori/
categories:
  - Linux
  - Programmi
  - terminal
  - Tips
tags:
  - Linux
  - man
  - most
  - Tips
---
Tutti gli amanti dei sistemi unix & unix-like conoscono l'importanza del comando _**man**_. Spesso però capita di non venire a capo di nulla e di perdersi all'interno di tale documentazione. Personalmente non sono un amante dei colori però riconosco che per facilità di comprensione e di diversificazione di comandi e commenti, a volte, i colori possono essere una mano sacra, in special modo per l'utente "tipo" che cerca di districarsi nella miriade di applicativi e di comandi della linux box. Lo strumento che ci permette appunto di colorare le pagine di documentazione di _man_ è  **most**.

Presente di default nella vostra adorata slackware, most non fa altro che colorare in maniera facilitata le pagine man di documentazione ai comandi.

**NB**: l'unica modifica da fare con il vostro editor preferito  in Slackware 12.1 è sul file **/usr/lib/man.conf** cambiando le variabili:

  * PAGER
  * BROWSER

come segue:

<pre>PAGER           /usr/bin/most -s
BROWSER         /usr/bin/most -s</pre>

Quindi un "_**man ls**_" ci restituirà:

<img class="alignnone size-full wp-image-378" title="most1" src="http://linuax.files.wordpress.com/2009/02/most1.jpg" alt="most1" width="455" height="275" /> 

**NB**: se non fosse ciò che cercavate "come colori", basterà modificare e/o creare _**~/.mostrc**_ come da esempio:

<pre>% Color settings

color normal lightgray black
color status yellow blue
color underline yellow black
color overstrike brightblue black</pre>

**NB**: In alternativa è possibile avere le pagine man colorate tramite less. Questo metodo ha molte più caratteristiche rispetto a most quindi è più gestibile da utenti avanzati. Occorre semplicemente aggiungere le seguenti istruzioni al file .SHELLrc come esempio:

<pre>export LESS_TERMCAP_mb=$'E[01;31m'
export LESS_TERMCAP_md=$'E[01;31m'
export LESS_TERMCAP_me=$'E[0m'
export LESS_TERMCAP_se=$'E[0m'
export LESS_TERMCAP_so=$'E[01;44;33m'
export LESS_TERMCAP_ue=$'E[0m'
export LESS_TERMCAP_us=$'E[01;32m'</pre>

\# End