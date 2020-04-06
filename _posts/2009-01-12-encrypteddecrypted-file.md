---
id: 242
title: encrypted/decrypted file
date: 2009-01-12T19:09:18+01:00
author: ax
excerpt: 'Per necessità o spesso semplicemente per "paranoia" può essere utile cryptare e/o decryptare file di testo. Esiste su linux un applicativo che ci rende liberi di dormire senza troppi pensieri: MCript.'
layout: post
guid: http://linuax.wordpress.com/?p=242
permalink: /2009/01/12/encrypteddecrypted-file/
categories:
  - Bash
  - Linux
  - Scripts
  - Tips
tags:
  - decrypt
  - encrypt
  - Linux
  - Tips
---
Per necessità o spesso semplicemente per "paranoia" può essere utile cryptare e/o decryptare file di testo. Esiste su linux  un applicativo che ci rende liberi di dormire senza troppi pensieri: MCript.

<a title="Download" href="http://downloads.sourceforge.net/mcrypt/mcrypt-2.6.8.tar.gz?modtime=1227352665&big_mirror=0" target="_blank">http://mcrypt.sourceforge.net/</a>

MCript è una rivisitazione del  vecchio  "crypt" che consente di cryptare senza effettuare cambiamenti drastici al codice stesso oltre a supportare svariate funzioni di crittografia.

Negli esempi ci sono due semplici script per velocizzare le operazioni di encrypt e decrypt di singoli file di testo.

**NB:** Assicuratevi di avere il pacchetto relativo alla distribuzione in uso e di aver risolto le dipendenze: "libmcrypt".

**encrypted.sh:**

<pre>#!/bin/bash
# script to encrypt text file

file=$1
echo -n "### Inserisci il nome completo del file da cryptare: "
read file

if [ ! -f $file ]
then
    echo "$file non è un file di testo!"
    exit 1
fi

echo -n "### Inserisci la password: "
read password

mcrypt -k $password --echo -o mcrypt-md5 $file
echo  "### File cryptato: $file.nc"<strong>
</strong></pre>

**decrypted.sh:**

<pre>#!/bin/bash
# script decrypt text file

file=$1
echo -n "### Inserisci il nome del file da decryptare: "
read file

if [ ! -f $file ]
then
    echo "### $file non è un file di testo!"
    exit 1
fi

echo -n "### Inserisci la password: "
read password

mcrypt -d -k $password --echo -o mcrypt-md5 $file
echo  "### Decrypt $file : ok"</pre>

**NB:** Come avrete notato, gli script usano la keymode _mcrypt-md5._ Se non è di vostro gradimento e desiderate cambiarla:

<pre>~$ mcrypt --list-keymodes</pre>

modificate la stringa:

_"mcrypt -d -k $password -echo -o **keyword\_scelta\_da_voi** $file"_

Per tutto il resto vi rimando al comando:

<pre>~$ mcrypt --help</pre>

#