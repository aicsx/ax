---
id: 317
title: 'Bootsplash advanced: progress bar enable "silent mode"'
date: 2009-01-20T22:22:15+01:00
author: ax
excerpt: |
  How-to che ha lo scopo di potenziare le funzioni del bootsplash classico su slackware in "silent mode" con l'avvio della progress bar graduale.
layout: post
guid: http://linuax.wordpress.com/?p=317
permalink: /2009/01/20/bootsplash-progress-bar-enable-silent-mode/
categories:
  - Boot
  - bootsplash
  - How-to
  - Linux
  - Slackware
  - Tips
tags:
  - bar
  - bootsplash
  - How-to
  - Linux
  - script
  - slackware
---
Questo how-to ha lo scopo di potenziare le funzioni del bootsplash classico su slackware  in "silent mode".

Qualche user curioso avrà per esempio sentito l'esigenza di provare il bootsplash  modificando lilo col parametro:

<pre>append = "splash=silent" # vedi vecchio post: <a href="http://linuax.wordpress.com/2009/01/20/bootsplash-grafico-su-slackware-gnulinux/" target="_blank">Bootsplash grafico su slackware GNU/linux</a></pre>

e si sarà reso conto che di default su slackware la patch kernel del bootsplash ci permette di avere la grafica in sfondo shell in modalità verbose ma al contrario in modalità silent impedisce di visualizzare la barra di avanzamento. Per ovviare a questa situazione quello che dovremmo fare non è altro che modificare 2 file che vengono caricati durante l’avvio:

<pre><strong>*rc.S</strong>
<strong>*rc.M</strong></pre>

e aggiungere uno script che caricherà la progress bar in modalità silent in maniera graduale: **rc.bootsplash**.

#

La prima cosa da fare è appunto salvare lo script bootsplash che dirà al boot kernel di caricare la progress bar in caso di "silent mode", seguendo questi semplici passi:

<pre>~# cd /etc/rc.d/
~# vi rc.bootsplash</pre>

all'interno del file rc.bootsplash incolleremo le seguenti stringhe bash:

<pre>#! /bin/sh
#
function progressbar()
{
    if [ $# != 1 ]
        then
            echo "Usage: progressbar {progress}"
        exit 1
           fi
    echo "show $(( 65534 * $1 / 100 ))" &gt; /proc/splash
}
#
function animate()
{
    if [ $# = 0 ]
        then
                   echo "Usage: animate {hook}"
        exit 1
           fi
    splash "$*"
}</pre>

Salvato lo script gli daremo i permessi di esecuzione all'avvio:

<pre>~# chmod ug+x rc.bootsplash</pre>

La seconda cosa da fare è appunto la modifica dei file **rc.S** e **rc.M** presenti in _**/etc/rc.d/**_ e che si occupano del boot dei servizzi e di tutti gli script init sulla nostra box  slackware. Prima di effettuare modifiche però per evitarci sgradevoli sorprese, è preferibile sempre un backup dei file originari:

<pre>~# cd /etc/rc.d/
~# cp rc.S rc.S_backup
~# cp rc.M rc.M_backup</pre>

Completato il backup dei file originari, siamo pronti per applicare modifiche su entrambi i file.

**NB:** La percentuale del caricamento non viene comunicata in automatico dal bootsplash, ma bensì dobbiamo essere noi ad aggiungere nello script il comando per indicare a che punto si trova lo stato di caricamento(10…20..30% e così via). Apriamo quindi il primo file **rc.S**, con un editor a nostro piacimento (grafico o testuale) ricordandoci che occorre avere privilegi da root per modificare tale file.

Aggiungeremo come prima stringa la stringa

<pre>. /etc/rc.d/rc.bootsplash
animate startup</pre>

Ora, aggiungeremo all'interno del file le percentuali che vogliamo corrispondere ad ogni passaggio, inserendo la seguente riga:

**progressbar XX** 

Dove **XX** rappresenta un numero compreso tra 1 e 100, essendo rc.S il primo dei due files di caricamento si consiglia di indicare come ultimo passaggio un valore che sia intorno al 50%.

Esempio:

inizio rc.S file:

<pre>#!/bin/sh
#
# /etc/rc.d/rc.S:  System initialization script.
#
# Mostly written by:  Patrick J. Volkerding, &lt;volkerdi@slackware.com&gt;
#

PATH=/sbin:/usr/sbin:/bin:/usr/bin

. /etc/rc.d/rc.bootsplash
animate startup

progressbar 1
# Mount /proc right away:
/sbin/mount -v proc /proc -n -t proc</pre>

Chiaramente stando a ciò che abbiamo detto prima il file **rc.S** finirà con una cosa tipo:

<pre>progressbar 47
# Carry an entropy pool between reboots to improve randomness.
if [ -f /etc/random-seed ]; then
  echo "Using /etc/random-seed to initialize /dev/urandom."
  cat /etc/random-seed &gt; /dev/urandom
fi
# Use the pool size from /proc, or 512 bytes:
if [ -r /proc/sys/kernel/random/poolsize ]; then
  dd if=/dev/urandom of=/etc/random-seed count=1 bs=$(cat /proc/sys/kernel/random/pool$
else
  dd if=/dev/urandom of=/etc/random-seed count=1 bs=512 2&gt; /dev/null
fi
chmod 600 /etc/random-seed
progressbar 50</pre>

Completata la configurazione di _rc.S_, non resta che modificare il secondo file di avvio boot ossia: **rc.M**.

Analogamente a quanto abbiamo fatto con il file _rc.S_ inseriremo come prima riga i seguenti due comandi:

<pre>. /etc/rc.d/rc.bootsplash
animate startup</pre>

Anche questa volta inseriremo all'interno del file i valori di progressbar (ricominciando dall'ultimo valore presente nel file rc.S "nel nostro esempio 50% sino ad arrivare all'ultimo valore pari a 100).

Come per il precedente rc.S un esempio di configurazione sarà ad inizio file **rc.M**:

<pre>#!/bin/sh
#
# rc.M          This file is executed by init(8) when the system is being
#               initialized for one of the "multi user" run levels (i.e.
#               levels 1 through 6).  It usually does mounting of file
#               systems et al.
#
# Version:      @(#)/etc/rc.d/rc.M      2.23    Wed Feb 26 19:20:58 PST 2003
#
# Author:       Fred N. van Kempen, &lt;waltje@uwalt.nl.mugnet.org&gt;
#               Heavily modified by Patrick Volkerding &lt;volkerdi@slackware.com&gt;
#

# Tell the viewers what's going to happen.
echo "Going multiuser..."

. /etc/rc.d/rc.bootsplash
animate startup

progressbar 51
# Update all the shared library links:
if [ -x /sbin/ldconfig ]; then
 echo "Updating shared library links:  /sbin/ldconfig &"
/sbin/ldconfig &
fi
progressbar 52
etc.....</pre>

E finirà con stringhe simili a queste:

<pre>progressbar 99
# Start the GPM mouse server:
if [ -x /etc/rc.d/rc.gpm ]; then  
  . /etc/rc.d/rc.gpm start  
fi

# If there are SystemV init scripts for this runlevel, run them.
if [ -x /etc/rc.d/rc.sysvinit ]; then
  . /etc/rc.d/rc.sysvinit  
fi

# Start the local setup procedure.
if [ -x /etc/rc.d/rc.local ]; then  
  . /etc/rc.d/rc.local
fi
progressbar 100
# All done.</pre>

**NB**: più righe di _progressbar XX_ inserire nei vostri files **_rc.S_** e **_rc.M_** più l'andamento della barra risulterà graduale e fluido e l'effetto ottico risulterà di sicuro più piacevole.

**NB**: l'inserimento di righe _progressbar_ XX è decisamente legato al gusto soggettivo, non c'è una particolare regola nella modifica e quindi nel setting della progress bar.

Effettuate le modifiche dei file e inserito lo script necessario all'esecuzione della progress bar non resterà che modificare la configurazione in lilo e quindi per rifarci al precedente how-to su bootsplash modificare la stringa nel conf di lilo come segue:

<pre># Kernel image Bootsplash start
image = /boot/image2624_splash
initrd = /boot/initrd.tattoo
append = "splash=silent"   # stringa modificata
root = "/dev/hdaX"
label = "SlackSplash"
vga = 791
read-only
# Kernel image Bootsplash end</pre>

Salvate le modifiche a lilo, non resta che il restart di lilo per rendere permanenti le modifiche:

<pre>~# lilo -v</pre>

\# End