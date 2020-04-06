---
id: 1400
title: 'SLKBUILD: creazione di pacchetti Slackware'
date: 2011-06-17T20:07:37+01:00
author: ax
excerpt: |
  <p>Sempre più spesso negli ultimi posts in relazione alla distribuzione GNU/Linux Salix ho speso non pochi pregi nel menzionare slkbuild. Ciò che da sempre rende Slackware poco idonea allo sviluppo di aggiornamenti e che da sempre spaventa gli utenti meno esperti "ovvero i pacchetti" rende l'avvicinarsi a Slackware leggermente traumatico, almeno nelle fasi iniziali. La risposta: SLKBUILD!</p>
layout: post
guid: http://linuax.wordpress.com/?p=1400
permalink: /2011/06/17/slkbuild-semplicitaefficacia-ed-efficienza-nella-creazione-di-pacchetti-slackware/
email_notification:
  - "1308337659"
  - "1308337659"
jabber_published:
  - "1308337659"
  - "1308337659"
categories:
  - How-to
  - Linux
  - Programmi
  - Salix
  - Scripts
  - Slackware
  - Tips
tags:
  - How-to
  - Linux
  - salix
  - slackware
  - slkbuild
---
Sempre più spesso negli ultimi posts in relazione alla distribuzione GNU/Linux **Salix** ho speso non pochi pregi nel menzionare **slkbuild**. Ciò che da sempre rende Slackware poco idonea allo sviluppo di aggiornamenti e che da sempre spaventa gli utenti meno esperti "ovvero i pacchetti" rende l'avvicinarsi a Slackware leggermente traumatico, almeno nelle fasi iniziali.

