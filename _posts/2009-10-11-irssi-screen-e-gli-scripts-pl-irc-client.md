---
id: 837
title: 'irssi, screen e gli scripts.pl: IRC client.'
date: 2009-10-11 20:47:31 +02:00
author: ax
excerpt: |
  <p>I client testuali sono considerati spesso obsoleti; certamente non è questo il caso, se si rapportano le straordinarie potenzialità di questo client di chat IRC: irssi.</p>
layout: post
guid: http://linuax.wordpress.com/?p=837
permalink: /2009/10/11/irssi-screen-e-gli-scripts-pl-lirc-client/
image_url:
  - http://linuax.wordpress.com/files/2009/10/irssi.jpg
  - http://linuax.wordpress.com/files/2009/10/irssi.jpg
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/10/irssi.jpg" class="alignnone size-full wp-image-838" title="irssi"   alt="irssi"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/10/irssi.jpg" class="alignnone size-full wp-image-838" title="irssi"   alt="irssi"    />'
image_colors_bg:
  - 'a:11:{i:0;s:6:"181818";s:2:"+1";s:6:"3a3a3a";s:2:"+2";s:6:"525252";s:2:"+3";s:6:"8c8c8c";s:2:"+4";s:6:"c5c5c5";s:2:"+5";s:6:"e8e8e8";i:-1;s:6:"141414";i:-2;s:6:"121212";i:-3;s:6:"0c0c0c";i:-4;s:6:"060606";i:-5;s:6:"020202";}'
  - 'a:11:{i:0;s:6:"181818";s:2:"+1";s:6:"3a3a3a";s:2:"+2";s:6:"525252";s:2:"+3";s:6:"8c8c8c";s:2:"+4";s:6:"c5c5c5";s:2:"+5";s:6:"e8e8e8";i:-1;s:6:"141414";i:-2;s:6:"121212";i:-3;s:6:"0c0c0c";i:-4;s:6:"060606";i:-5;s:6:"020202";}'
image_colors_fg:
  - 'a:11:{i:0;s:6:"c5c5c5";s:2:"+1";s:6:"c5c5c5";s:2:"+2";s:6:"ffffff";s:2:"+3";s:6:"0c0c0c";s:2:"+4";s:6:"181818";s:2:"+5";s:6:"181818";i:-1;s:6:"c5c5c5";i:-2;s:6:"c5c5c5";i:-3;s:6:"8c8c8c";i:-4;s:6:"8c8c8c";i:-5;s:6:"8c8c8c";}'
  - 'a:11:{i:0;s:6:"c5c5c5";s:2:"+1";s:6:"c5c5c5";s:2:"+2";s:6:"ffffff";s:2:"+3";s:6:"0c0c0c";s:2:"+4";s:6:"181818";s:2:"+5";s:6:"181818";i:-1;s:6:"c5c5c5";i:-2;s:6:"c5c5c5";i:-3;s:6:"8c8c8c";i:-4;s:6:"8c8c8c";i:-5;s:6:"8c8c8c";}'
image_size:
  - 'a:7:{i:0;s:3:"782";i:1;s:3:"446";i:2;s:1:"2";i:3;s:24:"width="782" height="446"";s:4:"bits";s:1:"8";s:8:"channels";s:1:"3";s:4:"mime";s:10:"image/jpeg";}'
  - 'a:7:{i:0;s:3:"782";i:1;s:3:"446";i:2;s:1:"2";i:3;s:24:"width="782" height="446"";s:4:"bits";s:1:"8";s:8:"channels";s:1:"3";s:4:"mime";s:10:"image/jpeg";}'
categories:
  - Arch
  - irssi
  - Linux
  - Scripts
  - Slackware
  - terminal
tags:
  - Arch
  - How-to
  - irc
  - irc client
  - irssi
  - Linux
  - script
  - shell
  - terminale
  - theme
