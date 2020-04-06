---
id: 857
title: '*box wm ultraleggero: OpenBox'
date: 2009-10-25T10:46:24+01:00
author: ax
excerpt: |
  <p>Guida generale all'uso e alla customizzazione del wm della famiglia *box: OpenBox.</p>
layout: post
guid: http://linuax.wordpress.com/?p=857
permalink: /2009/10/25/box-wm-ultraleggero-openbox/
image_url:
  - http://linuax.wordpress.com/files/2009/10/screen_ax_openbox.png
  - http://linuax.wordpress.com/files/2009/10/screen_ax_openbox.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/10/screen_ax_openbox.png" class="alignnone" title="ax_openbox.png"   alt=""    />'
  - '<img src="http://linuax.wordpress.com/files/2009/10/screen_ax_openbox.png" class="alignnone" title="ax_openbox.png"   alt=""    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:4:"1152";i:1;s:3:"864";i:2;s:1:"3";i:3;s:25:"width="1152" height="864"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:4:"1152";i:1;s:3:"864";i:2;s:1:"3";i:3;s:25:"width="1152" height="864"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
categories:
  - Arch
  - How-to
  - Linux
  - Openbox
  - Slackware
  - Wm
tags:
  - desktop
  - Linux
  - openbox
  - slackware
  - wm
  - X
---
Dopo anni, stanco del mio ambiente desktop _nativo_: **fluxbox**, ho deciso di sperimentare per curiosità un nuovo wm di _**X**_. La prima scelta è ricaduta su **<a href="http://icculus.org/openbox/index.php/Main_Page" target="_blank">openbox</a>.** Scelta dettata anche dal fatto che mi sono accorto della _quasi moda_ che si stava sviluppando attorno a questo wm che continua ad essere tutt'ora poco conosciuto in dettaglio.

Openbox è un wm davvero ben fatto, con un ampia possibilità di personalizzazione. L'interfaccia nativa è decisamente minimale, con mia grande sorpresa; ancora più minimale di fluxbox (cosa che non mi dispiace affatto).

**NB**: Openbox si presenta senza una **dockbar** e **traybar** (per capirci quella barra che in fluxbox al _primo avvio_ è posta in basso al centro, __ che implementa: "desktop in uso, dock,tray icons e orologio"). L'utilizzo e/o la scelta di un pannello è strettamente personale. Esistono una miriade di soluzioni al momento. Più avanti verranno elencate alcune delle soluzioni che reputo valide per carattersistiche e personalizzazione.

Chiaramente come tutte le cose ci sono dei pro e dei contro. Tanto per cominciare **Openbox** è pensato in **.xml** (linguaggio che non amo particolarmente). Detto questo, vediamo in dettaglio. L'installazione non prevede particolari attenzioni oltre ai soliti:

<pre><span style="color: #000000;">~$ ./configure NB: configure che prevede opzioni aggiuntive, vedi ./configure --help ~$ make ~$ su - Password: ~# make install</span></pre>

E' consigliata l'installazione di **<a href="http://icculus.org/openbox/index.php/ObConf:About" target="_blank">obconf</a>** che facilità l'utente nella configurazione delle parti principali del wm oltre che del focus,delle finestre, dei temi e della dockbar.

**NB**: Come sempre mi sento di consigliare la compilazione manuale dei pacchetti. In ogni caso  sono disponibili gli <a href="http://slackbuilds.org/result/?search=openbox&sv=13.0" target="_blank">Slackbuilds</a> e/o i pacchetti precompilati per Arch.

Per il primo caso non credo sia necessaria una spiegazione; idem per l'utilizzo degli slackbuilds. Nel caso siate archmuniti:

<pre>~# pacman -S openbox obconf</pre>

Per eventuali temi aggiuntivi

<pre>~# pacman -S openbox-themes</pre>

Installato, provvederemo a modificare il nostro ~/.xinitrc nel caso di slackware attraverso il solito **xwmconfig**, oppure come da esempio:

<pre>~$ vi ~/.xinitrc</pre>

<pre>exec openbox-session</pre>

<span style="color: #ff0000;"><strong>NB</strong>: <span style="color: #000000;">Se in slackware non si fosse creato il link simbolico a openbox.init richiamato da xwmconfig (normalmente non autogenerato in fase di compilazione manuale di un <strong>de</strong> e/o <strong>wm</strong>) si modificherà a mano <strong>.xinitrc </strong>come da esempio precedente oppure bisognerà creare il file di init manualmente (per file di init intendiamo la scelta di menù richiamata da xwmconfig; in slackware i vari init risiedono nella directory /etc/X11/xinit). Per dubbi consultare i post realtivi all'analogo problema su fluxbox dopo la compilazione manuale. L'articolo è presente nel blog.</span></span> Se ci fossero particolari dubbi e incertezze sono ovviamente sempre disponibile a rispondere ad una richiesta di aiuto tramite commento pubblico :> .

