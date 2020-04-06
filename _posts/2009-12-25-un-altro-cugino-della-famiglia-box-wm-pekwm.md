---
id: 932
title: 'Un altro cugino della famiglia *box wm: PekWM'
date: 2009-12-25T07:00:37+01:00
author: ax
excerpt: |
  <p>Guida generale all'uso e alla customizzazione di un altro wm discendente della famiglia *box: PekWM.</p>
layout: post
guid: http://linuax.wordpress.com/?p=932
permalink: /2009/12/25/un-altro-cugino-della-famiglia-box-wm-pekwm/
image_url:
  - http://linuax.wordpress.com/files/2009/12/pekwm_ax.png
  - http://linuax.wordpress.com/files/2009/12/pekwm_ax.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/12/pekwm_ax.png" class="alignnone size-thumbnail wp-image-948" title="pekwm_ax"   alt=""    />'
  - '<img src="http://linuax.wordpress.com/files/2009/12/pekwm_ax.png" class="alignnone size-thumbnail wp-image-948" title="pekwm_ax"   alt=""    />'
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
  - PekWM
  - Slackware
  - Wm
tags:
  - How-to
  - Linux
  - pekwm
  - slackware
  - wm
---
Dopo un breve periodo di inattività, ecco un altro post che mira a chiarire (per quanto si può) le idee rispetto al meraviglioso mondo dei **wm** alternativi. Il più conosciuto resta ovviamente **Fluxbox**, a cui segue immediatamente **OpenBox** per sviluppo e adesione di utenza. Questa volta si cercherà di dare uno sguardo più mirato a <a href="http://pekwm.org/" target="_blank"><strong>PekWM</strong></a>, così come richiesto in primis da **mauri** (che approfitto per salutare). **<a href="http://pekwm.org/" target="_blank">PekWM</a>** è l'ultimo arrivato per conoscenza da parte dell'utenza generale ma che per molti (compreso me) risulta essere il giusto compromesso di usabilità, personalizzazione e consumo di risorse su pc _datati_.

Tanto per cominciare bisogna capire di cosa si parla. **PekWM** è un progetto basato sul vecchio **<a href="http://freshmeat.net/projects/aewm_plus_plus/" target="_blank">aewm++</a>** scritto in **C++** che a sua volta risultava/risulta essere un fork del predecessore **<a href="http://www.red-bean.com/decklin/aewm/" target="_blank">aewm</a>** che è scritto in **C**. Quando parlo di _vecchio_ chiaramente mi riferisco al fatto che le ultime release di questi wm datano anno 2006/2007 e per una serie di ragioni si sono fermate per fare spazio a progetti _meglio_ inquadrati nel rapporto qualità,usabilità,prestazioni. Le qualità principali di PekWM che risultano essere molto distaccate da quelle dei suoi predecessori sono l'ampia configurazione e la ripresa di alcune specifiche già conosciute ed apprezzate in **fluxbox** e **openbox**. Fra queste ci sono: personalizzazione programmi e funzionalità attraverso la tastiera "keybindings" , proprietà singole delle applicazioni "apparenza e scorrimento", personalizzazione e focus delle finistre, utilizzo di xinerama per la virtualizzazione multipla dei desktop sotto X server, personalizzazione del menù e tante , tante,  altre.

L'installazione non ha nulla di particolare rispetto alla compilazione classica. Al momento attuale della scrittura del post l'ultima release disponibile risulta essere la 0.1.11. Quindi:

<pre>~$ wget <a href="http://pekwm.org/projects/pekwm/files/pekwm-0.1.11.tar.bz2">pekwm-0.1.11.tar.bz2</a>
~$ tar jxvf pekwm-0.1.11.tar.bz2
~$ cd pekwm-0.1.11/
~$ ./configure</pre>

**NB**: non è necessario abilitare le opzioni di compilazione riguardo le features manualmente perchè sono tutte (quelle principali) abilitate di default. Se fosse necessario consultare **_./configure -help_**

<pre>~$ make
~$ su -
password:
~# make install</pre>

Per chi usa, Archlinux è disponibile come sempre, il precompilato:

<pre>~# pacman -Sy pekwm</pre>

Una volta installato, una modifica veloce allo xinitrc, per dire al sistema cosa avviare. Su Slackware avviamo **xwmconfig** oppure in generale sostituiamo e/o aggiungiamo manualmente la stringa:

<pre>~$ vi ~/.xinitrc
exec pekwm</pre>

Salvate le modifiche di avvio del server X per il file **~/.xinitrc** , seguirà ovviamente il classico:

<pre>~$ startx</pre>

**BASE:**

**C**on il primo avvio si autogenerano nella directory di riferimento i file di configurazione compreso il menu di base con le applicazioni installate sulla macchina. Così come accadeva per **OpenBox**, **PekWM** non include nessun tipo di task/tray bar di default. Per questo è consigliato l'utilizzo di pannelli esterni (vedi link del post riguardo <a href="http://linuax.wordpress.com/2009/12/24/wm-e-pannelli-non-nativi-tint2/" target="_blank"><strong>i pannelli non nativi in generale e l'utilizzo di tint2</strong></a>).

**NB**: la directory di riferimento di PekWM contenente tutti i file di configurazione, gli scripts, i temi ed eventualmente le icone è: **~/.pekwm** . Diamo uno sguardo specifico a tutti i vari files di conf.

**CONFIG:**

E' il file generale che racchiude la configurazione generale di **PekWM**. E' suddiviso in zone, che vanno più o meno modificate in base all'importanza delle stesse. Vediamo in dettaglio le zone commentando (#) i settaggi particolarmente importanti:

<pre><span style="color: #993300;"># Questa prima parte definisce i percorsi dei vari file generali di configurazione.</span>
Files {
 Keys = "~/.pekwm/keys"
 Mouse = "~/.pekwm/mouse"
 Menu = "~/.pekwm/menu"
 Start = "~/.pekwm/start"
 AutoProps = "~/.pekwm/autoproperties"
 Theme = "~/.pekwm/themes/Inda-B"
 Icons = "~/.pekwm/icons/"
}
<span style="color: #993300;"># Fine sezione files</span>

<span style="color: #993300;"># Sezione MoveResize</span>
MoveResize {
 EdgeAttract = "10"
# Definisce la distanza dal bordo dello schermo per le finestre
 EdgeResist = "10"
# Definisce la distanza dal bordo dello schermo per le altre finestre che si startano in "resistenza"
 WindowAttract = "5"
# Definisce la distanza di aggancio (due finestre poste di fianco) tra una finestra e l'altra.
 WindowResist = "5"
# Definisce la distanza di aggancio per le altre finestre startate in "resistenza".
 OpaqueMove = "True"
# Settato su true applica il movimento opaco
 OpaqueResize = "False"
# Settato su true applica il resize della finestra opaco
}
<span style="color: #993300;"># Fine sezione MoveResize</span>

<span style="color: #993300;"># Inizio sezione Screen</span>
Screen {
 Workspaces = "2"
# Numero di desktop virtuali
 WorkspacesPerRow = "2"
# Numero aree di lavoro per ogni riga. Se &lt;1 adatta tutte le aree di lavoro su un unica riga.
 WorkspaceNames = "1;2"
# Nome delle aree desktop virtuali
 ShowFrameList = "True"
# Mostra lo stato del frame
 ShowStatusWindow = "True"
# Mostra lo stato dello spostamento o ridimensionamento finestre
 ShowStatusWindowCenteredOnRoot = "False"
# Come sopra pastando a centro finestra
 ShowClientID = "False"
# Mostra l'id client nella finestra titolo
 ShowWorkspaceIndicator = "500"
# Mostra il cambio del desktop virtuale 1;2 per N millisecondi. Se impostato &lt;1 è disattivato.
 FocusNew = "True"
 PlaceNew = "True"
# Definisce il focus delle nuove/transitorie finestre 

 TrimTitle = "..."
# Definisce il testo per tagliare i titoli troppo lunghi
 FullscreenAbove = "True"
 FullscreenDetect = "True"
# Definisce il riempimento e attiva il rilevamento per le finestre che passano dalla modalità normale a full e viceversa.
 HonourRandr = "True"
# Forza la lettura di XRANDR. Può essere disattivata se si utilizza un driver di visualizzazione che utilizza sia xinerama e randr e solo uno dei due risulto corretto.
 EdgeSize = "1 1 1 1"
# Definisce i bordi dello schermo. Rispettivamente: su - giu - sinistra - destra.
 EdgeIndent = "False"
# Definisce se bisogna riservare spazio ai bordi dello schermo.
 PixmapCacheSize = "20"
 DoubleClickTime = "250"
# Intervallo in millisecondi che definisce il doppio click  

 Placement {
 Model = "CenteredOnParent Smart MouseCentered"
 Smart {
 Row = "False"
 TopToBottom = "True"
 LeftToRight = "True"
 OffsetX = "0"
 OffsetY = "0"
   }
 }
 UniqueNames {
 SetUnique = "True"
 Pre = " #"
 Post = ""
 }
}
<span style="color: #993300;"># Fine sezione Screen</span> 

