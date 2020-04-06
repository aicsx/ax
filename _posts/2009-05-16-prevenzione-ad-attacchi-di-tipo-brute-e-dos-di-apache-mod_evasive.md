---
id: 718
title: 'Prevenzione attacchi DoS apache: mod_evasive'
date: 2009-05-16T12:33:43+01:00
author: ax
excerpt: |
  <p>mod_evasive è un modulo per Apache v1.3 - v2.0 che ha lo scopo di ridurre e/o "filtrare" mediante semplici rules attacchi di tipo: HTTP DOS, DDoS, brute force.</p>
layout: post
guid: http://linuax.wordpress.com/?p=718
permalink: /2009/05/16/prevenzione-ad-attacchi-di-tipo-brute-e-dos-di-apache-mod_evasive/
categories:
  - Linux
  - Sicurezza
tags:
  - apache
  - ddos
  - Linux
  - mod_evasive
---
Sempre più spesso mi ritrovo con una miriade di log che riguardano attacchi di tipo _brute force_ e/o _DDoS_ sul server web. Infastidito da questa cosa, mi sono ricordato di un modulo per **Apache** la cui funzione è quella di abilitare un controllo sulle richieste verso l'intero sito e/o la singola pagina in un arco di tempo definibile;

Quando le richieste superano un determinato margine, l'ip in questione viene inserito in blacklist, e da quel momento il server web restituisce una pagina di  errore  di tipo **403 "Non permesso"** per un lasso di tempo, anch'esso definibile quindi flessibile. Diciamo che si tratta di una sorta di _limit-burst (riferito ad iptables)_ per il nostro server web Apache. Si tratta del modulo _**mod_evasive**_.

L'installazione prevede pochi semplici passi:

**NB**: la procedura è riferita ad _**Apache 2.0**_. Per ulteriori versioni di Apache e implementazione di tale modulo basta consultare il README.

<pre>~$ wget <a href="http://www.zdziarski.com/projects/mod_evasive/mod_evasive_1.10.1.tar.gz" target="_blank">mod_evasive_1.10.1.tar.gz</a>
~$ tar zxvf mod_evasive_1.10.1.tar.gz
~$ cd mod_evasive
~$ su -
~# /usr/sbin/apxs -cia mod_evasive20.c</pre>

Installato il modulo mod_evasive si passa alla configurazione. Usate il vostro editor preferito per configurare il file generico httpd.conf. Il percorso della directory  apache chiaramente cambia da distro a distro.

<pre><span style="color: #000000;">~# vi /etc/apache2/httpd.conf</span></pre>

> <pre>&lt;IfModule Mod_evasive20.c&gt;
<span class="google-src-text" style="direction: rtl; text-align: right;">DOSHashTableSize   3097</span>
<span class="google-src-text" style="direction: rtl; text-align: right;">DOSPageCount        2</span>
<span class="google-src-text" style="direction: rtl; text-align: right;">DOSSiteCount        50</span>
<span class="google-src-text" style="direction: rtl; text-align: right;">DOSPageInterval     1</span>
<span class="google-src-text" style="direction: rtl; text-align: right;">DOSSiteInterval     1</span>
<span class="google-src-text" style="direction: rtl; text-align: right;">DOSBlockingPeriod   10</span>
<span class="google-src-text" style="direction: rtl; text-align: right;">&lt;/IfModule&gt;</span></pre>

Dove i parametri sono:

  * <pre>DOSHashTableSize: dimensione della tabella di hash, più è grande più memoria è richiesta, ma più veloce saranno i lookups.</pre>

  * <pre>DOSPageCount:  numero di richieste, per la stessa pagina, massime concesse prima che l'indirizzo finisca in blacklist.</pre>

  * <pre>DOSSiteCount: numero di richieste, per l'intero sito, massime concesse prima che l'indirizzo finisca in blacklist.</pre>

  * <pre>DOSPageInterval: l'arco di tempo, in secondi, entro il quale devono rientrare le richieste massime definite nel pageconut.</pre>

  * <pre>DOSSiteInterval: l'arco di tempo, in secondi, entro il quale devono rientrare le richieste massime definite nel sitecount.</pre>

  * <pre>DOSBlockingPeriod: l'arco di tempo, in secondi, durante il quale verrà restituito un errore di tipo 403 all'ip blacklistato, il timer verrà resettato qualora durante il conteggio l'ip faccia un altra hit, ricevendo quindi, l'errore per più tempo ancora.</pre>

**NB**: I parametri sono soggettivi in base alle esigenze.

Modificato e salvato il conf non resta che riavviare il servizio:

<pre>~# /etc/rc.d/rc.httpd restart</pre>

**NB**: il percorso di riavvio per i  demoni di servizzi in sistemi GNU/linux cambia come detto precedentemente da distro a distro. Nello specifico è riferito alla directory _/etc/rc.d/_ di slackware.

Link ufficiale del modulo: [**mod_evasive** ****](http://www.zdziarski.com/blog/?page_id=442)
