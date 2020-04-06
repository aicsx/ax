---
id: 178
title: 'Configurare fluxbox - menu, icone ed emulatore di terminale'
date: 2009-01-05T00:08:52+01:00
author: ax
excerpt: |
  <p>How-to sulle configurazioni principali di Fluxbox - icone dekstop,terminali,menu.</p>
layout: post
guid: http://linuax.wordpress.com/?p=178
permalink: /2009/01/05/configurare-fluxbox-menuicone-e-terminale/
image_url:
  - http://linuax.wordpress.com/files/2009/01/screenshot_ax_fluxbox.png
  - http://linuax.wordpress.com/files/2009/01/screenshot_ax_fluxbox.png
  - http://linuax.wordpress.com/files/2009/01/screenshot_ax_fluxbox.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/01/screenshot_ax_fluxbox.png" class="alignnone size-thumbnail wp-image-925" title="screenshot_ax_fluxbox"   alt=""    />'
  - '<img src="http://linuax.wordpress.com/files/2009/01/screenshot_ax_fluxbox.png" class="alignnone size-thumbnail wp-image-925" title="screenshot_ax_fluxbox"   alt=""    />'
  - '<img src="http://linuax.wordpress.com/files/2009/01/screenshot_ax_fluxbox.png" class="alignnone size-thumbnail wp-image-925" title="screenshot_ax_fluxbox"   alt=""    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:4:"1152";i:1;s:3:"864";i:2;s:1:"3";i:3;s:25:"width="1152" height="864"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:4:"1152";i:1;s:3:"864";i:2;s:1:"3";i:3;s:25:"width="1152" height="864"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:4:"1152";i:1;s:3:"864";i:2;s:1:"3";i:3;s:25:"width="1152" height="864"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
categories:
  - Fluxbox
  - How-to
  - Linux
  - menu
  - terminal
  - Wm
tags:
  - fluxbox
  - How-to
  - Linux
  - menu
  - programmi
  - shell
  - slackware
  - Tips
  - wm
  - X
---
Nel post precedente che riguarda la compilazione di Fluxbox si è trattata la compilazione di un ambiente minimalistico che supportasse ogni tipo di personalizzazione e di risposta alla richiesta di usabilità in leggerezza.

Per l'appunto in questo post vedremo in specifico come usare e configurare alcuni applicativi che rendono l'interfaccia di questo wm più semplice e veloce per semplici operazioni quali icone, sfondo, pager etc.

#

Avrete notato che di default il menù di fluxbox appare nettamente scarno e non implementa molte delle applicazioni installate nella nostra linuxbox anche se le stesse fanno parte di altri wm. Per ovviare a questa spiacevole situazione basta usare uno strumento implementato dalla release 0.9.x di default ossia fluxbox-generate_menus.

NOTA: fluxbox-generate_menu -h  ci darà un idea chiara delle opzioni.

<pre>~$ fluxbox-generate_menu -kgB -t urxvt -w linuax.wordpress.com -b firefox</pre>

NOTA: nel caso che la generazion automatica del menù non soddisfi le vostre richieste e/o non sia esattamente ciò che volete vedere nel vostro menù esiste la seconda opzione ossia: modificare il menù a mano.

**\*LA DIRECTORY ./FLUXBOX \***

