---
id: 912
title: 'Creazione pacchetti source su Slackware-src2pkg.'
date: 2009-11-21T09:00:02+01:00
author: ax
excerpt: "Src2pkg è un ottimo tool di gestione e creazione di pacchetti partendo da source-tarball. Src2pkg estrae,compila e installa applicazioni permettendo all'utente medio di creare pacchetti con una facilità disarmante ed un più che alto livello di customizzazione per l'utente esperto."
layout: post
guid: http://linuax.wordpress.com/?p=912
permalink: /2009/11/21/creazione-e-gestione-pacchetti-da-tarball-source-su-slackware-src2pkg/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Linux
  - Slackware
  - terminal
tags:
  - How-to
  - Linux
  - slackware
---
**Considerazioni personali prepost:**

Molti ritengono che distribuzioni come  Slackware manchino di requisiti minimi di usabilità. In primis ciò che li frena è la gestione delle dipendenze dal punto di vista software  _"A dipende da B che a sua volta dipende da C"_ ( problema noto a chi per giorni ha compilato a mano più e più pacchetti nel disperato tentativo di ritrovarsi un parco software quasi perfetto e un sistema stabile, magari anche esente da crash). Io personalmente non ho mai considerato questa cosa un problema, ne tantomeno un difetto... Ritengo invece che sia un grosso vantaggio da svariate angolazioni. Ciò che piuttosto mi ha sempre ispirato è l'idea di avere un tool che gestisse i pacchetti nativi "formato _source_" e che con le giuste opzioni  **rigorosamente** inserite da me in maniera manuale e/o mediante script esterno (sempre scritto da me "della serie:  per personale uso e consumo") mi restituisca il pacchetto finale compilato e pronto per essere installato. Una sorta di tool appositamente creato per distro source-based come "Sourcerer/Lunar" per intenderci. A tal proposito poco tempo fà mi sono accorto che esisteva. Si chiama **<a href="http://www.src2pkg.net/" target="_blank">src2pkg</a>**.

**Cos'è e cosa fà:**

Src2pkg è appunto un tool di gestione di pacchetti **source** per Slackware comunemente zippati in tarball dalle specifiche e varie caratteristiche di compressione: tar,tgz,tar.bz2,tar.gz,tbz.

Src2pkg scarica, estrae, compila e crea pacchetti attraverso l'uso di script che sfruttano comandi comuni quali configure,make,checkinstall/make install,etc... Riesce a gestire oltre a source formato tarball anche pacchetti binari di tipo \*.rpm o \*.deb come vedremo in seguito.

Il programma utilizza Trackinstall e Tracklist per creare i pacchetti. Il primo è una sorta di Checkinstall ( molto familiare a chi utilizza Slackware ) con una serie di modifiche aggiuntive. Estrae il contenuto di un source-tarball in una directory specificata e/o temporanea  poi compila e crea il pacchetto. Permette la generazione di script in maniera automatizzata o differenziata utile alla ricompilazione degli applicativi.

Tracklist invece crea i log di tutti i comandi dati e le directory create durante la creazione dei pacchetti. Non è intrusivo, crea un file chiamato FILELIST nella current directory e può essere richiamato mediante apposito comando.

Tutti gli script sono richiamati da src2pkg che implementa una serie di opzioni di sintassi che facilitano l'utente per creare ed eventualmente richiamare il package creato mediante script.

**NB**: come già detto nella prefazione e come ribadito dall'autore: src2pkg **non** risolve dipendenze e **non** è stato creato per sostituire applicativi destinati all'upgrade generale o aggiornamento del sistema. Il tool infatti, se non fosse chiaro a tutti, **non** mira ad essere un gestore di pacchetti generale e di upgrade di sistema come possono esserlo emerge,slackpkg, apt-get e tutti quelli che vi vengono in mente...

**Vantaggi di utilizzo:**

I vantaggi sono molti. Primo fra tutti è quello di tenere sempre il sistema pulito con un parco software ordinato nel formato _proprietario_ (*.txz) gestibile poi a sua volta da pkgtool e residente in **/var/log/packages** come default di sistema stesso. Ogni user capisce bene infatti che un conto è compilare e installare un pacchetto seguendo i metodi tradizionali che spesso non garantiscono nemmeno all'utente la chiarezza di locazione finale dei file che contribuiscono al programma che si sta installando.