****Se ricordate un pò di tempo fà si era parlato nel blog di uno strumento molto efficace per la creazione di pacchetti Slackware: [**src2pkg**](http://linuax.wordpress.com/2009/11/21/creazione-e-gestione-pacchetti-da-tarball-source-su-slackware-src2pkg/), cui va onore e merito. Bene, con **Salix** e con l'implementazione di **slkbuild** in una distribuzione Slackware based, il discorso si è evoluto fino a raggiungere più che un ottimo compromesso per la creazione di pacchetti per Salix e ovviamente pienamente compatibili con Slackware. La guida sarà suddivisa in due fasi per semplicità di didattica. La prima dase sarà relativa alla teoria e la seconda fase relativa alla pratica partendo da un esempio di pacchetto costruito da me per i repository ufficiali **Salix**.

# FASE 1° TEORIA

### **Slkbuild: Cos'è e come agisce ?**

Slkbuild è uno script che rende la vita decisamente facile nella creazione di un pacchetto ***.txz** . Il suo scopo è quello di analizzare un file chiamato appunto **SLKBUILD** in cui risiedono tutte le principali istruzioni relative al pacchetto da creare. Come primo passo viene creato uno **script-build.sh** entro cui vengono eseguite una serie di operazioni di compilazione ed eventualmente le opzioni dichiarate nel pacchetto. Il tutto tradotto, per farla breve può essere differenziato in:

**slkbuild** è il **tool  
** 

**SLKBUILD** è il file contenente le istruzioni****. Per altro molto simile ai **PKGBUILD** che si utilizzano su Arch Linux. Le differenze sono minime e risiedono nella gestione di alcune opzioni, ma in linea di massima il concetto è pressochè identico.

Fatta una panoramica generica vediamo in dettaglio le funzioni dello **script-build.sh**:

  1. **Mette i file in /usr/src/$pkgname-$pkgver** -  copia i file sorgenti che non contengono URL
  2. **Estrae l'archivio sorgente** - in dettaglio sarà spiegato dopo nell'esempio. In particolare nella fase build (). Il sorgente sarà estratto in modo automatico. Tutto ciò che si dovrà fare per procedere con le istruzioni di compilazione sarà un cd nella directory scompattata in modo automatico.
  3. **Pulisce la vecchia costruzione** - a volte è necessario costruire un pacchetto con una serie di istruzioni e più di una volta. Prima di creare il pacchetto finale, e che il build proceda, i files vecchi sono ripuliti. Questo consente come finale di avere un pacchetto pulito.
  4. **Dowload automatico del source** - se si fornisce un URL sorgente nell'SLKBUILD lo script si occupa di scaricarlo in modo automatico. Questa funzione non si verifica solo nel caso in cui il pacchetto source sia già presente nella directory in cui viene lanciato slkbuild.
  5. **Imposta i permessi** - si assicura che tutti i permessi siano impostati in modo standard.
  6. **Controlla i files di menu** - controlla che tutte le Icon= variabili nei file .desktop non abbiano estensione
  7. **Controllo sui binari** - cerca tutti i binari e li separa dai simboli
  8. **Controllo sulle pagine di man ed info** - verifica se le pagine man siano state installate in /usr/share. In questo caso le sposta in /usr e le zippa con gzip. A tale condizione l'istruzione di compilazione -mandir=/usr/man non risulta necessaria.
  9. **Copia dopo la costruzione build** - copia lo script **build-$pkgname.sh** creato da slkbuild e il rispettivo SLKBUILD.
 10. **Crea il pacchetto** - fa il makepkg e calcola l'md5sum.
 11. **Copia il file .desktop** - **** copia il file .desktop in /usr/share/applications .

**NOTA bene:** è solo una panoramica illustrativa. Non temete se alcune cose vi sfuggono adesso. Sarà tutto più chiaro dopo aver letto il file SLKBUILD.

## **SLKBUILD**

Dopo aver illustrato la teoria passiamo alla pratica. La prima cosa da fare è scrivere un file chiamato **SLKBUILD** e posizionarlo in una directory pulita che è strutturato come da esempio:

<div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="bash codecolorer">
    <span class="co0">#Creatore del pacchetto: Nome Cognome e/o nickname esempio Alex Zan <email@address.com></span><br /> <span class="co0">#Collaboratore: Nome Cognome e/o nickname <email@address.com></span><br /> <br /> <span class="co0">#Campi obbligatori</span><br /> <span class="re2">pkgname</span>=libdv<br /> <span class="re2">pkgver</span>=1.0.0<br /> <span class="re2">pkgrel</span>=1az<br /> <span class="re2">arch</span>=i486<br /> <span class="re2">source</span>=<span class="br0">&#40;</span><span class="st0">"<a title="</span>http:<span class="sy0">//</span>project-xy.net<span class="sy0">/</span>dl<span class="sy0">/</span><span class="re1">$pkgname</span><span class="sy0">/</span><span class="re1">$pkgname</span>-<span class="re1">$pkgver</span>.tar.gz<span class="st0">" href="</span>http:<span class="sy0">//</span>project-xy.net<span class="sy0">/</span>dl<span class="sy0">/</span><span class="re1">$pkgname</span><span class="sy0">/</span><span class="re1">$pkgname</span>-<span class="re1">$pkgver</span>.tar.gz<span class="st0">" rel="</span>nofollow<span class="st0">">http://project-xy.net/dl/<span class="es2">$pkgname</span>/<span class="es2">$pkgname</span>-<span class="es2">$pkgver</span>.tar.gz</a>"</span> <span class="st0">"thing.desktop"</span> <span class="st0">"anyothersourcestuff"</span><span class="br0">&#41;</span><br /> <span class="re2">sourcetemplate</span>=<span class="sy0"><</span>a <span class="re2">title</span>=<span class="st0">"http://my-server.net/packages/<span class="es2">$pkgname</span>/"</span> <span class="re2">href</span>=<span class="st0">"http://my-server.net/packages/<span class="es2">$pkgname</span>/"</span> <span class="re2">rel</span>=<span class="st0">"nofollow"</span><span class="sy0">></span>http:<span class="sy0">//</span>my-server.net<span class="sy0">/</span>packages<span class="sy0">/</span><span class="re1">$pkgname</span><span class="sy0">/</</span>a<span class="sy0">></span><br /> <span class="co0">#Campi opzionali</span><br /> <span class="re2">docs</span>=<span class="br0">&#40;</span><span class="st_h">'authors'</span> <span class="st_h">'copying'</span> <span class="st_h">'changelog'</span> <span class="st_h">'install'</span> <span class="st_h">'news'</span> <span class="st_h">'readme'</span><span class="br0">&#41;</span><br /> <span class="re2">url</span>=<span class="st0">"<a title="</span>http:<span class="sy0">//</span>libdv.sourceforge.net<span class="sy0">/</span><span class="st0">" href="</span>http:<span class="sy0">//</span>libdv.sourceforge.net<span class="sy0">/</span><span class="st0">" rel="</span>nofollow<span class="st0">">http://libdv.sourceforge.net/</a>"</span><br /> <span class="re2">dotnew</span>=<span class="br0">&#40;</span><span class="st_h">'etc/thing'</span> <span class="st_h">'etc/foo'</span> <span class="st_h">'etc/bar'</span><span class="br0">&#41;</span><br /> <span class="re2">options</span>=<span class="br0">&#40;</span><span class="st_h">'noautodotnew'</span><span class="br0">&#41;</span><br /> <span class="re2">CFLAGS</span>=<span class="st0">"-03 -funrolloops"</span><br /> <span class="re2">CXXFLAGS</span>=<span class="st0">"-03 -funrolloops"</span><br /> <br /> <span class="re2">slackdesc</span>=<br /> <span class="br0">&#40;</span><br /> <span class="co0">#|-----lunghezza-massima-per-le-righe-di-descrizione--------------------|</span><br /> <span class="st0">"libdv (software codec for DV video)"</span><br /> <span class="st0">"The Quasar &nbsp;DV codec (libdv) is a software codec for DV video, the"</span><br /> <span class="st0">"encoding format used by most digital camcorders, typically those that"</span><br /> <span class="st0">"support the IEEE 1394 (a.k.a. FireWire or i.Link) interface. Libdv was"</span><br /> <span class="st0">"developed according to the official standards for DV video: IEC 61834"</span><br /> <span class="st0">"and SMPTE 314M"</span><br /> <span class="br0">&#41;</span><br /> <br /> build<span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">cd</span> <span class="re1">$startdir</span><span class="sy0">/</span>src<span class="sy0">/</span><span class="re1">$pkgname</span>-<span class="re1">$pkgver</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; .<span class="sy0">/</span>configure <span class="re5">--prefix</span>=<span class="sy0">/</span>usr <span class="re5">--libdir</span>=<span class="sy0">/</span>usr<span class="sy0">/</span>lib<span class="co1">${LIBDIRSUFFIX}</span> <span class="re5">--localstatedir</span>=<span class="sy0">/</span>var <span class="re5">--sysconfdir</span>=<span class="sy0">/</span>etc<br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">make</span> <span class="sy0">||</span> <span class="kw3">return</span> <span class="nu0">1</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">make</span> <span class="re2">DESTDIR</span>=<span class="re1">$startdir</span><span class="sy0">/</span>pkg <span class="kw2">install</span><br /> <span class="br0">&#125;</span><br /> <br /> doinst<span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span><br /> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;comandi da lanciare dopo l<span class="st_h">'installazione del pacchetto<br /> }<br /> <br /> # Fine dell'</span>SLKBUILD relativo al pacchetto libdv
  </div>
</div>

**Nota bene:** per i meno esperti i simboli **#** sono ovviamente righe di commento. Non ci resta che un analisi dei campi obbligatori/opzionali sulla base del nostro SLKBUILD di esempio e della sintassi del comando.

## **Campi obbligatori:**

**NOTA:** le lettere che risulteranno marcati, corsivi e colorati sono ovviamente il nostro riferimento.

<pre><strong>pkgname</strong>= nome del pacchetto, il nome del software:<span style="color: #ff0000;"><em> <strong>libdv</strong></em></span>-1.0.0-i486-1az.txz</pre>

**pkgver**= versione del pacchetto, la versione del software: libdv-<span style="color: #ff0000;"><em><strong>1.0.0</strong></em></span>-i486-1az.txz

**pkgrel**= release del pacchetto, il numero di compilazioni e le lettere indicative del creatore "az" subito prima del suffisso *.txz: libdv-1.0.0-i486-<span style="color: #ff0000;"><em><strong>1az</strong></em></span>.txz

**arch**= l'architettura, di solito i486 oppure noarch, in alcune occasioni i686: libdv-1.0.0-<span style="color: #ff0000;"><em><strong>i486</strong></em></span>-1az.txz. Dalla release slkbuild-0.7.0, questo campo non è più obbligatorio e sarà automaticamente decretato dall'architettura del sistema. Diventa necessario solo per i pacchetti che si intende flaggare 'noarch'.

**source**= sono tutti i files necessari alla costruzione del pacchetto. Si può inserire un url da dove scaricare il pacchetto nel caso il sorgente di partenza non sarà presente nella directory di avvio dell'SLKBUILD. Se si dispone di più file di partenza come ad esempio una patch e/o un file di icona sarà sufficiente inserire le istruzioni come da esempio: _**source=("altro_archivio.tar.gz" "file.patch" "bar.icon")**_. Se non si specifica un URL per un file esso sarà copiato all'interno del pacchetto nella directory: _**/usr/src/$pkgname-$pkgver**_ .

**slackdesc**= Queste sono le linee dedicate per la descrizione nel pacchetto. Bisogna assicurarsi che ogni riga sia inferiore a 70 caratteri per evitare che non venga visualizzata al momente dell'installazione correttamente. Ci sono delle brevi regole generali per la descrizione di un pacchetto. La prima linea ha il nome e una breve descrizione. Le linee  sotto hanno una descrizione più ampia. Non è necessario distaccare le prima due linee siccome slkbuild lo fa automaticamente. Bisogna ricordarsi di mettere le virgolette ad inizio e fine di ogni riga " esempio ". Infine bisogna assicurarsi di non superare le 10 righe in totale. Slkbuild esegue un controllo su queste regole e nel caso si ecceda con le linee lo dirà.

**build() -** questa è la funzione che crea affettivamente il pacchetto. Per capire il funzionamento è necessario capire cosa fa lo script di build. Per prima cosa viene lanciato un "**_pwd_**" per **_$startdir_**. Subito dopo sarà creata **_$startdir/src_** . Subito dopo sarà copiato tutto in **_$startdir/src_** dove verrà effettuata l'estrazione dell'archivio.  Verrà creato _**$startdir/pkg**_ . Il compito di  _**build ()**_ è di fare tutto ciò che deve fare in _**$startdir/src**_ per ottenere _**$startdir/pkg**_ come DESTDIR finale. Tutte le altre operazioni come compressione,pulitura delle dir inutili etc, sono come già detto gestite da slkbuid quindi non è necessario preoccuparsene.  Oltre al **"cd"** ed ai  normali comandi di compilazione potrebbe tornare utile l'utilizzo di_ **"make || return 1"**_ che si occuperà di arrestare lo script se la compilazione presenterà errori.

## **Campi opzionali:**

**sourcetemplate**= specifica la posizione URL dove SLKBUILD e build-script.sh possono essere scaricati. Tutti i file _sorgenti_ senza URL devono essere disponibili in questo link.

**docs**= specifica uno qualsiasi dei documenti che devono essere copiati. Readme, install, changelog, ecc. Non c'è bisogno di specificare dove perchè slkbuild fa una ricerca ricorsiva per verificare la posizione.

**options=**  è un array che consente di controllare alcuni comportamenti dello _**script-build.sh**_ . Le opzioni disponibili sono:  <span style="color: #ff0000;"><em><strong>'noextract'</strong></em></span> che impedisce l'estrazione automatica del sorgente. Se si applica tale opzione bisognerà includere nel **build()** i comandi di estrazione ovviamente. <span style="color: #ff0000;"><em><strong>'nostrip'</strong></em></span> impedisce l'esecuzione della funzione di stripping (separazione). <span style="color: #ff0000;"><em><strong>'Noautodotnew'</strong></em></span> viene utilizzato per rimuovere la gestione automatica .dotnew in tutti i file in  'tgz', 'tbz' e 'TLZ' e/o nel formato supportato.Di default è **TXZ**. Se si imposta più di un formato come: 'tgz', 'tbz' e 'TLZ', solo il primo verrà utilizzato.

**url**= Homepage url o altre informazioni sul software.

**dotnew**=  Sono in genere i file di configurazione. Essi saranno rinominati con estensione_ **.new**_ . L'aggiunta di tali file sarà specificata in doinst.sh nel caso si voglia applicare al pacchetto.

**doinst() -** rappresenta l'aggiunta di comandi che si desidera eseguire subito dopo l'installazione del pacchetto che non verranno aggiunti automaticamente da makepkg nella costruzione del pacchetto.

**CXXFLAGS/CFLAGS=** si può scegliere di settare le flags che di default sono_ **"-02 -march=$arch -mtune=i686"**_ dove $arch rappresenta la variabile settata in SLKBUILD. Notate bene che se si sceglie di cambiare bisogna mettere le flag per intero. Se ad esempio si desidera cambiare **_-02_** in_ **-03**_ bisogna mettere _**CFLAGS="-03 -march=$arch -mtune=i686"**_  e **non** solo _CFLAGS="-03"_ .

## Sintassi del comando:

Finite queste nozioni fondamentali passiamo alla sintassi di lancio del comando **slkbuild** in Salix.

_**slkbuild**_ eseguito senza argomenti crea un tradizionale build-$pkgname.sh che successivamente può essere analizzato e lanciato per la costruzione del pacchetto vero e proprio.****

_**slkbuild -c**_  rimuove la directory src e pkg.****

_**slkbuild -x**_ fa la stessa cosa di slkbuild lanciato senza argomenti fatta eccezione per lo script build che viene lanciato.****

_**slkbuild -X**_  combina l'opzione -c e -x insieme. E' anche il modo migliore di lanciare la costruzione di un pacchetto.

**slkbuild -g[prototype]** copia un prototipo da /etc/slkbuild nella directory corrente. L'opzione prototype è in realtà l'estensione del nome del prototipo dell'SLKBUILD fra quelli presenti in /etc/slkbuild. Quindi, se faremo **slkbuild-gpython,** sarà copiato/etc/slkbuild/SLKBUILD.python. Se faremo **slkbuild-gfoo,** sarà copiato /etc/slkbuild/SLKBUILD.foo. Se il comando viene eseguito senza argomenti, ovvero **slkbuild -g,** sarà copiato il file /etc/slkbuild/SLKBUILD.

_**slkbuild -v**_ ci mostrerà ovviamente la versione di slkbuild in uso.

**Nota bene:** per evitare problemi di permessi è buon uso utilizzare slkbuild attraverso il comando fakeroot. Per capirci il comando:

<pre>~$ slkbuild -X</pre>

diventerà:

<pre>~$ fakeroot slkbuild -X</pre>

# **FASE 2° - PRATICA **

Ore che abbiamo spiegato a livello generale cos'è e come si costruisce, facciamo un esempio pratico. Ovviamente la distro di riferimento visto che da pochissimi giorni assieme a kerd è stata rilasciata sarà: **Salix OS 13.37 (Fluxbox)**. Partendo da un sistema pulito di base i passi da eseguire per costruire ed installare un pacchetto che ci interessa sono i seguenti:

**NOTA:** nell'esempio il pacchetto voluto è **FluxMenu revision 24**. Visto che la distro in tema monta fluxbox alcuni utenti potrebbero volerlo nel parco pacchetti. Essendo un editor di menu grafico di Fluxbox potrebbe far comodo. Proprio in queste ore dovrebbe essere disponibile anche sui repository Salix in ogni caso.

**NOTA  BENE:** essendo non specificata l'architettura da utilizzare, nell'esempio viene presa quella di default nel mio caso ovvero i486 (processore 32bit).

1) Scriviamo e/o salviamo il build in un file **SLKBUILD** come quello preso per l'esempio seguente e lo posizioniamo in una directory pulita.