**BASE**:

**NB**: Prima di lanciare openbox è bene chiarire che la directory di riferimento di openbox è **~/.config/openbox**. All'interno della directory dovranno essere presenti 2 file necessari al corretto funzionamento del wm + 1 script bash, che sono rispettivamente:

<pre><strong>autostart.sh</strong> &gt; contenente le direttive dei programmi da runnare all'avvio di openbox</pre>

<pre><strong>menu.xml </strong>&gt; menu <em>classico</em> nativo</pre>

<pre><strong>rc.xml</strong> &gt; file di configurazione: generale/settaggi Keys&Mouse/proprietà applicazioni - modificabile in parte anche attraverso l'uso di obconf</pre>

**NB**: Potrebbe capitare che la directory di riferimento generale non sia stata autocreata in fase compilatoria, in tal caso:

<pre>$ mkdir -p ~/.config/openbox/</pre>

Se al primo avvio del wm non siano stati autogenerati i 2 file principali e/o manca uno di essi è sufficiente copiare quello di sistema presente nella directory **/etc/xdg/openbox/** come da esempio:

<pre>$ cp /etc/xdg/openbox/rc.xml ~/.config/openbox/rc.xml
$ cp /etc/xdg/openbox/menu.xml ~/.config/openbox/menu.xml</pre>

**NB**:  **autostart.sh** è un banale _script.sh_ che racchiude gli applicativi da runnare all'avvio di openbox, analogalmente a ciò che succedeva con il file **_~/.fluxbox/startup._**

Potreste infatti scegliere di utilizzare lo stesso file per dire ad openbox quali applicativi runnare al suo avvio come da esempio:

<pre>~$ cp ~/.fluxbox/startup ~/.config/openbox/autostart.sh</pre>

<pre>o anche tramite link simbolico:</pre>

~$ ln -s ~/.fluxbox/startup ~/.config/openbox/autostart.sh  
Lo script in ogni caso sarà nello standard classico degli script bash:

<pre>#!/bin/sh
#
# openbox startup-script:
# Lines starting with a '#' are ignored.

# Eliminare momentanea modulo pcspkr (campanella di sistema)
xset -b
# Statistiche
conky &
gkrellm &
# Aggiungere altre applicazioni e/o comandi secondo l'ordine prescelto.</pre>

Startando openbox  noteremo l'estrema snellicità e semplicità di interfaccia. A questo punto avvieremo dal menù desktop o da un terminale **obconf** per modificarei settaggi principali compresi il focus delle finestre, lo spostamento del focus attraverso il mouse etc.

**RC.XML:**

Questo file contiene le direttive principali riguardo l'associazione dei tasti e del mouse. Il file è opportunamente commentato dai tag **<!commento ->**. Nel file rc.xml risiedono anche le proprietà specifiche per le applicazioni stabilite (bordi,numero desktop,shade,sticky, etc..)

**NB**: La configurazione è davvero intuitiva. In ogni caso ci saranno altri post successivi al seguente, dove saranno viste in specifico queste funzionalità.

**MENU:**

Come detto precedentemente la configurazione del menù avviene attraverso la modifica del file **~/.config/openbox/menu.xml** con un editor preferito. Il menù è rappresentato in ordine come da esempio seguente:

<pre>&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;openbox_menu&gt;
     &lt;menu id="root-menu" label="OpenBox"&gt;   
           &lt;separator label="ax" /&gt;
              &lt;item label="Shell"&gt; &lt;action name="Execute"&gt;
                &lt;execute&gt;urxvt&lt;/execute&gt;
              &lt;/action&gt; &lt;/item&gt;
     &lt;/menu&gt;
&lt;/openbox_menu&gt;</pre>

In specifico le parti di codice e i  tag definiscono:

**NB**: A destra dei tag _"il codice racchiuso tra i caratteri_ <>_"_ ci sono i commenti _"ciò che segue il simbolo_ **!**_"_.

**NB**: I commenti non vanno inseriti nel menù di test, servono solo a far capire ogni voce a cosa si riferisce. Per facilità di comprensione le parti di menù sono state suddivise per colori. In questo modo avrete una più chiara idea di dove comincia un menu **<apertura>** e dove lo stesso viene chiuso **</chiusura>** . La differenza tra l'aprire e il chiudere una sezione in .xml sta nel precedere la chiusura del tag aperto precedentemente con il simbolo  **/** .

