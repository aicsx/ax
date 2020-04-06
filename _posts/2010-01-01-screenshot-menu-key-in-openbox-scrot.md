---
id: 957
title: 'Screenshot menu " key in OpenBox: scrot.'
date: 2010-01-01T07:00:06+01:00
author: ax
excerpt: |
  <p>Abilitazione del menu e delle combinazioni di tasti utili per effettuare una Screenshot del nostro desktop con OpenBox mediante l'ausilio di un applicativo esterno quale può essere: scrot.</p>
layout: post
guid: http://linuax.wordpress.com/?p=957
permalink: /2010/01/01/screenshot-menu-key-in-openbox-scrot/
categories:
  - Arch
  - How-to
  - Linux
  - menu
  - Openbox
  - Slackware
  - Tips
tags:
  - How-to
  - menu
  - openbox
  - slackware
  - Tips
  - wm
  - X
---
Negli ultimi giorni ho ricevuto un bel pò di richieste di aiuto riguardo la fase post nstallazione e configurazione di **WM** come **OpenBox** e **PekWM**. Quindi, da oggi partiranno una serie di mini articoli che trattano degli aspetti non affrontati nei post precedenti. In questo post, vediamo in maniera semplice come abilitare un **submenu** che implementi la funzione di **Screenshot** magari seguita anche da una combinazione di tasti che richiamino la stessa funzione in **OpenBox**.

Per arrivare al risultato sperato ci sono molte strade, la più semplice richiede l'utilizzo di un programma esterno (ne esistono tanti) ma per comodità e facilità di utilizzo parlerò brevemente di: <a href="http://freshmeat.net/projects/scrot/" target="_blank"><strong>scrot</strong></a>.

**Scrot** è un applicativo fatto per creare degli snapshot di finestre attive, di aree parziali e/o anche totali del desktop in ambienti GNU/Linux attraverso l'ausilio di **Imlib2**. Recementemente questo progetto sta andando a scomparire per lasciare spazio a tanti altri successori. Io personalmente ritengo che sia di una comodità disarmante. Per quel che riguarda lo scopo finale; partiamo dall'installazione di scrot per poi arrivare all'implementazione dello stesso nel menu di **OpenBox**, così da effettuare i nostri screen in modo istantaneo, veloce, magari a tempo.

L'installazione su **Slackware** è come al solito molto semplice:

<pre>~$ wget <a href="http://www.linuxsoft.cz/en/redirect.php?id_download=10615" target="_blank">scrot</a>
~$ tar zxvf scrot-0.8.tar.gz
~$ cd scrot-0.8/
~$ ./configure
~$ make
~$ su -
Password:
~# make install</pre>

Oppure più semplicemente tramite pacchetto precompilato su **Arch** Linux:

<pre>~# pacman -Sy scrot</pre>

Installato l'applicativo che ci serviva dobbiamo richiamarlo nel sottomenu di **OpenBox**, quindi provvediamo ad aggiungere i comandi prescelti per fare le screenshots come da esempio:

**NB**: la directory di riferimento per modificare i menu resta ovviamente quella di OpenBox, vale a dire: **~/.config/openbox/** ; ovviamente il file da modificare è  **menu.xml** . Aggiungiamo come da esempio le seguenti stringhe:

<pre>&lt;menu id="scrot" label="Screenshot"&gt;
  &lt;item label="Screenshot (PrtSc)"&gt;
   &lt;action name="Execute"&gt;
    &lt;execute&gt;
      scrot -e 'mv $f ~/immagini/'
    &lt;/execute&gt;
   &lt;/action&gt;
  &lt;/item&gt;
 &lt;item label="Screenshot delay 5"&gt;
   &lt;action name="Execute"&gt;
    &lt;execute&gt;
      scrot -d 5 -e 'mv $f ~/immagini/'
    &lt;/execute&gt;
   &lt;/action&gt;
  &lt;/item&gt;
 &lt;/menu&gt;</pre>

**NB**: nell'esempio creeremo un sottomenu di nome **Screenshot** nel menu desktop di **OpenBox** che avrà due opzioni:

  * Una semplice screen istantanea
  * Una screen stabilita a tempo con il parametro_ **delay 5**_ (che indica i secondi da attendere dallo start della screen).

Si è scelto di aggiungere le opzioni che spostano la screen appena creata nella cartella utente _**~/immagini/**_ .Ovviamente sono opzioni modificabili. Si consiglia quindi di leggere sempre il solito **man** dell'applicativo e/o anche dare uno sguardo diretto al comando:

<pre>~$ scrot --help</pre>

Per capirci meglio il risultato sarà simile alla screen:

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2010/01/scrot_openbox.jpg"><img class="alignnone size-full wp-image-1530" title="scrot_openbox" src="http://linuax.altervista.org/blog/wp-content/uploads/2010/01/scrot_openbox.jpg" alt="" width="197" height="256" /></a></pre>

Stesso identico risultato finale può essere quello di aggiungere il richiamo di **scrot** mediante combinazioni di tasti e non più tramite menu. Questa volta il file conf da modificare sarà: **~/.config/openbox/rc.xml** aggiungendo le stringhe come da esempio:

<pre>&lt;keybind key="Print"&gt;
  &lt;action name="Execute"&gt;
   &lt;execute&gt;scrot -e 'mv $f ~/immagini/'&lt;/execute&gt;
  &lt;/action&gt;
 &lt;/keybind&gt;
&lt;keybind key="C-Print"&gt;
  &lt;action name="Execute"&gt;
   &lt;execute&gt;scrot -d 5 -e 'mv $f ~/immagini/'&lt;/execute&gt;
  &lt;/action&gt;
 &lt;/keybind&gt;</pre>

In questo esempio il tasto:

<pre>"STAMP - RSIST"</pre>

(per intenderci quello in alto a destra sulla tastiera che solitamente richiama automaticamente una screenshot) ci restituirà una screen istantanea del desktop.

Nel secondo keybind settato invece la combinazione di tasti:

<pre>Ctrl + Stamp - RSIST</pre>

scatterà una screenshot al termine dei 5 secondi di delay, così come avveniva per il menu precedentemente settato.

**NB**: l'aggiunta di scrot non implica necessariamente l'utilizzo del keybindings per i tasti prescelti e viceversa. Sono solo esempi per arrivare allo stesso risultato. Se alcuni termini e/o alcune parti del post non fossero chiare si consiglia di dare uno sguardo agli articoli riguardo <a href="http://linuax.altervista.org/blog/2009/10/25/box-wm-ultraleggero-openbox/" target="_blank"><strong>OpenBox</strong></a> ed il <a href="http://linuax.altervista.org/blog/2009/10/27/applicazionidesktopbindkeys-openbox-tips/" target="_blank"><strong>keybindings </strong></a>dello stesso.

\# End