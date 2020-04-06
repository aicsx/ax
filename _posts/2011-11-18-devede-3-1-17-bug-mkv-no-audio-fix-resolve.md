---
id: 1709
title: 'Devede 3.1.17 bug: *.mkv no audio on conversion (fix resolve)'
date: 2011-11-18T08:30:32+01:00
author: ax
excerpt: |
  <p>Gli ultimi aggiornamenti di Devede su Arch Linux con tutte le dipendenze relative creano problemi di conversione di sorgenti audio/video *.mkv. I dvd finali non hanno audio e/o è presente solo in una parte del film. Questo post illustra un <em>banale</em> tip per risolvere il problema tramite il downgrade dei pacchetti.</p>
layout: post
guid: http://linuax.altervista.org/blog/?p=1709
permalink: /2011/11/18/devede-3-1-17-bug-mkv-no-audio-on-conversion-fix-resolve/
categories:
  - Arch
  - Multimedia
  - Tips
tags:
  - 3.1.17
  - avi
  - devede
  - divx to dvd
  - mkv
---
Sicuramente chi è da un pò che traffica con Arch Linux avrò avuto rogne con la conversione di divx in dvd. Nello specifico gli ultimi perenni aggiornamenti di **Devede** hanno dato tantissimi problemi di compatibilità con i codecs usati da **mplayer** per i vari formati del player e del codec audio/video.

Se i nostri divx di partenza sono \*.avi non ci sono problemi, idem per i files \*.mpg che non necessitano di riconversione. Il vero problema si verifica quando si tenta di convertire films con sorgente video in formato **matroska (*.mkv)** . Il risultato è un dvd che ha il video perfettamente codificato e l'audio in AC3 mancante, per tutto il film e/o solo in una parte dello stesso.

Per ovviare vi incollo la mia soluzione che consiste  in un downgrade alle release risultante funzionante per tutti i sorgenti audio/video di partenza: avi, mkv, mpg.

**NOTA BENE:** devede per funzionare richiede mencoder e mplayer. Lo stesso mplayer richiede i codec **x264**. Per questo si consiglia di utilizzare le revisioni dell'esempio seguente e di non specificare altre versioni di pacchetto differenti. Se cambiate ad esempio mplayer si tirerà dietro come dipendenza x264 e così via. Se mischiate le versioni mischiandole fra i vari programmi richiesti da devede sicuramente non funzionerà pià nemmeno il video con mplayer probabilmente.

**NOTA:** ci rechiamo nella directory di cache dei pacchetti installati _**/var/cache/pacman/pkg/**_. Processo valido per chi aggiorna periodicamente la distribuzione. Ovviamente l'esempio è per **ARCH** su cui ho riscontrato il problema. Su Slackware la stessa cosa non si verifica perchè le release di pacchetti non sono le stesse e con la stessa frequenza di Arch Linux._ **(effettuate il downgrade dei pacchetti da utente root o tramite sudo se lo utilizzate assieme a pacman)**_.

<pre># cd /var/cache/pacman/pkg/
# pacman -U  <strong>mplayer-33713-1</strong>-x86_64.pkg.tar.xz <strong>mencoder-33713</strong>-1-x86_64.pkg.tar.xz <strong>devede-3.16.9-2</strong>-any.pkg.tar.xz <strong>x264-20101013-1</strong>-x86_64.pkg.tar.xz</pre>

**NOTA:** l'esempio è basato su Arch Linux x86_64 (64bit) _"in grassetto sono indicate le revisioni che al momento non danno problemi di conversione di alcun tipo di file compreso il formato *.mkv"_.

**NOTA BENE:** se per un motivo o per un altro avete cancellato la cache dei pacchetti installati, procurateveli via (google) e installateli tranquillamente con pacman "chiaramente ci importa che siano quelle versioni specificate e non altre" al fine di risolvere il problema.

Una volta effettuato il downgrade dei pacchetti quasi certamente vi ritroverete con un problema di librerie da risolvere relativo al codec x264 che porterà mplayer a non funzionare. Problema che si risolve con un semplice link simbolico alla libreria da usare.

**ERRORE:**

<pre>~$ mplayer

mplayer: error while loading shared libraries: libx264.so.115: cannot open shared object file: no such file or directory</pre>

**Risoluzione** con un nuovo link simbolico alla libreria attuale di x264:

<pre>~# ln -s /usr/lib/libx264.so.119 /usr/lib/libx264.so.115</pre>

Problema risolto!!! Adesso potete convertire con devede tutti i divx che volete  partendo da tutti i formati sorgenti video. Del resto  (come era sempre stato fino a questo momento). Il bug era stato corretto nella release di Devede-git di qualche tempo fà, ma a quanto pare si è ripresentato. Per questo vi consiglio di lasciare almeno per il momento le versioni specificate.

**Si consiglia:**

Io ho aggiunto in blacklist i pacchetti incriminati per evitare che pacman tenti di aggiornarli al prossimo _pacman -Syu_ . Se volete prevenire anche voi la cosa è molto semplice. Basta modificare il conf di pacman in questo modo:

<pre># vi /etc/pacman.conf</pre>

Alla voce **IgnorePkg** aggiungiamo i pacchetti da non aggiornare (decommentiamo nel caso la stringa) in modo che risulti:

<pre>IgnorePkg   = mplayer mencoder x264 devede</pre>

\# End