<pre>~$ mkdir FluxMenu
~$ cd FluxMenu
~$ wget<strong> <a href="http://people.salixos.org/ax/fluxMenu/24/SLKBUILD">SLKBUILD</a></strong></pre>

2) Lanciamo il build del pacchetto con slkbuild.

<pre>~$ fakeroot slkbuild -X</pre>

3) Costruito il pacchetto automaticamente, basterà installarlo con il metodo tradizionale Slackware ovviamente:

<pre>~$ su
Password:
~# installpkg fluxMenu-24-i486-1ax.txz</pre>

**NOTA BENE:** nella directory FluxMenu ovviamente ci saranno gli altri file generati automaticamente da slkbuild. Al termine dell'installazione del pacchetto, potrete tranquillamente cancellare i files e conservare esclusivamente il build per poterlo uppare in uno spazio FTP per comodità etc etc ... è a discrezione vostra.

4) Lanciare l'applicativo appena installato. Nel nostro caso di esempio:

<pre>~$ fluxmenu.sh</pre>

Niente di più semplice direi! :)

## Considerazioni personali:

Come si nota da questo thread, **SLKBUILD** è un amico fidato ed importante. Io personalmente la trovo una cosa geniale implementata di default in un sistema come Slackware. Motivo che mi ha spinto a collaborare nel team **Salix**. Tralasciando tanti discorsi che mi porterebbero ad allungare di parecchio il thread, e considerando che io sono da sempe un conservatore orgoglioso di Slackware. Credo proprio che questa sia stata una delle cose che più ho apprezzato di questa distribuzione Salix. Ed è per questo che ho pensato di scrivere qualche riga a riguardo.

Sarebbe molto bello in un futuro "spero" prossimo per quanto mi riguarda ovviamente, vedere la nascita di una sorta di [**AUR**](http://aur.archlinux.org/index.php?) su **Salix**. Credo che la cosa porterebbe solo grandi vantaggi per gli utenti. Senza trascurare la solidità e l'impeccabilità che un sistema in tutte le sue parti compatibile a pieno con Slackware può dare.

**Links e riferimenti:**

Ovviamente tutta la parte teorica non è che un sunto in italiano della documentazione presente sul sito **Salix OS**: [**QUI**](http://www.salixos.org/wiki/index.php/Building_packages_with_slkbuild)

### **\# End**