---
Non poteva che arrivare il momento di parlare delle chat e dei client testuali. I progetti in merito sono parecchi, alcuni più famosi di altri. Tra questi elenco quelli che ho sempre usato io: <a href="http://www.bitchx.com/" target="_blank">bitchX</a>, <a href="http://www.weechat.org/" target="_blank">weechat</a>, [ekg2](http://www.ekg2.org/). Vuoi per sviluppo di alcuni (bitchx), vuoi per mancata ed adeguata documentazione di altri in lingue accettabili e comprensibili ai comuni mortali e per svilluppo di release (ekg2), sono sempre rimasto non troppo convinto di cosa usare. La scelta obbligata ovviamente è ricaduta sul client che utilizzavo da anni sui server in remoto: **<a href="//irssi.org" target="_blank">irssi</a>**.

**Irssi** _**-** **Il client del futuro**_ _(concordo e non in modo provocatorio)_ è senza dubbio ciò che più si avvicina a quelli che sono i _miei_ requisiti minimi: usabilità, snellicità, implementazioni, documentazione;

**NB**: Questo **non** è un how-to  su come si usa la chat, attraverso IRC; dovrebbe solo servire a chiarire alcune piccole cose riguardo **irssi** per facilitare l'utente nell'uso quotidiano dello stesso.

**IL PROGRAMMA**:

Per startarlo, essendo pensato come client text, apriamo un emulatore di terminale a scelta o direttamente da bash shell:

<pre>~$ irssi</pre>

Startato, l'interfaccia di base di default è costituita da una **Topicbar** _(in alto)_ - **Statusbar** _(in basso)_ - **Prompt** (sotto la Statusbar; dove si digitano i comandi). Al primo avvio sarà creata se non esiste la directory _~/.irssi/_ contenente i settaggi generali e gli eventuali temi e script utili alla customizzazione.

&nbsp;

Per effettuare la connessione si usa il comando _/server_ o anche _/connect_:

<pre>/server irc.server_scelto.it porta
/connect irc.server_scelto.it porta</pre>

Per visionare la lista server:

<pre>/server list</pre>

I comandi generali sono gli stessi di tutti i client di chat IRC: _**join,part,kick,ban,quit**_ etc...

<span style="color: #ff0000;"><span style="color: #000000;"><strong><span style="color: #ff0000;">NB</span>:</strong></span> <span style="color: #000000;">Lo sfondo, le trasparenze, i colori, la codifica del charset sono relativi al tipo di emulatore di terminale scelto e chiaramente ai settaggi del sistema.</span></span>

Per spostarsi tra i canali si usa la combinazione di tasti:

<pre>ALT + numero</pre>

**NB**: numeri compresi [0-9], esempio: ALT + 1 ; ALT + 2 ; ALT + 3

**NB**: se i canali superano tale range _(più di 10 canali)_,  saranno aggiunte ricorsivamente i range di lettere della tastiera \[q-p\] \[a-l\] etc...

Irssi fa riferimento a comandi _(vedi /help)_ che molto spesso sono anche riconfigurati sotto forma di alias. La finalità sta nel definire una serie di comandi _abbreviati_ per sopperire al bisogno di digitare non solo comandi ma anche stringhe numericamente lunghe ad ogni eventuale bisogno. Esempio, per avviare una query si può usare:

<pre>/query nick</pre>

o anche l'alias:

<pre>/q nick</pre>

Stesso discorso per chiudere di una finestra; query,canale, o finestra splittata (di cui si parlerà più avanti):

<pre>/window close</pre>

o anche l'alias:

<pre>/wc</pre>

o anche:

<pre>/win k</pre>

Per una lista degli alias settati in uso:

<pre>/alias</pre>

**NB**: Una delle funzionalità migliori di irssi è l'implementazione d'uso del tasto [TAB] , che funge da _**bash completion**_ della shell. Gli utenti in linux sanno bene l'efficacia di questa funzionalità. Esempio in un canale:

<pre>/q a[TAB]</pre>

Mostrerà e scorrerà a schermo tutti i nick che cominciano per _"a"_, ad ogni successiva pressione cambierà il nick da querare. Stesso discorso per i settaggi:

<pre>/set [TAB]</pre>

o anche in specifico:

<pre>/set n[TAB]</pre>

Mostrerà tutte le opzioni di setting che cominciano con la lettera _"n"_ (nick,nicklist,etc). Per altre info:

<pre>/help opzione</pre>

**TEMI**:

La directory di riferimento è sempre ~/.irssi ; è sufficiente scaricare il file di tema scelto in formato *.theme posizionato nella directory di riferimento. A seguito il comando:

<pre>/set theme nome_tema_scelto.theme</pre>

E' disponibile una vasta gamma di temi dal <a href="http://www.irssi.org/themes" target="_blank">link</a> ufficiale.

**SCREEN**:

Screen è un applicativo che è decisamente utile quando si lavora su più shell contemporaneamente e si vuole evitare di aprire 2-3-4 terminali con conseguenti login e/o processi sulla stessa box. Esempio: sono sul _**server1**_ e mentre lavoro a _config.txt_ con **vi** ho bisogno di greppare una stringa da _esempio.txt_; come faccio ad evitare di uscire momentaneamente dal primo editor aperto per digitare i comandi in shell che mi servono per continuare il mio lavoro essendo in remoto o anche in locale? Ecco che screen ci aiuta tenendo aperta una sessione in modo virtuale per noi. Siccome irssi è un client di chat che sfrutta la shell e siccome non è detto che il nostro emulatore di terminale abbia la modalità tabbed e/o vogliamo evitare di aprire 2-3-4 terminali contemporaneamente andando inutilmente ad appesantire il sistema stesso (è cosa buona e giusta :P) utilizzare screen per virtualizzare irssi. Questa cosa ci consentirà in ogni momento di ritornare alla nostra shell di base, nel frattempo screen si preoccuperà di tenere aperta la sessione irssi.

**NB**:  Potrebbe sembrare macchinoso spiegato, ma è tutto molto semplice. Ovviamente per uscire "modalità detach - staccata" e/o riaprire "modalità attach" si usano delle facili combinazioni di tasti preimpostate attraverso **keys**. Esempio d'uso:

<pre>~$ screen irssi</pre>

Aquesto punto, si aprirà una sessione screen con irssi. Volendo virtualizzare "detaching" il processo si userà la combinazione di tasti:

<pre>CTRL + a + d</pre>

o anche:

<pre>CTRL +a  CTRL +d</pre>

Ritornati in shell otterremo come output:

<pre>[detached]</pre>

possiamo visionare i processi virtualizzati in corso:

<pre>~$ screen -list
There is a screen on:
 9162.irssi    (Attached)
1 Socket in /tmp/screens/S-user.</pre>

Stesso risultato ottenibile con:

<pre>~$ screen -ls</pre>

Come da esempio il comando ci mostra quanti e quali processi sono attualmente detacchati con relativo socket e percorso.

Per riattaccare il processo attualmente virtualizzato:

<pre>~$ screen -raAd</pre>

**NB:** riattaccherà l'applicativo "irssi" nel nostro caso solo se non ci sono altri applicativi detachati in corso, altrimenti sarà necessario specificare quale di essi riattaccare specificando il socket.

<pre>~$ screen -r socket</pre>

esempio:

<pre>~$ screen -list
There are screens on:
 11260.pts-2.genarch    (Detached)
 <span style="color: #99cc00;"><strong>11245.pts-2.genarch </strong> </span>  (Detached)
2 Sockets in /tmp/screens/S-user.
~$ screen -r 11245.pts-2.genarch</pre>

**NB**: riattaccherà il secondo processo listato identificato con il socket **11245.pts-2.genarch.**

Volendo ****killare uno dei due socket si utilizzerà il comando:

<pre>~$ screen -X -S socket kill</pre>

Come da esempio precedente, volendo killare il secondo processo in modalità screen:

<pre>~$ screen -X -S<span style="color: #99cc00;"><strong> </strong><strong> </strong><span style="color: #000000;"><strong>11245.pts-2.genarch</strong> kill </span></span></pre>

Per altre info vi rimando al _**man screen**_ .

Può  essere comodo a questo punto creare un alias al comando_ ******'screen irssi'**_. Modificare **~/.bashrc** o **/etc/profile** come da esempio:

<pre>~$ vi ~/.bashrc
alias irssi='screen irssi'</pre>

Idem per richiamare irssi in caso di detach di una singola sessione aperta in screen:

<pre>alias screenr='screen -raAd'</pre>

**SCRIPTS**:

Irssi di base può sembrare "scarno" e molto spesso si può presentare l'esigenza di abilitare funzioni che non fanno parte del client di base. Per questa ragione irssi permette l'utilizzo di scripts esterni in perl.

**NB**: Alcuni script richiedono i moduli perl adeguati al caso installati. Se fosse necessario è possibile installare i moduli richiesti dallo script, attraverso CPAN, come da esempio generico:

<pre>~# perl -MCPAN -e 'install LWP::Simple'</pre>

**NB**: E' un esempio di utilizzo di CPAN per installare moduli aggiuntivi perl richiesti da un eseguibile.pl , va adeguato al tipo di modulo richiesto se non fosse presente. Non mi dilungo altrimenti ci vorrebbe un corso accellerato solo per CPAN.

Tornando agli scripts, la directory di riferimento è _**~/.irssi/scripts**_ e/o anche _**~/.irssi/scripts/autorun**_ per gli scripts che si intendono runnare automaticamente all'avvio del client.

**NB**: gli script autoloadati non richiedono ovviamente di essere richiamati con i comandi opportuni dal client dopo lo start.

Di default le directory non esistono; per crearle:

<pre>~$ mkdir ~/.irssi/scripts ; mkdir ~/.irssi/scripts/autorun</pre>

Sul <a href="http://scripts.irssi.org/" target="_blank">link ufficiale</a> sono raccolti numerosi scripts per le più svariate funzioni. Io elenco quelli che reputo fondamentali per vivere una vita serena di chat IRC:

**<a href="http://scripts.irssi.org/scripts/nicklist.pl" target="_blank">nicklist.pl</a>** : abilita la nicklist sul lato destro del client.

Scarichiamo in una delle directory di riferimento; poi da client irssi:

<pre>/script load nicklist.pl</pre>

Se irssi è runnato con screen e si intende abilitare la nicklist in screen:

<pre>/nicklist SCREEN</pre>

Se vogliamo runnare automaticamente lo script all'avvio di irssi:

<pre>~$ echo "/script load nicklist.pl" &gt;&gt; $HOME/.irssi/startup

/set nicklist_automode SCREEN
/save</pre>

**<a href="http://scripts.irssi.org/scripts/oidenty.pl" target="_blank">oidenty.pl</a>** : abilitazione identificazione ed eventualmente spoofing del servizio auth "ident" attraverso oidentd.

**NB**: richiede chiaramente opportuna installazione e configurazione del demone **oidentd**.

**<a href="http://irssi.org/scripts/scripts/usercount.pl" target="_blank">usercount.pl</a>** : abilitazione dello stat numerico degli utenti presenti nella statusbar.

<pre>/load usercount.pl</pre>

o anche:

<pre>/run usercount.pl
/script load usercount.pl</pre>

**NB:** sono tutti comandi che richiamano uno script.

<pre>/statusbar window add usercount</pre>

**<a href="http://anti.teamidiot.de/static/nei/*/Code/Irssi/adv_windowlist.pl" target="_blank">adv_windowlist.pl</a>** : statusbar _avanzata_ che mostrerà il nome dei canali seguita della numerazione classica 1,2,3 ...

Dopo averla loadata:

<pre>/set awl_display_key $Q%K|%n$H$C$S
/set awl_block -15</pre>

**NB**: è possibile mantanere la statusbar classica compresa di **usercount.pl** per comodità ed abilitarle entrambe oppure decidere di eliminare la vecchia statusbar.

<pre>/statusbar window visible inactive</pre>

**NB**: consultare /help statusbar.

Salviamo i cambiamenti:

<pre>/save</pre>

**NB**: per invertire il processo e ritornare alla vecchia statusbar:

<pre>/script unload adv_windowlist
/statusbar window visible active</pre>

<a href="http://scripts.irssi.org/scripts/trackbar.pl" target="_blank"><strong>trackbar.pl</strong></a> : un linea di demarcazione stile mIRC, che segna il testo e le azioni ancora da leggere.

<a href="http://scripts.irssi.org/scripts/mouse.pl" target="_blank"><strong>mouse.pl</strong></a> : implementazione delle funzioni di gestione del client irssi attraverso il mouse (cosa non supportata da irssi e in alcuni casi molto comoda), compreso lo scrolling.

Le funzioni si aggiungono a quelle default, facilmente visionabili con il solito comando:

<pre>/set</pre>

Di default lo scrolling è settato per funzionare sulla finestra attiva, io preferisco averlo sulla nicklist implementata con **nicklist.pl** per comodità. Esempio:

<pre>/set mouse_scroll_down /nicklist scroll +20
/set mouse_scroll_up /nicklist scroll -20</pre>

**NB**:  una volta abilitato e richiamato lo script, per abilitare il copy/paste nelle finestre attive bisogna cliccare due volte nella finestra attiva. Il mouse restarà di default per 5 secondi in modalità copy/paste.  Si può decidere di cambiare il timer ovviamente.  Stesso discorso per la funzione di spostamento dei canali tenendo in pressione il tasto sinistro e spostando il mouse verso destra/sinistra. Anche questa funzione è ampiamente configurabile.

Il risultato è un client _"minimalistico"_ che non ha nulla da invidiare in funzioni rispetto ai migliori client grafici (xchat,kvirc,etc..)

&nbsp;

**Considerazioni**:  capite bene che questo post è solo una panoramica più dettagliata del client, rivolta a quegli utenti che per mancata informazione ritengono i client testuali, in specifico **irssi** poco consono ad un uso casalingo e deficitario rispetto al prodotto client/usabilità/chat. Spero sia stato in minima parte utile per cambiare quell'idea, tenendo sempre a mente che poi, alla fine, è una questione di scelte.

\# End
