---
id: 890
title: 'Applicazioni/desktop/bindkeys: OpenBox Tips'
date: 2009-10-27T09:00:52+01:00
author: ax
excerpt: Breve how-to delle principali possibilità di customizzazione desktop/sfondo/bindings/programmi che sono implementate in OpenBox.
layout: post
guid: http://linuax.wordpress.com/?p=890
permalink: /2009/10/27/applicazionidesktopbindkeys-openbox-tips/
image_url:
  - http://linuax.wordpress.com/files/2009/10/urxvt_openbox.png
  - http://linuax.wordpress.com/files/2009/10/urxvt_openbox.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/10/urxvt_openbox.png" class="alignnone size-full wp-image-891" title="urxvt_openbox"   alt="urxvt_openbox"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/10/urxvt_openbox.png" class="alignnone size-full wp-image-891" title="urxvt_openbox"   alt="urxvt_openbox"    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"500";i:2;s:1:"3";i:3;s:24:"width="800" height="500"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"500";i:2;s:1:"3";i:3;s:24:"width="800" height="500"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
categories:
  - Arch
  - How-to
  - Linux
  - Openbox
  - Slackware
  - Tips
tags:
  - bindkeys
  - desktop
  - Linux
  - openbox
  - pekwm
  - Tips
---
Come accennato nel <a href="http://linuax.wordpress.com/2009/10/25/box-wm-ultraleggero-openbox/" target="_blank">post precedente</a> relativo all'utilizzo generale di questo **wm**, la customizzazione di **OpenBox** è sostanzialmente racchiusa in 3 file.

In specifico, il file **rc.xml** che risiede nella directory di riferimento **~/.config/openbox** racchiude le configurazioni generali e altre interessanti possibilità di customizzazione. Vediamo brevemente come sfruttarle.

Cominciamo dicendo che chi utilizza da un pò di tempo i **wm**:  **fluxbox,openbox,pekwm**...ed altri, quasi sempre ha la necessità di assegnare funzioni e/o applicativi ad una certa combinazione di tasti. **Openbox** gestisce il bind **keys** nel file generale **rc.xml** e le direttive relative ad una combinazione di tasti sono racchiuse dai tag:

<pre>&lt;!-- riferito all'apertura del bindkey --&gt;
&lt;keybind&gt;

&lt;!-- riferito alla chiusura del bindkey --&gt;
&lt;/keybind&gt;</pre>

**NB**: I tag relativi al bind sono gestiti con regole di codice inserite all'interno dalla sezione rappresentata dai tag:

<pre>&lt;keyboard&gt;

&lt;/keyboard&gt;</pre>

Vediamo un esempio pratico di ciò che il file **rc.xml** già supporta, aprendo il file con un editor di testo:

<pre>~$ vi ~/.config/openbox/rc.xml
&lt;keybind key="A-F1"&gt;
 &lt;action name="DesktopLeft"&gt;
 &lt;dialog&gt;no&lt;/dialog&gt;
 &lt;wrap&gt;no&lt;/wrap&gt;
 &lt;/action&gt;
 &lt;/keybind&gt;

&lt;keybind key="A-F2"&gt;
 &lt;action name="DesktopRight"&gt;
 &lt;dialog&gt;no&lt;/dialog&gt;
 &lt;wrap&gt;no&lt;/wrap&gt;
 &lt;/action&gt;
 &lt;/keybind&gt;</pre>

In specifico i tag si riferiscono a:

<pre>&lt;keybind key="A-F1"&gt;         # Tipo di combinazione di tasti
 &lt;action name="DesktopLeft"&gt; # Tipo di azione
 &lt;dialog&gt;no&lt;/dialog&gt;         # Tipo di notifica e/o finestra di dialogo
 &lt;/action&gt;                   # Chiusura dell'azione</pre>

Nell'esempio quindi, la combinazione di tasti creata e/o preformattata dal wm **A+F1** e **A+F2** , dove per A si intende il tasto "**ALT**" ci permette mediante pressione singola di spostare l'aria desktop da utilizzare verso destra o verso sinistra muovendoci all'interno dei nostri desktop virtuali.

**NB**: Allo stesso modo sono gestite le regole bindings del mouse se pur con molte proprietà aggiuntive  per caratteristiche differenti.

**Esempio di customizzazione applicativo + binding:**

Supponiamo di voler fare in modo che la nostra shell emulata attraverso l'applicativo che preferiamo "xterm,gnome-terminal,urxvt,aterm,eterm,mrxvt,etc.." debba restare a schermo in modalità sticky (attaccata come se fosse incorporata nel desktop), magari senza i bordi finestra nel caso sia anche trasparente.  Per ottenere questo risultato è evidente che abbiamo bisogno di due cose:

  1. Una regola che binda una combinazione di tasti utile a massimizzare la finestra in caso di bisogno, visto che non avrà più i bordi della finestra che ce lo permetterebbero.
  2. Una specifica regola che dica a openbox che quel terminale deve essere aperto in quel particolare punto e che non deve avere bordi finestra etc...