Tutt'altra cosa invece è installare software precedentemente compilato in modo specifico "personale" (che è diverso da generale), _confezionato_, che sarà installato nel formato _proprietario_ della distribuzione in uso o come in questo caso *.txz . In sostanza potrebbe sembrare ciò che anche gli Slackbuild già fanno, con la differenza che gli Slackbuild si portano dietro delle limitazioni di tipo pratico legate giustamente alla competenza dell'utente che nel peggiore dei casi (io non lo farei mai per indole) deve lanciare lo script così come è scritto. **Src2pkg**, invece, lascia ampio margine di sfogo alle richieste di utenti di tutte le **fasce** di competenza, consentendo all'utente poco esperto una installazione generalizzata e a quello esperto una più specifica e dettagliata _**"quasi meticolosa"**_ compilazione. E' proprio questa snellicità di fascia di utenza e competenza che lo rende un ottimo tool di creazione e compilazione di pacchetti.

**Installazione e fase preliminare:**

L'installazione è davvero semplice. Per prima cosa si scarica e si installa il pacchetto per Slackware dal <a href="http://distro.ibiblio.org/pub/linux/distributions/amigolinux/download/src2pkg/" target="_blank">link</a> ufficiale:

<pre>~$ wget <a href="http://distro.ibiblio.org/pub/linux/distributions/amigolinux/download/src2pkg/src2pkg-1.9.9-noarch-3.tgz" target="_blank">src2pkg-1.9.9-noarch-3.tgz</a>
~$ su -
Password:
~# installpkg src2pkg-1.9.9-noarch-3.tgz</pre>

A questo punto bisogna configurare il programma in maniera standard attraverso un semplice comando:

<pre>~# src2pkg --setup</pre>

Il programma si configurerà prendendo dalla configurazione di sistema i flag USE e allocherà i file principali di configurazione nelle directory di riferimento.

**NB**: Il setup è necessario solo se si tratta del primo avvio del tool.

**NB**: La directory **/etc/src2pkg** conterrà il file **src2pkg.conf** . In questo file è possibile specificare e cambiare molti dei settaggi di base del tool che possono in ogni caso essere richiamati e modificati in fase di avvio. Per ora magari è consigliata un occhiata al file per capire meglio di cosa si parla. Magari è opportuno "consigliato" cambiare subito il formato finale dei pacchetti visto che l'ultima release del programma prevede l'utilizzo del precedente formato *.tgz .

Quindi decommentiamo nella sezione PKG_FORMAT come da sempio:

<pre>~# vi /etc/src2pkg/src2pkg.conf
# PKG_FORMAT
# src2pkg can create packages using bzip2 or lzma instead of gzip. These packages
# are not compatible with standard Slackware pkgtools, but can be installed using
# the tukaani pkgtools or other installers which support them. This defaults to
# the normal 'tgz' packages, or can be set to 'tbz' for bzip2 or 'tlz' for lzma
[[ $PKG_FORMAT ]] || PKG_FORMAT="txz"</pre>

Salvati i cambiamenti siamo pronti ad utilizzare il tool.

**Utilizzo standard di src2pkg:**

Di base il programma è talmente facile che potrebbe usarlo anche un bambino che non ha mai compilato. Supponiamo di voler compilare e creare il pacchetto dell'ultima release in sviluppo di fluxbox senza voler abilitare niente che non siano i parametri default:

Scarichiamo il tarball del source nel formato che più ci aggrada:

<pre>~# wget <a href="http://downloads.sourceforge.net/project/fluxbox/fluxbox/1.1.1/fluxbox-1.1.1.tar.gz?use_mirror=ovh" target="_blank">fluxbox-1.1.1.tar.gz</a></pre>

Completato il download non resta che lanciare e lasciare che il tool faccia il resto:

<pre>~# src2pkg fluxbox-1.1.1.tar.gz
</pre>

**NB**: Con immensa sorpresa per i più scettici lo stesso risultato è ottenibile dando direttamente il link del tarball. Sarà infatti **src2pkg** a preoccuparsi di scaricarlo mediante configurazione precedente del gestore di download da utilizzare. Di default è impostato **wget** che può facilmente essere modificato dal file **/etc/src2pkg/src2pkg.conf**

<pre>~# src2pkg http://downloads.sourceforge.net/project/fluxbox/fluxbox/1.1.1/fluxbox-1.1.1.tar.gz?use_mirror=ovh</pre>

L'output sarà come il seguente:

<pre>Found source archive: fluxbox-1.1.1.tar.gz
Deleting old build files - Done
Creating working directories:
 PKG_DIR=/tmp/fluxbox-1.1.1-pkg-1
 SRC_DIR=/tmp/fluxbox-1.1.1-src-1
Unpacking source archive - Done
Correcting source permissions - Done
Checking for patches - None found
Found configure script - Done
Configuring sources using:
 LDFLAGS="-L/lib -L/usr/lib" CFLAGS="-O2 -m32 -march=i486 -mtune=i686" ./configure --prefix=/usr --libdir=/usr/lib
