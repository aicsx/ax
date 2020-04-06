---
id: 780
title: Traduzione e localizzazione italiana del Kernel Linux
date: 2009-08-02T16:15:37+01:00
author: ax
excerpt: "Uno degli aspetti che solitamente preoccupa l'utente medio risulta essere la compilazione del Kernel Linux. A tal proposito, esiste un progetto che mira proprio a rendere semplice la compilazione del kernel linux mediante la traduzione delle singole voci."
layout: post
guid: http://linuax.wordpress.com/?p=780
permalink: /2009/08/02/traduzione-e-localizzazione-italiana-del-kernel-linux/
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/08/kernel_i18n.png" class="alignnone size-full wp-image-781" title="kernel_i18n"   alt="kernel_i18n"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/08/kernel_i18n.png" class="alignnone size-full wp-image-781" title="kernel_i18n"   alt="kernel_i18n"    />'
image_url:
  - http://linuax.wordpress.com/files/2009/08/kernel_i18n.png
  - http://linuax.wordpress.com/files/2009/08/kernel_i18n.png
image_size:
  - 'a:6:{i:0;s:3:"700";i:1;s:3:"424";i:2;s:1:"3";i:3;s:24:"width="700" height="424"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"700";i:1;s:3:"424";i:2;s:1:"3";i:3;s:24:"width="700" height="424"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
categories:
  - How-to
  - Kernel
  - Linux
  - Tips
tags:
  - How-to
  - kernel
  - Linux
  - module
  - slackware
---
Uno degli aspetti che solitamente preoccupa l'utente medio risulta essere stranamente la compilazione del **Kernel** Linux. Non si capisce come mai, essendo questo un passo quasi _obbligato_ oltre che  essenziale al fine di snellire i moduli necessari al corretto funzionamento dell'hardware, oltre che all'implementazione di molteplici patch che possono essere dirette all'hardware o semplicemente alla sicurezza.

Parlando qui e li, in giro, mi sono accorto, che tutto questo gran problema molto spesso è generato dalla traduzione e dalla non localizzazione dello stesso che solitamente tende a generare un pò di confusione nell'user inesperto alle prime armi. A tal proposito, in Italia, esiste un progetto che mira proprio a rendere semplice la compilazione del **kernel** mediante la traduzione delle singole voci.

<a href="http://massimo.solira.org/pcikl/index.html" target="_blank"><strong>Link Ufficiale Progetto</strong></a>

L'operazione è estremamente semplice e risulta estremamente veloce in base al metodo scelto che cambia a seconda del kernel in uso, come spiegato sul sito ufficiale del progetto. Tanto per "siccome ci sono sempre i pigri che non amano aprire troppi link (potrebbe essere rischioso per il loro bene mentale leggere troppe pagine)", vediamo in dettaglio.

**Metodo 1° - mediante patch da applicare alle versioni kernel da 2.6.7  a 2.6.24**

**NB:** La versione di riferimento del kernel è solo un esempio.

<pre>~$ wget  <a href="http://massimo.solira.org/pcikl/files/patch-2.6.18-conf_it.2.bz2">patch-2.6.18-conf_it.2.bz2</a></pre>

Scaricata e scompattata la patch, provvedermo a spostarla per effettuare la compilazione standard:

<pre>~$ bunzip2 patch-2.6.18-conf_it.2.bz2
~$ su
Password:
~# cp patch-2.6.18-conf_it.2 /usr/src/linux</pre>

**NB**: La directory ospitante i sorgenti del kernel è ovviamente un link simbolico. Se non fosse presente il link diretto alla versione di kernel scelto, il percorso di riferimento dei sorgenti del kernel sarà come da esempio:  _**/usr/src/linux-2.6.18**_

Applichiamo la patch:

<pre>~# cd /usr/src/linux
~# patch -p1 &lt; patch-2.6.18-conf_it.2</pre>

**NB**: Dopo aver scaricato la versione appropiata per la versione del kernel in uso dall'apposita sezione download del progetto, basta applicarla e ricompilare il kernel con il metodo _standard_. Chiaramente il processo và fatto su <a href="http://www.kernel.org/" target="_blank">kernel (ufficiali).</a>

**NB**: se non si conoscono i metodi di compilazione del kernel linux, consultare l'articolo nell'apposita sezione del blog.

**Metodo 2° - mediante file catologo applicabile per versioni kernel da 2.6.12 a 2.6.30**

Rispetto al precedente metodo non richiede ricompilazione del kernel. Questo perchè a partire dalla versione 2.6.12 i kernel sono stati dotati del supporto dell'internazionalizzazione delle interfaccie di configurazione.

**NB**: A differenza dell'altro metodo è possibile applicare il file catalogo di riferimento anche ai kernel predefiniti nelle varie distro. Non è necessario partire da un kernel vanilla.

Scarichiamo e scompattiamo il file catalogo per <a href="http://massimo.solira.org/pcikl/download.html" target="_blank">la versione <strong>in uso</strong></a>; supponiamo per un kernel 2.6.24.x ad esempio predefinito in Slackware 12.1 :

<pre>~$ wget <a href="http://massimo.solira.org/pcikl/files/linux-2.6.24.mo.bz2">linux-2.6.24.mo.bz2</a>
~$ bunzip2 linux-2.6.24.mo.bz2</pre>

Rinominiamo e spostiamo il file catalogo italiano nella directory di riferimento per la traduzione delle voci del kernel:

<pre>~$ su
Password:
~# mv linux-2.6.24.mo /usr/share/locale/it/LC_MESSAGES/linux.mo</pre>

Il risultato finale sarà che alla prima e/o prossima compilazione otterremo un menu kernel di configurazione tradotto in tutte le sue voci e quindi facilmente intuibile dai più o meno esperti.

<pre><img class="alignnone size-full wp-image-781" title="kernel_i18n" src="http://linuax.files.wordpress.com/2009/08/kernel_i18n.png" alt="kernel_i18n" width="455" height="275" /></pre>

\# End