Per il punto uno quindi aggiungiamo un binding che prevede ad esempio la combinazione **Mod4+m**.

**NB**: Capendoci meglio per **Mod4** si intende il tasto della tastiera rappresentante il_ **"simbolo di windows"**_ (una volta tanto questo benedetto windows ci risulta utile hahaha :P)

Regola da aggiungere nell'apposita sezione **<keyboard>** come da esempio:

<pre>&lt;keybind key="W-m"&gt;
 &lt;action name="ToggleMaximizeFull"/&gt;
 &lt;/keybind&gt;</pre>

<span style="color:#ff0000;"><strong>NB</strong></span>: E' buona norma controllare sempre se l'azione non è già presente nel file rc.xml e in tal caso se contiene un altro tipo di keybind. Di default OpenBox implementa già molti bindings.

Per definire l'applicazione, la posizione sul desktop e le preferenze della stessa invece si utilizza l'apposita sezione racchiusa nei tag:

<pre>&lt;applications&gt;

&lt;/applications&gt;</pre>

**NB**: E' posta solitamente in prossimità della fine del file rc.xml.

Aggiungiamo nei tag appositi la nostra applicazione in modo che risulti all'interno dei tag come da esempio:

<pre>&lt;applications&gt;</pre>

<application name="urxvt">  
<decor>no</decor>  
<focus>yes</focus>  
<position>  
<x>300</x>  
<y>0</y>  
</position>  
<layer>normal</layer>  
<maximized>false</maximized>

<pre>&lt;/applications&gt;</pre>

**NB**: verranno definite quindi: l'applicazione, il tipo di decorazione e preferenze di stile e focus, la posizione sullo schermo e le altre caratteristiche di posizionamento della finestra. In identico modo potevano essere aggiunte e/o modificate le caratteristiche come da esempio:

<pre>&lt;applications&gt;</pre>

<application name="urxvt">  
<decor>no</decor>  
<focus>yes</focus>  
<position>  
<x>300</x>  
<y>0</y>  
</position>

<div class="codecolorer-container text dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="text codecolorer">
    <layer>below</layer><br /> <desktop>all</desktop>
  </div>
</div>

<maximized>false</maximized>

<pre>&lt;/applications&gt;</pre>

**NB**: In questo caso il nostro emulatore di terminale verrà considerato parte integrante del desktop e di tutti i desktop virtuali presenti. Una sorta di modalità "sticky" per intenderci.

Screen del risultato:

<pre><img class="alignnone size-full wp-image-891" title="urxvt_openbox" src="http://linuax.files.wordpress.com/2009/10/urxvt_openbox.png" alt="urxvt_openbox" width="455" height="284" />
</pre>

**NB**:  In questo caso il binding sarà relativo al terminale in uso che potrà essere massimizzato con l'apposito binding da noi precedentemente creato: **Mod4+m**.

Capite bene che ci sono ampie possibilità di personalizzazione e customizzazione delle varie applicazioni oppure delle singole funzione assegnate ai tasti, al mouse o direttamente come integrazie desktop. Non resta che sperimentare.

Per una guida dettagliata e approfondita vi rimando alla consultazione della documentazione ufficiale relativa al <a href="http://icculus.org/openbox/index.php/Help:Bindings" target="_blank">bindings</a>.

**Desktop: settare lo sfondo su OpenBox  
** 

I modi per settare lo sfondo su OpenBox sono vari e tutti validi. Per questo mi limito a descrivere brevemente  quello che uso io. L'applicativo che fa da tramite allo scopo si chiama <a href="http://freshmeat.net/projects/feh/" target="_blank">feh</a>.

Installato senza particolari problematiche basta lanciare:

<pre>~$ feh --bg-center /percorso/immagine/file_esempio.estensione</pre>

**NB**: Per stabilire la risoluzione dell'immagine e/o magari l'affiancamento ci sono le opzioni:

<pre>--bg-tile
--bg-center
--bg-seamless
</pre>

Settato lo sfondo scalato e/o affiancato e/o centrato verrà creato un file nella nostra home **~/.fehbg** che contiene il comando da richiamare all'avvio di **OpenBox**.

Aggiungiamo con il nostro editor preferito al file **~/.config/openbox/autostart.sh** la stringa come da esempio:

<pre>~$ vi ~/.config/openbox/autostart.sh
eval `cat ~/.fehbg` &</pre>

**NB**: In questo modo ad ogni avvio avremo il nostro sfondo configurato. In alternativa si può aggiungere il richiamo del comando **feh** direttamente al file **~/.xinitrc** .

\# End