Configuration has been - Successful!
Compiling sources - Using: 'make'
Compiling has been - Successful!
Checking for 'install' rule - Okay
Checking for DESTDIR (or similar) support - Found DESTDIR
Installing using DESTDIR - Using:
 make DESTDIR=/tmp/fluxbox-1.1.1-pkg-1 install
Installation in DESTDIR - Successful
Processing package content:
Correcting package permissions - Done
Stripping ELF binaries - Using: strip -p --strip-unneeded Done
Checking for standard documents - Done
 Notice - Moving man pages installed under usr/share/man to /usr/man
Compressing man pages - Done!
Creating slack-desc - From default text
Searching for links in PKG_DIR - None found
Rechecking package correctness: 

Checking for misplaced dirs - Done
Rechecking package permissions -
 Notice - Found directories with unusual owner, group or permissions:
 0755 root:root install
 0755 root:root usr
 0755 root:root usr/share
 0755 root:root usr/share/fluxbox
 0755 root:root usr/share/fluxbox/styles
 0755 root:root usr/share/fluxbox/styles/zimek_green
...
</pre>

<pre>Notice - These files and/or directories are listed for information only.
 Corrections may not be needed, but you should check them to be sure.
Making installable package - Done
Package Creation - Successful! - Package Location:
/tmp/fluxbox-1.1.1-i486-1.txz

~#</pre>

**NB**: Di default il tool lavora in modalità **quiet**, evitandoci tutta la trafila di stringhe pastate a terminale durante la compilazione e l'output sarà colorato per permettere una più semplice comprensione di ciò che sta avvenendo. Sarà possibile ovviamente per scelta "personale" disattivare i colori e/o disattivare la modalità quiet attraverso il solito file **src2pkg.conf** . Al termine sarà creato il pacchetto **fluxbox-1.1.1-i486-1.txz** che sarà allocato nella directory **/tmp** . Anche in questo caso le opzioni sono modificabili attraverso il conf generale o mediante sintassi manuale che vedremo in specifico.

A livello generale diciamo che il pacchetto sarebbe in ogni caso già pronto per essere installato mediante pkgtool:

<pre>~# installpkg /tmp/fluxbox-1.1.1-i486-1.txz</pre>

**Sintassi ed esempi:**

Per consultare tutte le possibilità di sintassi:

<pre>~# src2pkg --help</pre>

**NB**: Tutti i comandi sono ampiamente commentati. Non credo sia necessario spiegarli singolarmente. In ogni caso faremo qualche esempio pratico di seguito.

Di base, spesso, gli applicativi che si tenta di compilare non finiscono tale processo. Nella maggior parte dei casi questo risultato è dato dalle mancate opzioni in fase di  conpilazione. A tal proposito src2pkg ci permette un uso mirato delle opzioni di compilazione. Tanto per citarne una, possiamo scegliere di compilare un applicativo con l'opzione **-e=** corrispondente a **extra_configs=** . Questa opzione consente di passare parametri aggiuntivi che saranno racchiusi all'interno degli apici come da esempio:

<pre>~# src2pkg -e='--enable-static --disable-tests' tarball-source-nome.tar.bz2</pre>

Potremmo definire il prefisso di installazione dell'applicativo che nel caso precedente era **/usr** mediante il parametro: **-p=** o **-prefix=/directory/...**

Allo stesso modo potremmo ad esempio scegliere di abilitare l'opzione **-A** che genererà uno script contenente tutte le opzioni di compilazione del nostro tarball-source da compilare. Questo script sarà utile perchè la modifica dello stesso in modo manuale ci permetterà in seguito di ricompilare il nostro package con i parametri opportunamente modificati.

Supponiamo di voler compilare come prima fluxbox passandogli una serie di parametri aggiuntivi come da esempio:

<pre>~# src2pkg -A -e='--enable-shape --enable-slit --enable-remember --enable-toolbar --enable-regexp --enable-newwmspec --enable-nls --enable-timed-cache --enable-kde --enable-gnome --enable-xft --enable-xrender --enable-xpm --enable-imlib2 --enable-xmb --enable-randr --enable-xinerama' -p=/usr</pre>

**NB**: In questo caso abiliteremo tutte le opzioni che abbiamo scelto per fluxbox (xinerama,icone,trasparenza,etc...), si genererà uno script build modificabile per una ricompilazione del package successiva volendo e dichiareremo il prefisso di installazione mediante prefix.

Al termine lo script autogenerato contenente i parametri utilizzati sarà presente nella directory corrente sotto il nome di **fluxbox.src2pkg.auto**.Il package creato sarà sempre creato nella directory **/tmp** , in alternativa si potrà fornire manualmente il percorso prescelto mediante sintassi oppure tramite modifica del file **/etc/src2pkg/src2pkg.conf**.