Nella directory **~/.fluxbox/** sono presenti vari file di configurazione, che permettono un più alto livello di configurazione e personalizzazione; di seguito saranno elencati i più importanti, con alcune istruzioni sulla loro modifica.

**-MENU**

Questo può esser considerato il file più rilevante, in quanto permette di modificare a piacere il menù di Fluxbox.  
Una volta aperto il file, ci si rende conto di come sono strutturate le voci, e conseguentemente, come aggiungerne o togliere altre.  
La sintassi di base per aggiungere una voce nel menù è questa:**  
** 

<pre><strong>[exec] (etichetta_programma) {percorso_eseguibile_programma} </strong>
<strong> </strong></pre>

come esempio (per capire meglio, se ce ne fosse bisogno):**  
** 

<pre><strong>[exec] (xmms) {xmms} </strong>
<strong></strong></pre>

E' da ricordare che **(etichetta_programma)** sarà il nome della voce che verrà inserita nel menù.

**NB**: in **{percorso\_eseguibile\_programma}** spesso, è necessario indicare soltanto il comando, come se lo si stesse dando da linea di comando, cioè non è necessario specificare tutto il percorso (in quanto quasi tutti gli eseguibili dei programmi sono in /usr/bin, directory che appartiene automaticamente al proprio PATH).  
Se si vogliono creare sottomenù (cioè voci che si diramano dal menù principale) basta conoscere questa sintassi:**  
** 

<pre><strong>[submenu] (etichetta_sottomenu) </strong>
<strong> [exec] (.......) {.......}</strong>
<strong> [exec] (.......) {.......}</strong>
<strong>[end]</strong>
<strong></strong></pre>

**NB**: è importante ricordarsi che quando si apre un sottomenù, bisogna necessariamente poi chiuderlo con **[end]** .  
Inoltre le istruzioni

<pre><strong>[separator]</strong>
<strong></strong></pre>

o**  
** 

<pre><strong>[nop] (-----)</strong>
<strong></strong></pre>

permettono di separare i vari sottomenù (o le singole voci) tra di loro, mediante linea separatrice o con simboli (in questo esempio i trattini) racchiusi tra le parentesi tonde.

**AGGIUNGERE ICONE AL MENU**

Per poter aggiungere icone nel menu principale basta dire al wm il percorso dell'icona racchiusa nei tag code **<>**

Un esempio può essere l'aggiunta dell'icona ad xmms che da come era precedentemente settato diventa:

<pre><strong></strong><strong>[exec] (xmms) {xmms} </strong>&lt;/home/utente/percorso_icona/icona_scelta.png&gt;</pre>

&nbsp;

**-STARTUP**

**~/.fluxbox/startup  
** 

In questo file è possibile gestire tutte le funzioni post apertura del window manager.  
Si può dunque settare (come spiegato prima) il proprio sfondo desktop preferito, in modo da vederselo caricare automaticamente ad ogni avvio, oppure specificare le applicazioni da avviare automaticamente assieme a Fluxbox, ricordandosi però di mettere il simbolo "**&**" (che indica l' esecuzione in background) alla fine del nome dell' applicazione.

**NB**: per lanciare applicazioni automaticamente all' avvio, si può, in alternativa, inserirle nel file **~/.fluxbox/apps** , seguendo questa sintassi:

<pre><strong>[startup] {nome_applicazione} </strong><strong> </strong></pre>

**-KEYS**

Il file **~/.fluxbox/keys** consente di definire i "keybindings", ovvero associare ad un tasto un' azione specifica  
La sintassi è questa:**  
** 

<pre><strong>tasto_funzione tasto_associato :azione_corrispondente</strong>
<strong></strong></pre>

Spiegazione:  
**tasto_funzione**: può essere **Mod1** (equivalente al tasto ALT della tastiera), **Mod4** (equivalente ad uno dei tasti extra presenti nelle tastiere a 104 tasti, di solito è il famigerato tasto "windows" presente su molte tastiere), Control **(equivalente al tasto CTRL) oppure Shift (il tasto SHIFT della tastiera).**

**tasto_associato: tasto da associare assieme al primo;** 

può essere uno qualsiasi dei tasti presenti sulla tastiera, ad eccezione di quelli funzione (ad esempio a, b, F1, Tab, ...)****

****azione_corrispondente: azione da associare alla combinazione di tasti (per un elenco completo delle azioni possibili consultare la pagina man di fluxbox);

tra le più utili:

ShowDesktop: riduce ad icona tutte le finestre  
ExecCommand rxvt: apre un terminale  
ToggleDecor: nasconde le decorazioni delle finestre  
KillWindow: naturalmente, forza la chiusura delle finestre  
Quit  
Restart

Passiamo adesso alla fase interna dei file e degli style's di fluxbox:

**\*CONFIGURAZIONE FLUXBOX E DESKTOP\***

Innanzitutto consiglio di copiare i temi di Fluxbox dentro la cartella home dell'utente in modo che si possono tranquillamente personalizzare senza intaccare i temi agli altri utenti.

<div class="xoopsCode">
  <pre>~$ cp /usr/share/fluxbox/styles/* ~/.fluxbox/styles/*</pre>
  
  <p>
    Per settare uno sfondo per il desktop basta usare il programma <em>fbsetbg</em> con la seguente sintassi:
  </p>
  
  <div class="xoopsCode">
    <pre>$ fbsetbg -f percorsoimmagine</pre>
    
    <p>
      <strong>NB:</strong> come per quasi tutti gli applicativi l'uso dell'opzione -h è consigliata per visionare la man page.
    </p>
    
    <p>
      Per permettere il caricamento dello sfondo all'avvio di fluxbox bisogna inserire il precedente comando nel file <em>~/.fluxbox/startup</em>, nella seguente riga:
    </p>
    
    <div class="xoopsCode">
      <pre># You can set your favourite wallpaper here if you don't want
# to do it from your style.
#
# bsetbg -f ~/pictures/wallpaper.png
#
# This sets a black background

#/usr/X11R6/bin/bsetroot -solid black

fbsetbg -f percorsoimmagine</pre>
      
      <p>
        <strong>NB:</strong> dalla release 0.9.x per avere effettivi cambiamenti sul cambio dell'immagine del desktop bisogna modificare il file nella cartella $HOME/.fluxbox/init e modificare se già c'è oppure aggiungere la stringa:
      </p>
      
      <pre>session.screen0.rootCommand:    fbsetbg -C ~/percorso_immagini/immagine_scelta.png</pre>
    </div>
  </div>
  
  <p>
    Inoltre per evitare che lo sfondo venga sovrascritto dal tema che usiamo dobbiamo fare una modifica al file del tema, ecco un esempio per il tema shade, per gli altri temi il procedimento sarà molto simile, verso la fine del file troviamo:
  </p>
  
  <div class="xoopsCode">
    <pre>rootCommand:                    bsetroot -solid rgb:4/4/4</pre>
  </div>
  
  <p>
    Bisogna commentare (mettendo un # davanti) l'ultima riga (rootCommand), in questo modo il <em>fbsetbg</em> che viene dato allo startup o nell'init di fluxbox non verrà sovrascritto dal tema che vogliamo usare.
  </p>
</div>

**\*ICONE DESKTOP CON IDESK\*  
** 

Fluxbox non ha un gestore per le icone del desktop però possiamo avere lo stesso risultato utilizzando il programma _idesk:_

<a href="http://idesk.sourceforge.net/" target="_blank">http://idesk.sourceforge.net/</a>

Per poter funzionare idesk richiede il file _~/.ideskrc_ che contiene i settaggi generali del programma e la cartella _~/.idesktop_ dove metteremo i link a tutti i programmi che vogliamo sul desktop. Tutti settaggi che potete mettere sul file ~/.deskrc li trovate qui (<a href="http://idesk.sourceforge.net/wiki/index.php/Idesk-usage" target="_blank">LINK</a>) comunque vi posto un file di esempio che poi potete personalizzare secondo le vostre preferenze:

<div class="xoopsCode">
  <pre>table Config
  FontName: tahoma
  FontSize: 8
  FontColor: #000000
  Locked: false
  Transparency: 150
  Shadow: true
  ShadowColor: #efefef
  ShadowX: 1
  ShadowY: 2
  Bold: true
  ClickDelay: 300
  IconSnap: true
  SnapWidth: 64
  SnapHeight: 100
  SnapOrigin: BottomRight
  SnapShadow: true
  SnapShadowTrans: 200
  CaptionOnHover: false
end

table Actions
  Lock: control right doubleClk
  Reload: middle doubleClk
  Drag: left hold
  EndDrag: left singleClk
  Execute[0]: left doubleClk
  Execute[1]: right doubleClk
end</pre>
</div>

Dopo aver creato il file ~/.ideskrc bisogna creare la cartella ~/.idesktop dove inserire tutti i programmi che vogliamo creandogli un file apposito in questo modo:

<div class="xoopsCode">
  <pre>table Icon
  Caption: nome del programma che viene visualizzato nel desktop
  Icon: percorsoicona
  X: 1088
  Y: 106
  Command[0]: comando da eseguire cliccando due volte con il tasto sinistro
  Command[1]: comando da eseguire cliccando due volte con il tasto destro
end</pre>
</div>

Un file di questo tipo deve essere inserito nella cartella ~/.idesktop con estensione _*.lnk_, inoltre in questa cartella meglio non mettere file di altro genere altrimenti idesk non partirà. Attenzione che idesk è molto esigente, basta un piccolo errore in un file di configurazione e il programma non parte più.

Adesso per permettere a idesk di avviarsi con l'avvio di fluxbox dobbiamo inserire il comando nel file _~/.fluxbox/startup_, precisamente in questa sezione:.

<div class="xoopsCode">
  <pre># Applications you want to run with fluxbox.
# MAKE SURE THAT APPS THAT KEEP RUNNING HAVE AN ''&'' AT THE END.
#
# unclutter -idle 2 &
# wmnd &
# wmsmixer -w &
# idesk &

idesk &</pre>
</div>

In questa sezione si definiscono i programmi che devono partire in automatico con fluxbox, in questo caso mettiamo idesk &.

**\*CONFIGURAZIONE EMULATORI DI TEMINALI\***

Adesso passiamo alla personalizzazione del terminale, da usare in accoppiata a fluxbox ve ne sono molti: aterm, eterm, xterm, rxvt, etc.. Metto qua alcuni parametri per la personalizzazione di _aterm_.

<span style="text-decoration: underline;">-bg colore</span>: questo stabilisce il colore di sfondo (ovviamente sostituite il colore che volete al posto di ?colore?)

<span style="text-decoration: underline;">-fg colore</span>: questo invece stabilisce il colore del testo del terminale

<span style="text-decoration: underline;">-tr</span>: questo parametro rende la finestra di terminale trasparente

<span style="text-decoration: underline;">-sh valore</span>: questo paramentro può essere usato in combinazione con il precedente in modo da stabilire il livello di trasparenza, al posto di ?valore? va messo un numero compreso tra 0 e 100 (100 è l'immagine originale)

<span style="text-decoration: underline;">+sb</span>: toglie la barra di scorrimento

Se volete altri parametri più personalizzati vi rimando alla pagina di manuale man aterm.

**NB:** per utilizzare urxvt magari trasparente sul desk vi rimando al post precedente nella sezione Tips: Personalizzare emulatore di terminale.

<pre><strong>*TIPS PER CANCELLARE BORDI FINISTRA IN EMULATORI DI TERMINALI*</strong></pre>

Come detto precedentemente il file **~./fluxbox/keys** racchiude le combinazioni di tasti e le funzioni da eseguire.A tal proposito vi posto la mia configurazione per l'eliminazione dei bordi dalle finestre.

Modificare **~./fluxbox/keys** e inserire la stringa:

<pre>Mod4 W :ToggleDecor</pre>

Equivale alla combinazione di tastiere moderne che supportano oramai la vecchia care icona di "windows" al centro dei tasti Ctrl e Alt in basso a sinistra, tanto per intenderci + il tasto W.

Riassumendo: "TASTO WINDOWS + W" = nascondi bordi finestra

**NB:** Fluxbox supporta la funzione ricorda posizione e grandezza raggiungibile col tasto destro del mouse sezione >REMENBER che può tornare utile se si apre una shell trasparente e la si vuole piazzare sul desktop in maniera permanete magari senza bordo finestra. Tenete a mente questa funzione :P

<span style="color: #b61f0b;"><strong>Conclusioni:</strong></span>

Fluxbox è un ambiente ampiamente configurabile e personalizzabile che supporta una notevole mole di opzioni il cui scopo è rendere il desk esteticamente quanto più  bello possibile e per quanto riguarda  l'usabilità non tralascia alcun aspetto ià presente in tutti gli altri wm. Il tutto con estrema snellicità e il più basso consumo possibile delle risorse della nostra linuxbox. Tanto per essere consapevoli delle sue potenzialità vi linko un paio di screen così da porvi delle domande a riguardo:

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2009/01/ax_fluxbox1.jpg"><img class="alignnone size-full wp-image-1551" title="ax_fluxbox1" src="http://linuax.altervista.org/blog/wp-content/uploads/2009/01/ax_fluxbox1.jpg" alt="" width="1024" height="768" srcset="https://linuax.altervista.org/blog/wp-content/uploads/2009/01/ax_fluxbox1.jpg 1024w, https://linuax.altervista.org/blog/wp-content/uploads/2009/01/ax_fluxbox1-300x225.jpg 300w" sizes="(max-width: 1024px) 100vw, 1024px" /></a></pre>

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2009/01/screenshot_ax_fluxbox.png"><img class="alignnone size-full wp-image-1552" title="screenshot_ax_fluxbox" src="http://linuax.altervista.org/blog/wp-content/uploads/2009/01/screenshot_ax_fluxbox.png" alt="" width="1152" height="864" srcset="https://linuax.altervista.org/blog/wp-content/uploads/2009/01/screenshot_ax_fluxbox.png 1152w, https://linuax.altervista.org/blog/wp-content/uploads/2009/01/screenshot_ax_fluxbox-300x225.png 300w, https://linuax.altervista.org/blog/wp-content/uploads/2009/01/screenshot_ax_fluxbox-1024x768.png 1024w" sizes="(max-width: 1152px) 100vw, 1152px" /></a></pre>

Per altre **INFO** su fluxbox vedi**: <a href="http://linuax.altervista.org/blog/2009/01/05/tips-fluxbox/" target="_blank">TIPS Fluxbox</a>**

\# End