<pre><span style="color: #000000;">&lt;?xml version="1.0" encoding="UTF-8"?&gt; <em>!versione xml e codifica charset</em></span><span style="color: #0000ff;"> &lt;openbox_menu&gt;</span> <em><span style="color: #000000;">!apre il menu nativo di openbox</span> </em><span style="color: #99cc00;">&lt;menu id="root-menu" label="OpenBox"&gt;</span> <em><span style="color: #000000;">!definisce l'id di menu e il nome dello stesso</span> </em><span style="color: #993366;"> &lt;separator label="ax" /&gt;</span> <em> <span style="color: #000000;">!inserisce un separatore con intestazione "ax". Si usa anche per separare e titolare il menu base di openbox. </span></em>
          <span style="color: #800000;">&lt;item label="Shell"&gt;</span> <span style="color: #800000;">&lt;action name="Execute"&gt;</span> <span style="color: #000000;"><em>!titolo 1° applicazione di menu</em></span>
          <span style="color: #800000;">&lt;execute&gt;urxvt&lt;/execute&gt;</span> <span style="color: #000000;"><em>!programma relativo alla 1° applicazione di menu</em></span>
 <span style="color: #800000;"> &lt;/action&gt; &lt;/item&gt;</span> <em><span style="color: #000000;">!chiusura tag 1° applicazione di menu</span> </em><span style="color: #800000;">&lt;item label="Home"&gt; &lt;action name="Execute"&gt; <em><span style="color: #000000;">!titolo 2° applicazione</span></em> &lt;execute&gt;pcmanfm&lt;/execute&gt; <em><span style="color: #000000;">!programma relativo alla 2° applicazione </span></em> &lt;/action&gt; &lt;/item&gt; <em><span style="color: #000000;">!chiusura tag 2° applicazione di menu</span></em></span><span style="color: #339966;"><span style="color: #99cc00;"> &lt;/menu&gt;</span> </span><span style="color: #000000;"><em>!chiude il root-menu aperto precedentemente</em></span>
 <span style="color: #0000ff;">&lt;/openbox_menu&gt;</span><em> <span style="color: #000000;">!chiude definitivamente il menù openbox</span> </em></pre>

Volendo aggiungere un sottomenu contenente le voci native di openbox, compresi tools e uscita dal wm**:** 

**Esempio:**

<pre><span style="color: #000000;">&lt;?xml version="1.0" encoding="UTF-8"?&gt; <em>!versione xml e codifica charset</em></span><span style="color: #0000ff;"> &lt;openbox_menu&gt;</span> <span style="color: #000000;"><em>!apre il menu nativo di openbox</em></span>
    <span style="color: #99cc00;">&lt;menu id="root-menu" label="OpenBox"&gt;</span> <span style="color: #000000;"><em>!definisce l'id di menu e il nome dello ste</em></span><span style="color: #800080;"><span style="color: #000000;">sso</span> &lt;separator label="ax" /&gt;</span> <em> <span style="color: #000000;">!inserisce un separatore con intestazione "ax". Si usa anche per separare e titolare il menu base di openbox. </span></em>
          <span style="color: #800000;">&lt;item label="Shell"&gt;</span> <span style="color: #800000;">&lt;action name="Execute"&gt;</span> <span style="color: #000000;"><em>!titolo 1° applicazione di menu</em></span>
          <span style="color: #800000;">&lt;execute&gt;urxvt&lt;/execute&gt;</span> <span style="color: #000000;"><em>!programma relativo alla 1° applicazione di menu</em></span><span style="color: #800000;"> &lt;/action&gt; &lt;/item&gt;</span> <em><span style="color: #000000;">!chiusura tag 1° applicazione di menu</span> </em><span style="color: #800000;"> &lt;item label="Home"&gt; &lt;action name="Execute"&gt; <span style="color: #000000;"><em>!titolo 2° applicazione</em></span> &lt;execute&gt;pcmanfm&lt;/execute&gt; <span style="color: #000000;"><span style="color: #000000;"><em>!programma relativo alla 2° applicazione</em></span> </span> &lt;/action&gt; &lt;/item&gt; <span style="color: #000000;"><span style="color: #000000;"><em>!chiusura tag 2° applicazione di menu</em></span> </span></span>     <span style="color: #339966;">&lt;menu id="40" label="OpenBox"&gt;</span> <span style="color: #000000;"><em>!Nuovo menu titolato OpenBox </em></span><span style="color: #800080;">&lt;menu id="client-list-menu"/&gt;</span> <span style="color: #000000;"><em>!sottomenu desktop</em></span>
          <span style="color: #800000;">&lt;item label="ObConf"&gt; &lt;action name="Execute"&gt; <span style="color: #000000;"><em>!applicazione 1</em></span> &lt;execute&gt;obconf&lt;/execute&gt; &lt;/action&gt; &lt;/item</span><span style="color: #993366;">&gt;</span><em> <span style="color: #000000;">!chiusura applicazione 1</span></em>
           <span style="color: #800000;">&lt;item label="ObMenu"&gt; &lt;action name="Execute"&gt; <span style="color: #000000;"><em>!<span style="color: #000000;">applicazione 2 </span></em></span> &lt;execute&gt;obmenu&lt;/execute&gt; &lt;/action&gt; &lt;/item&gt;</span><em><span style="color: #000000;"> !chiusura applicazione 2</span></em>
      <span style="color: #339966;">&lt;/menu&gt;</span> <em>!chiusura sottomenu</em> <em>OpenBox</em>
        <span style="color: #800080;">&lt;separator/&gt;</span> <em>!Linea di separazione</em> semplice
 <span style="color: #800080;"> &lt;item label="Reconfigure"&gt; &lt;action name="Reconfigure"/&gt; &lt;/item&gt;</span> <span style="color: #000000;"><em>!rilegge la configurazione di openbox senza riavviare X</em></span>
 <span style="color: #800080;"> &lt;item label="Exit"&gt; &lt;action name="Exit"/&gt; &lt;/item&gt; <span style="color: #000000;"><em>!uscita</em></span></span><span style="color: #339966;"><span style="color: #99cc00;"><span style="color: #000000;">  </span> &lt;/menu&gt;</span> </span><span style="color: #000000;"><em>!chiude il root-menu aperto precedentemente</em></span>
 <span style="color: #0000ff;"> &lt;/openbox_menu&gt;</span><em> <span style="color: #000000;">!chiude il menù generale openbo</span></em></pre>

