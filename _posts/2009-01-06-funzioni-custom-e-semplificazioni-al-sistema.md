---
id: 215
title: Funzioni custom e semplificazioni al sistema.
date: 2009-01-06T23:31:12+01:00
author: ax
excerpt: Aggiungere funzioni personalizzate per raggiungere lo stesso risultato in maniera più veloce.
layout: post
guid: http://linuax.wordpress.com/?p=215
permalink: /2009/01/06/funzioni-custom-e-semplificazioni-al-sistema/
categories:
  - Linux
  - terminal
  - Tips
tags:
  - Linux
  - script
  - Tips
---
Molto spesso può essere estremamente comodo, definire una serie di acronimi, parole chiave, per effettuare una serie di attività molto comuni e decisamente ripetitive. I metodi sono vari, come ad esempio inserendo negli opportuni file di profiling **.bashrc** o **.bash_profile** nella propria $HOME una serie di alias, come ad esempio:

<pre>alias ll ='ls -lart'</pre>

L'alternativa però può essere quella di definire un file functions **/etc/functions** dove raccogliere tutta una serie di funzioni che velocizzano o semplificano le più diverse attività, esempio:

<pre># funzione guarda

function guarda()
{
        cd /$1/;
        ls .;
}

# inserire altre sotto</pre>

**NB:** Per abilitare la lettura delle funzioni non bisogna dimenticare di addare l'esecuzione del file di funzioni creato nel profile generale della nostra slackware,così da abilitarlo per tutti gli utenti. Quindi aggiungete la stringa in **/etc/profile:**

<pre>. /etc/functions</pre>

Il  risultato finale sarà quello di ovviare ogni volta ai soliti **cd $DIR** e **ls $DIR** con la sola funzione implementata:

<pre>~$ guarda /tmp</pre>

ci restituirà il contenuto della nostra directory /tmp  .**  
** 

\# END**  
** 

**  
**