**NB**: Lo script è modificabile manualmente. Opzione utile per il rebuild **-b=2** (ricompilerà il package *.pkg-**2**.txz

Lo script creato sarà simile al seguente:

<pre>#!/bin/bash
## src2pkg script for:     fluxbox
## Auto-generated by src2pkg-1.9.9
## src2pkg - Copyright 2005-2009 Gilbert Ashley &lt;amigo@ibilio.org&gt;

SOURCE_NAME='fluxbox-1.1.1.tar.gz'
NAME='fluxbox'   # Use ALT_NAME to override guessed value
VERSION='1.1.1'   # Use ALT_VERSION to override guessed value
# ARCH=''
BUILD='1'
PRE_FIX='usr'
# Any extra options go here:
EXTRA_CONFIGS="--enable-shape --enable-slit --enable-remember --enable-toolbar --enable-regexp --enable-newwmspec --enable-nls --enable-timed-cache --enable-kde --enable-gnome --enable-xft --enable-xrender --enable-xpm --enable-imlib2 --enable-xmb --enable-randr --enable-xinerama"

# Optional function replaces configure_source, compile_source, fake_install
# To use, uncomment and write/paste CODE between the {} brackets.
# build() { CODE }

# Get the functions and configs
. /usr/libexec/src2pkg/FUNCTIONS ;

# Execute the named packaging steps:
pre_process
find_source
make_dirs
unpack_source
fix_source_perms
configure_source #
compile_source   # If used, the 'build' function replaces these 3
fake_install     #
fix_pkg_perms
strip_bins
create_docs
compress_man_pages
make_description
make_doinst
make_package
post_process

## See the documentation for more help and examples. Below are some of
# the most common Extras and Options for easy cut-and-paste use.
# DOCLIST='' PATCHLIST='' INSTALL_TYPE=''
# CONFIG_COMMAND='' MAKE_COMMAND='' INSTALL_LINE=''
# When editing src2pkg scripts to add custom code, use these variables
# to refer to the current directory, the sources or the package tree:
# $CWD (current directory), $SRC_DIR (sources), $PKG_DIR (package tree)
# Other commonly-used directories include: $DOC_DIR (document directory)
# $MAN_DIR (man-page directory) $DATA_DIR (shared-data directory)</pre>

**NB**: Con uno script simile opportunamente settato oppure autogenerato per l'applicativo scelto da pacchettizzare sarebbe sufficiente un comando come il seguente per ritrovarsi con il package creato senza passare opzioni direttamente nella fase di avvio in shell. In effetti si fa la stessa cosa in due modi differenti. Esempio avendo lo script prima di aver compilato:

<pre>~# src2pkg fluxbox.src2pkg.auto</pre>

**NB**: In questo caso si partirebbe da uno script per passare le opzioni al tool anzichè definirle manualmente.

Esistono tante altre opzioni utili , come ad esempio quella per testare la compilazione senza procedere oltre: **-T** ; oppure l'opzione per installare il package al termine della compilazione con  **-I ;** la lista delle cose che riesce a fare src2pkg è davvero molto lunga. La migliore arma per imparare è sempre la pratica.

Per dubbi e incertezze vi rimando al <a href="http://www.src2pkg.net/manual:01_using_src2pkg" target="_blank">wiki ufficiale</a> e alla sintassi dell'help:

<pre>~# src2pkg --help
</pre>

**Considerazioni finali:**

Spesso si cercano cose di facili utilizzo, ma non sempre queste riescono a soddisfare le aspettative di tutte le fasce di utenza. Ho scritto questo post perchè credo che la strada giusta sia proprio in progetti come questo dove "snello e facile" non corrisponde ad "automatico e magari instabile e mal compilato o magari anche soltanto (non compilato a proprio uso)" . Sono sicuro che molti capiranno ciò che intendo dire. Come questo tanti altri tools sono in sviluppo...io personalmente credo che questo tipo di tools dovrebbero essere implementati in maniera standard in Slackware, magari sarò di parte perchè amo le distro source-based come gentoo, sourcerer e lunar. Fatto stà, che negli anni, dalla mia personale esperienza,  ho sempre tratto vantaggi dal compilare ed ho sempre "invocato al peccato" quando c'erano cose come apt* di mezzo. Detto questo è sempre e solo una questione strettamente personale. Slackware di suo non usa gestione pacchetti quindi una rogna in meno da considerare. Magari un tool del genere non mi dispiacerebbe vederlo sviluppato e implementato dalla comunità Slackware dipendente...vedremo :) .

\# End
