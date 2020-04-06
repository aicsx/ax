---
id: 323
title: 'Shutdown/reboot " Bootsplash'
date: 2009-01-21T12:56:20+01:00
author: ax
excerpt: 'Bootsplash: progressbar abilitata al reboot e/o shutdown di sistema su slackware.'
layout: post
guid: http://linuax.wordpress.com/?p=323
permalink: /2009/01/21/shutdownreboot-bootsplash/
categories:
  - Boot
  - bootsplash
  - How-to
  - Linux
  - Slackware
  - Tips
tags:
  - bootsplash
  - How-to
  - Linux
  - reboot
  - script
  - shutdown
  - slackware
---
Visti i post precedenti riguardo a bootsplash, mi è stato fatto notare che manca la parte relativa alla prograssbar in "silent mode" al momento del reboot e/o shutdown del sistema. In pratica una volta avviato il kernel image patchato con bootsplash in "verbose mode" e avviato il sistema resta al login shell. Il problema è che se si prova a riavviare la box il kernel non ricaricara l'initrd image e di conseguenza nemmeno lo script bootsplash e quindi un eventuale progressbar in "silent mode" per il reboot e/o shutdown.

Chiaramente non è un discorso relativo alla patch bootsplash, ma agli script di init che vanno modificati a seconda delle esigenze.

In specifico per ottenere lo stesso effetto dell'avvio e quindi una modalità silent e una verbose con tanto di progressbar è sufficiente modificare:

<pre>/etc/rc.d/rc.0</pre>

in maniera identica alle modifiche fatte su _**rc.S**_ e _**rc.M**_.

In specifico aggiungete all'inizio del file **/etc/rc.d/rc.0** le seguenti stringhe:

<pre>echo silent &gt; /proc/splash
. /etc/rc.d/rc.bootsplash
animate startup
progressbar 1</pre>

**NB**: configuriamo il bootsplash a livello kernel per non visualizzare i messaggi di shutdown.

Come esempio ci ritroveremo un file /etc/rc.d/rc.0 che inizierà con stringhe simili a queste:

<pre>#! /bin/sh
#
# rc.6          This file is executed by init when it goes into runlevel
#               0 (halt) or runlevel 6 (reboot). It kills all processes,
#               unmounts file systems and then either halts or reboots.
#
# Version:      @(#)/etc/rc.d/rc.6      2.47 Sat Jan 13 13:37:26 PST 2001
#
# Author:       Miquel van Smoorenburg &lt;miquels@drinkel.nl.mugnet.org&gt;
# Modified by:  Patrick J. Volkerding, &lt;volkerdi@slackware.com&gt;
#

# Set the path.
PATH=/sbin:/etc:/bin:/usr/bin
## bootsplash
echo silent &gt; /proc/splash
. /etc/rc.d/rc.bootsplash
animate startup
progressbar 1
# If there are SystemV init scripts for this runlevel, run them.
if [ -x /etc/rc.d/rc.sysvinit ]; then
  . /etc/rc.d/rc.sysvinit
fi</pre>

E finirà con stringhe simili a queste:

<pre>progressbar 100
# Now halt (poweroff with APM or ACPI enabled kernels) or reboot.
if [ "$command" = "reboot" ]; then
  echo "Rebooting."
  /sbin/reboot
else
  /sbin/poweroff
fi</pre>

**NB:** Così come per rc.S e rc.M, più stringhe "progressbar XX" aggiungerete e più l'effetto sarà graduale.

\# End