<span style="color: #993300;"># Inizio sezione Menu</span>
Menu {
 DisplayIcons = "True"
# Definisce se visualizzare le icone nel menu principale di pekwm

 Icons = "DEFAULT" {
 Minimum = "16x16"
 Maximum = "16x16"
 }
# Definisce la grandezza minima e massima delle stesse

 Select = "Motion"
 Enter = "Motion ButtonPress"
 Exec = "ButtonRelease"
}
<span style="color: #993300;"># Fine sezione Menu</span>

<span style="color: #993300;"># Inizio sezione CmdDialog</span>
CmdDialog {
 HistoryUnique = "True"
 HistorySize = "1024"
 HistoryFile = "~/.pekwm/history"
 HistorySaveInterval = "16"
}
<span style="color: #993300;"># Fine sezione CmdDialog</span>

<span style="color: #993300;"># Inizio sezione Harbour/dockapp</span>
Harbour {
 OnTop = "True"
 MaximizeOver = "False"
 Placement = "Right"
 Orientation = "TopToBottom"
 Head = "0"

 DockApp {
 SideMin = "64"
 SideMax = "0"
 }
}
<span style="color: #993300;"># Fine sezione Harbour/dockapp</span></pre>

**NB**:  si è scelto di commentare solo le voci ritenute essenziali alla prima personalizzazione. Tutte le altre sono facilmente consultabili da <a href="http://www.pekwm.org/files/pekwm/doc/0.1.10/html/index.html" target="_blank">documentazione ufficiale</a>. ****

**VARS**:

Questo file definisce variabili che saranno poi successivamente richiamate dal menu di base. Ad esempio supponiamo di voler lanciare l'emulatore di terminale dal menu di pekwm associandolo ad una serie di opzioni come quelle seguenti:

**$TERM="xterm -fn fixed +sb -bg white -fg black"**

Definendo le opzioni nella variabile creata nell'apposito file di config essa potrà essere richiamata dal menu evitando quindi di riscrivere ogni volta le stesse opzioni per l'applicativo xterm.

**NB**: Non abbiate timore.Per capire meglio questa funzione andiamo al file di config successivo.

**MENU:**

<span style="color: #000000;">E' ovviamente il file in cui sono descritti i parametri riguardo la descrizione e il lancio degli applicativi. Il menu di base viene autogenerato al primo avvio del wm. A <strong>differenza</strong> di fluxbox, <strong>PekWM prevede</strong> di base l'utilizzo di script esterni,così come avviene per </span>OpenBox in cui questa funzione è definita sotto il nome di pipemenu.

**NB**: Avrete già notato dal config di base descritto precedentemente che **PekWM** **prevede** l'utilizzo di icone nel menu. Questa funzione che è conosciuta e data per scontata da tutti gli utenti che utilizzano Fluxbox sarà nuovamente  apprezzata per l'altra fascia di utenza che utilizza OpenBox che invece **non** prevede icone di menu. Vediamo in dettaglio come sono differenziate le voci di menu e le opzioni che lo compongono:

<pre>RootMenu = "Pekwm" {
# DEfinisce il menu del desktop di pekwm chiamato "Pekwm"
    Entry = "Term" { Actions = "Exec $TERM &" }
    # Definisce il primo titolo Term che corrisponde all'azione Exec $TERM. <strong>NB</strong>: la famosa variabile definita nel conf <strong>vars</strong> per l'emulatore di terminale scelto ci ha evitato di scrivere tutte le opzioni per definire il comando.
    Entry = "Emacs" { Actions = "Exec $TERM -title emacs -e emacs -nw &" }
    # Definisce il secondo titolo Emacs che richiama l'applicativo partendo dal terminale prescelto
    Entry = "Vim" { Actions = "Exec $TERM -title vim -e vi &" }
    # Come sopra ... continua con le voci di menu
    Separator {}
    # Definisce una linea di separazione all'interno del menu

    Submenu = "Utils" {
    # Con la voce di menu Submenu si definisce un menu figlio del menu Root che conterrà a sua volta voci di menu
        Entry = "XCalc" { Actions = "Exec xcalc &" }
        Entry = "XMan" { Actions = "Exec xman &" }
    }
    # Chiusura del sottomenu attraverso parentesi "}"

    Separator {}

    Submenu = "Pekwm" {
    # Ancora una volta un sottomenu figlio che racchiude alcune azioni proprietarie di PekWM come la rilettura dei file config,il restart e l'uscita.
        Entry = "Reload" { Actions = "Reload" }
        Entry = "Restart" { Actions = "Restart" }
        Entry = "Exit" { Actions = "Exit" }
    }
}
# Chiusura finale del menu ROOT di PekWM "desktop menu"</pre>