**NB**: Non lasciatevi impressionare, è molto più semplice di quel che sembra. In ogni caso, se dovreste riscontrare particolari problemi  c' è sempre l'alternativa. Esistono vari tools infatti per la configurazione di un menu completo. Gli strumenti che ci vengono messi a disposizione sono principalmente due.

Il primo è nativo: <a href="http://obmenu.sourceforge.net/" target="_blank"><strong>ObMenu</strong></a>

**NB**: stesso discorso di prima per installarlo su slackware; solite procedure. Per gli archmuniti:

<pre>~# pacman -S obmenu</pre>

Una volta installato sarà sufficiente lanciarlo ed aggiungere le voci di menu desiderate.

Il secondo è utile allo scopo finale, anche se non nasce specificamente per openbox, bensì per molti wm, tra cui fluxbox,pekwm,openbox ed altri e si chiama: <a href="http://menumaker.sourceforge.net/" target="_blank"><strong>MenuMaker</strong></a>

**NB**: anche in questo, come il precedente caso; vai di configure,make,etc.. In arch:

<pre>~# pacman -S menumaker</pre>

Installato procederemo alla creazione di un menu completo con il comando:

<pre>~$ mmaker -vf OpenBox3</pre>

**NB**: l'opzione -f forza menumaker a sovrascrivere un aventuale menu esistente nella directory di riferimento openbox. E' bene creare una copia di backup "menu.xml_backup" prima di procedere con la creazione del nuovo menu.

**Note relative:** OpenBox non prevede l'utilizzo e quindi la visualizzazione di icone nel menu. Ci sono tanti altri tips relativi alla customizzazione del menu di openbox che saranno trattati in altri post.

**PANNELLI:**

Come ****precedentemente descritto, **OpenBox** non prevede l'avvio di un pannello nativo. Esistono tanti pannelli e la scelta del quale usare ricade solo sul gusto personale oltre magari alle risorse disponibili della macchina che si sta usando. Per capire meglio ed eventualmente scegliere, vi rimando al link dell'apposito post riguardo <a href="http://linuax.altervista.org/blog/2009/12/24/wm-e-pannelli-non-nativi-tint2/" target="_blank"><strong>i pannelli non nativi in generale e tint2</strong></a> (uno dei migliori a mio modesto parere).

**Considerazioni:**

Così come **Fluxbox**, **Openbox** si può considerare un cugino che combatte per lo stesso fine. Anche se l'approccio è diverso, entrambi sono **wm** ampiamente configurabili e stabili. Offrono leggerezza, snellicità e ampia customizzazione. Entrambi se pur con alcune limitazioni "molto piccole e che verrano spiegate più avanti" (per alcuni utenti _linuxdipendenti_ fastidiose, me compreso) sono ciò che più si avvicina al mio concetto di desktop oltre che dell'usabilità chiaramente inclusa nel discorso. La perfezione sicuramente non esiste. Ma devo dire che in questo ambito ciò che più ci si avvicina è un altro cugino della stessa discendenza: **Pekwm**; se ne parlerà più avanti. In tutto questo, non si capisce _"non mi capisco...sarò un conservatore"_ come mai gira e rigira cambiano i wm ma l'aspetto fondamentalmente è sempre lo stesso :P.

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2009/10/ax_openbox.jpg"><img class="alignnone size-full wp-image-1538" title="ax_openbox" src="http://linuax.altervista.org/blog/wp-content/uploads/2009/10/ax_openbox.jpg" alt="" width="259" height="194" /></a></pre>

Al prossimo post!

\# End