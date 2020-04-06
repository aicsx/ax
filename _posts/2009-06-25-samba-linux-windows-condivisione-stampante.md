---
id: 751
title: 'Samba Linux - Windows: Condivisione stampante'
date: 2009-06-25T19:05:21+01:00
author: ax
excerpt: 'Abilitare la condivisione di stampa in una rete "Linux - Windows" risulta estremamente semplice con Samba &amp; Cups.  '
layout: post
guid: http://linuax.wordpress.com/?p=751
permalink: /2009/06/25/samba-linux-windows-condivisione-stampante/
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:4:"1150";i:1;s:3:"817";i:2;s:1:"3";i:3;s:25:"width="1150" height="817"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:4:"1150";i:1;s:3:"817";i:2;s:1:"3";i:3;s:25:"width="1150" height="817"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
image_url:
  - http://linuax.wordpress.com/files/2009/06/cupsd1.png
  - http://linuax.wordpress.com/files/2009/06/cupsd1.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/06/cupsd1.png" class="alignnone size-large wp-image-753" title="cupsd"   alt="cupsd"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/06/cupsd1.png" class="alignnone size-large wp-image-753" title="cupsd"   alt="cupsd"    />'
categories:
  - How-to
  - Linux
  - Reti LAN
  - Samba
  - Slackware
tags:
  - How-to
  - Linux
  - samba linux
---
Una delle richieste dall'utente medio è di avere una stampante disponibile per più pc connessi alla stessa rete. Nel <a href="http://linuax.wordpress.com/2009/06/25/samba-linux-windows-simple-condivisione-file-e-cartelle/" target="_blank"><strong>post precedente</strong></a> abbiamo visto come configurare in maniera "simple" standard **Samba** su Slackware per accedere e/o per dare accesso alla condivisione di cartelle e file tra due pc connessi alla stessa rete. E se volessimo stampare dal pc Windows utilizzando la stampante attiva su Slackware ?

Grazie a Samba & Cups si risponde in maniera semplice anche a questo quesito.

**NB**: Sarà necessario innanzitutto assicurarsi di avere il paccheto **Cups**, noto a chi avrà già provveduto a configurare la stampante in ambiente GNU/linux.

La prima modifica da fare è proprio sul pannello di gestione del servizio **cups**. In merito, sarà  suficiente abilitare la stampante configurata in precedenza, alla condivisione,alla stampa, e all'amministrazione remota.

**NB**: Se non vedete il pannello di gestione di cups, è probabile che non sia configurato correttamente. Di norma il demone **cupsd** apre la porta TCP/631  ipp. Basterà un banale nmap per assicurarci che sia attivo e funzionante sulla linuxbox:

<pre>~$ nmap 127.0.0.1
Starting Nmap 4.60 ( http://nmap.org ) at 2009-06-25 20:24 CEST
Interesting ports on localhost (127.0.0.1):</pre>

<pre>631/tcp  open  ipp</pre>

Quindi da browser web ci connetteremo al servizio tramite l'indirizzo locale e la porta aperta dallo stesso:

<pre>http://localhost:631/</pre>

e modificheremo le impostazioni nella sezione **_Amministrazione_** come da screen:

<pre><img class="alignnone size-large wp-image-753" title="cupsd" src="http://linuax.files.wordpress.com/2009/06/cupsd1.png?w=1024" alt="cupsd" width="614" height="436" /></pre>

Spuntate le opzioni necessarie, provvederemo a modificare il config di samba per abilitare la nostra stampante alla condivisione nella rete. Quindi aggiungeremo ai parametri già presenti la sezione relativa alla stampante:

<pre>~# vi /etc/samba/smb.conf
[brother]
path=/var/spool/cups
public = yes
guest ok = yes
printable = yes</pre>

**NB**: Il parametro **[brother]** va sostituito con il nome della stampante utilizzata da voi e settata precedentemente nel pannello web di cups ****nella sezione **_stampanti_**.

Salvata la nuova configurazione non resterà che restartare Samba.

<pre>~# /etc/rc.d/rc.samba restart</pre>

**Problematiche comuni:**

- Una volta settato il lato server, non resta che aggiungere la stampante di rete sul Pc Windows. Non di rado però il processo di stampa fallisce, questo perchè nel 90% dei casi la directory incaricata di associare i processi di stampa _/var/spool/cups/_ non ha i permessi giusti per accogliere la richiesta di stampa dal computer **Windows** connesso in rete. Ecco perchè conviene mediare tramite chmod:

<pre>~# chmod 777 /var/spool/cups/</pre>

- Un'altro degli errori frequenti risulta essere la codifica del charset di sistema. Supponiamo ad esempio che la nostra **Slackware** sia settata con charset "ISO-8859-15" e che Windows da cui effettuiamo l'operazione di stampa utilizzi come (praticamente è nella realtà) il charset "UTF-8".

NOTA: Per chi non lo sapesse il charset è l'equivalente della codifica dei caratteri ascii e degli accenti.

Tornando a noi, il risultato sarebbe che ci ritroveremo nel file log di cups **/var/log/cups/error_log** un messaggio simile al seguente:

<pre>E [24/Jun/2009:22:10:07 +0200] Unsupported character set "iso-8859-15"!</pre>

Come è intuibile il risultato è il non avvio del processo di stampa; per ovviare anche a questo inconveniente basterà modificare il configure di samba preesistente aggiungendo le stringhe relative al charset da utilizzare:

<pre>~# vi /etc/samba/smb.conf
display charset = ISO-8859-15
unix charset = ISO-8859-15

~# /etc/rc.d/rc.samba restart</pre>

\# End