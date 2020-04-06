---
id: 743
title: 'Split e conversione in *.mp3 di file immagine audio *.ape'
date: 2009-06-16T10:46:37+01:00
author: ax
excerpt: |
  I file di immagine *.ape sono sempre più utilizzati in rete per la loro qualità finale e compressione. Sui sistemi  GNU/linux esistono dei tools che ci permettono con semplici comandi di effettuare la conversione dell'immagine primaria e la divisione "split" del file in singole tracce.
layout: post
guid: http://linuax.wordpress.com/?p=743
permalink: /2009/06/16/split-e-conversione-in-mp3-di-file-immagine-audio-ape/
image_size:
  - ""
  - ""
  - ""
categories:
  - Linux
  - Multimedia
  - Programmi
tags:
  - audio
  - Linux
  - slackware
---
Sempre più spesso ultimamente, specie in ambito audio, capita di scontrarsi con immagini di cd-audio il cui formato ***.ape** rappresenta un file **_esempio-cd.ape_** che al  suo interno implementa tutte le tracce del cd. Non esistono "_a mio sapere_" applicativi che implementano funzioni di split e lettura automatica di questi file; in parole povere non è possibile ascoltare i file traccia per traccia contenuti nell'immagine se prima non vengono effettuate su di essa semplici operazioni mirate alla  allo **split** del singolo file in più tracce e alla conversione delle stesse nel formato che più ci aggrada:

***.wav** e/o ***.mp3** etc..

A tal proposito sui sistemi GNU/linux esistono dei tools "testuali e/o grafici" che ci semplificano il lavoro  mediante semplici operazioni da shell.

**NB**: I pacchetti citati di seguito sono tutti presenti in formato standard _"ancora per poco"_ ***.tgz** per "Slackware" in rete, sui vari siti relativi quali: slacky,slackfind,linuxpackages... sarà necessaria una banale ricerca in rete anche per le altre distribuzioni.

**NB**: Prima di procedere accertatevi di avere i tools necessari allo split e conversione di file ***.ape**, che sono i seguenti:

  1. **mac** "libreria audio di conversione di file ***.ape** in formato ***.wav**"
  2. **cuetools** 
  3. **shntool** 
  4. **SoundKonverter**

Come da elenco vediamo in dettaglio un esempio di conversione partendo dall'immagine **cd.ape**

**Passo 1: Conversione dell'immagine \*.ape in formato \*.wav**

<pre>~$ mac cd.ape nome__nuovaimmagine_scelta.wav -d</pre>

**NB**: Sarà effettuata la conversione del file senza dividere le tracce in esso contenute.

**Passo 2: Split delle tracce** 

<pre>~$ cuebreakpoints cd.cue | shnsplit nome_immagine_scelta_nel_passo1.wav</pre>

**NB**: Il file _**cd.cue**_ "sarà presente assieme al file **cd.ape** precedentemente usato per la conversione", tale file è necessario per la suddivisione dell'immagine in più tracce.

Avviato il processo, partirà lo split e quindi la suddivisione in tracce che ci restituirà a fine processo i file numerati come segue:

<pre>split-track1.wav
split-track2.wav
split-track3.wav
etc...</pre>

**Passo 3: Conversione delle tracce splittate nel formato che preferiamo**

A questo punto basterà utilizzare un qualsiesi applicativo di conversione audio. Come citato precedentemente io consiglio **SoundKonverter** perchè oltre ad essere grafico, estremamente intuitivo e facile da utilizzare _"motivo per cui non credo sia necessario scrivere passo passo come convertire i file"_, ha un ottimo livello di conversione e di personalizzazione della qualità finale dei file.

**NB**: Chiaramente il risultato finale sarà file1.mp3 - file2.mp3 etc ... A questo punto bisognerà rinominare i file oppure usare un applicativo di lettura audio che si appoggia ad un database cbbd che riconosca i tag dei file convertiti.