---
id: 904
title: 'Openbox menu: tips di personalizzazione mediante Obmenu'
date: 2009-11-08T16:44:56+01:00
author: ax
excerpt: |
  <p>Obmenu è uno strumento che per alcuni può risultare indispensabile per la personalizzazione del menu di OpenBox mediante grafica. La sua forza in realtà non è la medesima funzione, ma sta nell'implementare 4 pipes-menu differenti che opportunamente richiamati contribuiscono a migliorare l'usabilità e l'interfaccia del wm.</p>
layout: post
guid: http://linuax.wordpress.com/?p=904
permalink: /2009/11/08/openbox-menu-tips-di-personalizzazione-mediante-obmenu/
image_url:
  - http://linuax.wordpress.com/files/2009/11/xdg-men_box.png
  - http://linuax.wordpress.com/files/2009/11/xdg-men_box.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/11/xdg-men_box.png" class="alignnone size-full wp-image-905" title="xdg-men_box"   alt="xdg-men_box"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/11/xdg-men_box.png" class="alignnone size-full wp-image-905" title="xdg-men_box"   alt="xdg-men_box"    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:3:"450";i:1;s:3:"400";i:2;s:1:"3";i:3;s:24:"width="450" height="400"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"450";i:1;s:3:"400";i:2;s:1:"3";i:3;s:24:"width="450" height="400"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
categories:
  - Arch
  - How-to
  - Linux
  - menu
  - Openbox
  - Slackware
  - Tips
tags:
  - Linux
  - menu
  - obmenu
  - openbox
  - Tips
  - wm
  - X
---
Nei post precedenti abbiamo visto in dettaglio le specifiche generali di questo splendido wm: **OpenBox.**

Volutamente fino ad ora non c'è stato nessun accenno _particolare_ alle specifiche di **Menu**. L'installazione di <a href="http://obmenu.sourceforge.net/" target="_blank"><strong>obmenu</strong></a> infatti _(strumento che ci consente di personalizzare in modalità grafica il menu di openbox)_ prevede di default l'installazione di una serie di strumenti che se opportunamente usati contribuiscono ad abbellire e rendere decisamente più usabile il nostro menu personalizzato attraverso l'aggiunta di **4** **pipes-menu** differenti.

<pre><strong>obm-dir</strong>
<strong>obm-moz</strong>
<strong>obm-nav</strong>
<strong>obm-xdg</strong></pre>

**NB**: I seguenti applicativi fanno tutti parte del pacchetto **obmenu**. Ogni binario deve essere richiamato opportunamente nel menu di openbox mediante stringa.

Vediamo ****come richiamare ogni binario e cerchiamo di capire che tipo di pipe-menu crea.

  * **obm-xdg**:

Il più usato risulta sicuramente **obm-xdg**. Opportunamente richiamato aggiunge al menu di base di OpenBox un sottomenu contenente tutte le applicazioni GTK/GNOME installate nel sistema. Per richiamare questo sottomenu è sufficiente aggiungere nella posizione che preferiamo del file di riferimento_ **(~/.config/openbox/menu.xml)**_ del menu di openbox la seguente stringa di esempio:

<pre>&lt;menu execute="obm-xdg" id="xdg-menu" label="Applicazioni"/&gt;</pre>

E' chiaro che il **label** e l'**id** sono personalizzabili a proprio piacimento.

**NB**: solitamente il richiamo di questo pipe-menu pesca le applicazioni installate nel sistema dalla directory di riferimento **/usr/share/applications**. In questa directory risiedono tutti i file ***.desktop** dei programmi installati. Se il menù non dovesse contenere una delle  applicazioni installate nel sistema sarà sufficiente creare il file **esempio.desktop** contenete le direttive relative al programma, come da esempio seguente:

<pre>~# vi /usr/share/applications/firefox.desktop
[Desktop Entry]
Type=Application
Name=firefox
GenericName=Firefox - Web Browser
Comment=Firefox - Web Browser
TryExec=firefox
Exec=firefox
Categories=Application;Network;</pre>

Il risultato è un sottomenu "**Applicazioni**".

&nbsp;

<span style="color: #ff0000;"><strong>NB</strong></span>: la corretta implementazione di questo menu di applicazioni attraverso _**obm-xdg**_ implica come dipendenza l'installazione del pacchetto **"gnome-menus"**.

  * **obm-nav:**

Così come quello precedente obm-nav opportunamente richiamato aggiunge al menu di base di OpenBox un sottomenu per esplorare la cartella specificata. La stringa per richiamare questo pipe-menu è come da esempio:

<pre>&lt;menu execute="obm-nav /var pcmanfm urxvt" id="var" label="/var"/&gt;</pre>

**NB**: per esplorare ed eventualmente aprire i file bisogna specificare il tipo di emulatore di shell da usare _(aterm,eterm,urxvt,mrxvt,etc)_ e che tipo di programma di esplorazione utilizzare per aprire la directory esplorata _(thunar,pcmanfm,rox,nautilus, etc)_ .

  * **obm-moz:**

Questo pipe-menu mostra un sottomenu contenente i segnalibri (preferiti) di firefox. Va richiamato come da esempio:

<pre>&lt;menu execute="obm-moz" id="bookmark" label="Bookmark"/&gt;</pre>

  * **obm-dir:**

Questo ultimo pipe-menu aggiunge un sottomenu che ordina tutti i file di una directory per nome e apre gli stessi con il programma specificato. Solitamente è utilizzato per le directory contenenti le immagini, come da esempio:

<pre>&lt;menu execute="obm-dir /home/utente/immagini 'fbsetbg'" id="image" label="immagini"/&gt;</pre>

**NB**: in questo caso gli si dice di settare a sfondo background l'immagine selezionata dalla directory _**/home/utente/immagini**_ avviando l'applicativo **fbsetbg** su di essa.

Tutte le informazioni trattate sono reperibili in forma ufficiale da link di documentazione <a href="http://obmenu.sourceforge.net/pipes-help" target="_blank">pipes-help</a> di **obmenu**.

\# End