**NB**: Normalmente ci si aspetterebbe la fine del conf, ma non è così, perchè con **PekWM** nello stesso file config di menu è presente anche il **Window Menu** (menu delle finestre) che può essere ampiamente riconfigurato. In questo caso vi incollo il mio che è rispetto a quello di default leggermente più completo:

<pre>WindowMenu = "Window Menu" {
 Entry = "(Un)Stick" { Actions = "Toggle Sticky" }
 Entry = "(Un)Shade" { Actions = "Toggle Shaded" }
 Submenu = "Maximize" {
 Entry = "Full" { Actions = "Toggle Maximized True True" }
 Entry = "Horizontal" { Actions = "Toggle Maximized True False" }
 Entry = "Vertical" { Actions = "Toggle Maximized False True" }
 }
 Submenu = "Fill" {
 Entry = "Full" { Actions = "MaxFill True True" }
 Entry = "Horizontal" { Actions = "MaxFill True False" }
 Entry = "Vertical" { Actions = "MaxFill False True" }
 }
 Submenu = "Stacking" {
 Entry = "Raise " { Actions = "Raise" }
 Entry = "Lower" { Actions = "Lower" }
 Entry = "Always On Top " { Actions = "Toggle AlwaysOnTop" }
 Entry = "Always Below" { Actions = "Toggle AlwaysBelow" }
 }
 Submenu = "Decor" {
 Entry = "Decor" { Actions = "Toggle DecorBorder; Toggle DecorTitlebar" }
 Entry = "Border" { Actions = "Toggle DecorBorder" }
 Entry = "Titlebar" { Actions = "Toggle DecorTitlebar" }
 }
 Submenu = "Skip" {
 Entry = "Menus" { Actions = "Toggle Skip Menus" }
 Entry = "Focus Toggle" { Actions = "Toggle Skip FocusToggle" }
 Entry = "Snap" { Actions = "Toggle Skip Snap" }
 }
 Entry = "Iconify " { Actions = "Set Iconified" }
 Entry = "Manual Action" { Actions = "ShowCmdDialog" }
 Separator {}
 Entry = "Kill " { Actions = "Kill " }
 Entry = "Close" { Actions = "Close" }
}</pre>

**NB**:  talmente intuitivo che non merita commenti. In ogni caso è presente anche per esso un ampia <a href="http://www.pekwm.org/files/pekwm/doc/0.1.10/html/index.html" target="_blank">documentazione ufficiale</a>.

**NB**: si era parlato di icone da inserire nel menu del desktop così come avveniva per fluxbox. Per aggiungere le icone è sufficiente copiarle nell'apposita directory di riferimento definita precedentemente nel **file config** . Se vi ricordate, i settaggi facevano riferimento a: **~/.pekwm/icons/** . Settata la directory si richiama l'icona attraverso il parametro:

<pre>Icon = "icona.png";</pre>

Esempio all'interno di una Entry di menu:

<pre>RootMenu = "pekwm" (
    Entry = "Term" { Actions = "Exec $TERM &" }</pre>

<pre><span style="color: #ff0000;"><strong>DIVENTA:</strong></span>
RootMenu = "pekwm" (
    Entry = "Term" { Icon = "xterm.png"; Actions = "Exec $TERM &" }</pre>

<span style="color: #000000;">Stesso discorso per aggiungere un icona ad un sottomenu:</span>

<pre>Submenu = "Utils" {<span style="color: #ff0000;"><strong> DIVENTA:</strong></span>
Submenu = "Utils" { Icon = "utils.png" ;
     Entry = ... ecc ... ecc ..</pre>

> **APPLICATIVI DI MENU**

Come sempre, esiste una larga fetta di utenza _pigra_, che ritiene troppo faticoso operare manualmente sul menu desktop di **PekWM** per ogni nuovo programma installato. Per ovviare è possibile l'utilizzo di programmi che facciano il lavoro sporco per noi. Nel caso specifico il programma che si occupa di aggiornare il menu automaticamente ricercando i vari programmi attraverso i link presenti in **/usr/share/applications** è <a href="http://menumaker.sourceforge.net/" target="_blank">MenuMaker</a>.

Dopo opportuna installazione, eseguendo i soliti passaggi di compilazione source oppure mediante pacman (su archlinux) sarà sufficiente lanciare il comando:

<pre>~$ mmaker PekWM</pre>

**NB**: menumaker dispone di numerose opzioni utili. Prima di tutto è opportuno lanciare come sempre **"mmaker -help"**.

**AUTOPROPERTIES:** 

Sta per "proprietà automatiche"; si riferisce alle proprietà delle applicazioni ovviamente. Questo file conf si occupa di gestire una miriade di cose. Tra le più importanti ci sono: dimensione, stato di partenza dell'applicazione, raggruppamento, area di lavoro, bordi della finestra e tante altre.

Per capire come utilizzare al meglio questo conf bisogna chiarire un paio di cose. Innanzitutto **PekWM** utilizza per tutti i file di conf  le stesse regole di sintassi fatta eccezione per il file di conf : "**start**" che vedremo in seguito. In specifico la sintassi generale risulta come da esempio:

<pre># commento
// altro commento commento
/*
	ancora un altro commento
*/

$VAR = "Variabile"
$_VARIABLE = "Variabile"
INCLUDE = "vars"
COMMAND = "programma da eseguire con una sintassi valida di opzioni"

# Formato normale di sintassi
Section = "Nome_sezione" {
	Event = "Param" {
		Actions = "azione parametro; azione2 parametro; $VAR $_VARIABLE"
	}
}

// Formato di sintassi compresso (breve)
Section = "Name" { Event = "Param" { Actions = "azione parametro; azione2 parameters; $VAR $_VARIABLE" } }</pre>

**NB**:  Come si nota le azioni definite possono essere più di una e vengono separate dal simbolo **punto&virgola  ";" .** Come già detto precedentemente pekwm utilizza un file conf chiamato **vars** nella directory di riferimento **~/.pekwm** che viene solitamente incluso attraverso **INCLUDE** ed oltre alle variabili momentanee $VAR e $_VARIABLE .

**NB**: vi starete chiedendo perchè si utilizzano due variabili. Si spiega perchè c'è una differenzzazione fra le due. La prima definita come **$VAR** si riferisce ad una variavile interna. La seconda definita **$_VARIABLE** si riferisce a quelle variabili globali che quindi sono sempre riutilizzate. Ciò non impedisce di utilizzare entrambi le variabili. Esempio:

<pre>$INTERNAL = "questa è una variabile interna
$_GLOBAL = "questa è una variabile globale"

# Esempi di come leggere entrambi i tipi di variabili
RootMenu = "Menu" {
	Entry = "$_GLOBAL" { Actions = "xmessage $INTERNAL" }
}</pre>

Chiarito il concetto variabile, si utilizzano classici caratteri jolly del tipo **regexp**. Esempio: _".*, firefox"_ . Se non sapete di cosa si parla è consigliata una lettura in merito (anche se avevo in mente di scrivere un post a riguardo appena avrò tempo). Detto questo passiamo ad un esempio pratico.

Siccome le cose sono davvero tante tratteremo solo le più immediate. Facciamo conto di utilizzare un programma che richieda una propria decorazione di finestra come ad esempio un emulatore di terminale. La prima parte del nostro autoproperties si presenterà come di seguito:

<pre># Necessario per aprire Templates
Require {
 Templates = "True"
}
# Sezione dedicata alle proprietà dell'applicativo
Property = ".*,^URxvt" {
 ApplyOn = "Start New"                 # Modalità di avvio
 Border = "False"; Titlebar = "False"  # Bordi e titolo eliminati
 ClientGeometry = "+260+0"             # Posizione x - y
 Layer = "0"                           # Posizionarlo sullo schermo
 Sticky = "True"                       # Modalità di attacco a schermo
}</pre>

**NB**: in questo esempio si definiscono le proprietà del nostro emulatore prescelto (urxvt). Le varie opzioni sono commentate chiaramente. Definendo queste proprietà il risultato sarà una shell incollata al desktop sempre visibile nella posizione **x y** prescelta senza bordo finestra attivo.

Di seguito nel nostro autoproperties potrebbe essere visibile di default la sezione **TypeRules**. Questa sezione racchiude tutta un'altra serie di opzioni customizzabili come di seguito:

<pre>TypeRules {
 /*
 Desktop windows such as nautilus window in gnome. These should
 cover the root window and be below all other windows. Also they
 should not be included in the menu and in snapping.
 */
 Property = "DESKTOP" {
 FrameGeometry = "0x0+0+0"
 Titlebar = "False"
 Border = "False"
 Sticky = "True"
 Skip = "FocusToggle Menus Snap"
 Layer = "Desktop"
 Focusable = "False"
 }
 Property = "DOCK" {
 Titlebar = "False"
 Border = "False"
 Sticky = "True"
 Layer = "Dock"
 Skip = "FocusToggle Menus"
 Focusable = "False"
 }
 Property = "TOOLBAR"  {
 Skip = "FocusToggle Menus Snap"
 }
 Property = "MENU"  {
 Titlebar = "False"
 Border = "False"
 Skip = "FocusToggle Menus Snap"
 }
 Property = "UTILITY"  {
 }
 Property = "SPLASH"  {
 Titlebar = "False"
 Border = "False"
 Layer = "OnTop"
 }
 Property = "DIALOG"  {
 Layer = "OnTop"
 }
 Property = "NORMAL"  {
 }
}</pre>

**NB**: Anche in questo caso, siccome finiremo per scrivere un post di 90 pagine, vi invito a leggere la <a href="http://www.pekwm.org/files/pekwm/doc/0.1.10/html/config/autoprops.html" target="_blank">documentazione ufficiale</a> per capire meglio alcune delle opzioni presenti in Autoproperties.

**KEYS:**

In questo file conf sono racchiusi gli eventi e il keybinding della tastiera. E' abbastanza intuitivo da capire. Ovviamente si riferisce ad eventi che lanciano programmi. Sarà trattato più avanti nel blog con un post dedicato.

**MOUSE:**

In questo file conf sono invece racchiusi gli eventi relativi al mouse. Click singolo, click doppio, scrolling, etc. Come per keys sarà trattato più avanti nel blog con un post dedicato**.  
** 

**START**:

Quest'ultimo file di conf è trattato come un normale script **bash** che racchiude i programmi da startare all'avvio. Esempio:

<pre>#!/bin/sh
# PekWM start file

# ripristino comando per settare lo sfondo
# precedentemente impostato con feh --bg-center image.file
# opzioni --bg-tile ; --bg-scale etc..
eval `cat ~/.fehbg` &
# pannello e applicativi da startare in background
tint2 &
conky &
gkrellm &</pre>

**CONSIDERAZIONI FINALI:**

**PekWM** è l'esempio finale di come i **wm** in generale siano ampiamente sottovalutati per concezione e caratteristiche di customizzazione. A mio modesto modo di vedere le cose, **PekWM** risulta essere il compromesso migliore tra **Fluxbox** e **OpenBox**. Praticamente implementa tutta una serie di caratteristiche (alcune di quelle che abbiamo elencato) che se presenti su fluxbox non lo sono in openbox e viceversa. Oltre a questo aggiunge una mole notevole di cose rendendolo non solo un agglomerato dei suoi predecessori ma un innovazione degli stessi. Per porla da un altra ottica, ad oggi, non ho ancora trovato una cosa che non sia possibile definire in **PekWM**.

Come sempre, di rito , vi pasto la screen finale del mio ambiente desktop con **PekWM** che continua a risultare stramaledettamente simile a quelle degli altri due wm (hahahaa :P).

**p.s**. Cambia il wm, cambiano i contenuti, cambia il linguaggio, ma il brodo finale è sempre lo stesso.  E' sempre solo un fatto di scelte. Ne approfitto per fare a tutti i migliori auguri di Buon Natale e di inizio del nuovo anno. Al prossimo post.

<pre><a href="http://linuax.altervista.org/blog/wp-content/uploads/2009/12/pekwm_ax.jpg"><img class="alignnone size-full wp-image-1533" title="pekwm_ax" src="http://linuax.altervista.org/blog/wp-content/uploads/2009/12/pekwm_ax.jpg" alt="" width="120" height="89" /></a></pre>

\# End