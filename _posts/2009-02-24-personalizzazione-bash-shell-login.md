---
id: 389
title: 'Personalizzazione: bash shell login'
date: 2009-02-24T17:20:08+01:00
author: ax
excerpt: |
  Tip che ha l'intento di illustrare un semplice quanto efficace modo di creare un logo personalizzato e magari implementarlo in un altrettanto "banale" script; così da goderci, la nostra visuale testuale ad ogni shell login e ad ogni cambio di login utente senza modificare tutti i file di profile utente e soprattutto, senza modificare /etc/profile. Quindi mantenendo ordine all'interno di tale file generico di profile e rispettando l'ordine "default" di esecuzione degli script di avvio.
layout: post
guid: http://linuax.wordpress.com/?p=389
permalink: /2009/02/24/personalizzazione-bash-shell-login/
categories:
  - Bash
  - How-to
  - Linux
  - Scripts
  - terminal
  - Tips
tags:
  - bash
  - Linux
  - login
  - shell
  - Tips
---
Tante e tante volte è richiesta la personalizzazione della shell login al solo scopo di godere per un microsecondo della "minimale ed entusiasmante grafica testuale (adoro le cose testuali)" della shell bash di linux.

Come tutti voi saprete, tale personalizzazione avviene attraverso i file di profile utente o i file di profile di massa, quali: /_**etc/profile ; ~/.bashrc ; ~/.bash_profile**_ etc.. e/o anche mediante semplici script bash.

Questo post ha l'intento di illustrare un semplice quanto efficace modo di creare un logo personalizzato e magari implementarlo in un altrettanto "banale" script; così da goderci, la nostra visuale testuale ad ogni shell login e ad ogni cambio di login utente senza modificare tutti i file di profile utente e soprattutto, senza modificare _**/etc/profile**_. Quindi mantenendo **ordine** all'interno di tale file generico di profile e rispettando l'ordine "default" di esecuzione degli script di avvio.

Su Slackware il tutto è semplificato essendo **/_etc/profile.d/_** la directory che contiene tutti gli script.sh di avvio che sono richiamati a loro volta dal file _**/etc/profile:**_

<pre># Append any additional sh scripts found in /etc/profile.d/:
for profile_script in /etc/profile.d/*.sh ; do
  if [ -x $profile_script ]; then
    . $profile_script
  fi
done</pre>

**NB:** In /etc/profile come si nota,vengono richiamati gli script aggiuntivi di login contenuti nella directory /etc/profile.d/

Detto questo, è chiaro che sarà sufficente creare un file _**/etc/profile.d/loginshell.sh**_, dargli i permessi in esecuzione e il risultato sarà visibile ad ogni login user,login root, cambio utente .. etc

Esempio:

<pre>~$ su -
~# vi /etc/profile.d/loginshell.sh</pre>

Ed incollare e/o creare lo script all'interno di esso:

<pre>#!/bin/bash
# Esempio script che visualizza informazioni di sistema al login 

date=`date |awk '{ print $4 }'`
uptime=`uptime | awk '{print $3,$4}' | sed -e 's/,/ /g'`
box=`uname -o`
arch=`arch`
#version=`cat /etc/*version`
kernel=`uname -r`
hostname=`hostname`
users=`users`
# Spazio disponibile: dove hda* è la partizione prescelta
disk_size=`df -h | grep hda1 | awk '{print $2}'`
disk_used=`df -h | grep hda1 | awk '{print $3, $5}'`
disk_avail=`df -h | grep hda1 | awk '{print $4}'`
disk_mount=`df -h | grep hda1 | awk '{print $6}'`
disk_size1=`df -h | grep hda2 | awk '{print $2}'`
disk_used1=`df -h | grep hda2 | awk '{print $3, $5}'`
disk_avail1=`df -h | grep hda2 | awk '{print $4}'`
disk_mount1=`df -h | grep hda2 | awk '{print $6}'`
# Info
echo -e ""
echo -e ""
echo -e " 33[1;37m                              LINUX BOX   "
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;34m  *33[1;32m Admin:33[1;37m pippo"
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;34m  *33[1;32m Machine:33[1;37m $box"
echo -e " 33[1;34m  *33[1;32m Kernel:33[1;37m $kernel"
echo -e " 33[1;34m  *33[1;32m Architetture:33[1;37m $arch"
echo -e " 33[1;34m  *33[1;32m Hostname:33[1;37m $hostname"
echo -e " 33[1;34m  *33[1;32m User Logged:33[1;37m $users "
echo -e " 33[1;34m  *33[1;32m Disk: 33[1;30mAvail: 33[1;37m$disk_avail 33[1;30mUsed: 33[1;37m$disk_used 3$
echo -e " 33[1;34m  *33[1;32m Disk: 33[1;30mAvail: 33[1;37m$disk_avail1 33[1;30mUsed: 33[1;37m$disk_used1 $
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;34m  *33[1;32m Login Time:33[1;37m $date "
echo -e " 33[1;34m  *33[1;32m Uptime:33[1;37m $uptime "
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;00m"</pre>

Salvato un esempio di script provvederemo a dargli i permessi in esecuzione:

<pre>~# chmod +x /etc/profile.d/loginshell.sh</pre>

E se invece volessimo disegnare in ascii? ... Niente di più semplice!

Basta infatti convertire in ascii un testo semplice ed inserirlo nello script. Per farlo possiamo usufruire via web di un convertitore di **<a href="http://www.kammerl.de/ascii/AsciiSignature.php" target="_blank">Text in ASCII</a>** oppure utilizzare un applicativo la cui installazione prevede la solita compilazione e che fa bene il suo "piccolo" compito ossia:  <a href="http://www.figlet.org/" target="_blank"><strong>figlet</strong></a>.

<pre>~$ wget <a href="ftp://ftp.figlet.org/pub/figlet/program/unix/figlet222.tar.gz" target="_blank">figlet222.tar.gz</a>
~$ tar zxvf figlet222.tar.gz
~$ cd figlet222/
~$ make</pre>

Compilato, non resta che creare  il nostro logo per il login e quindi:

<pre>~$ ./figlet linux box
 _ _                    _
| (_)_ __  _   ___  __ | |__   _____  __
| | | '_ | | |  / / | '_  / _  / /
| | | | | | |_| |&gt;  &lt;  | |_) | (_) &gt;  &lt;
|_|_|_| |_|__,_/_/_ |_.__/ ___/_/_</pre>

Se il risultato non dovesse essere di vostro gradimento potete cambiare il font di conversione leggendo nel file README quelli disponibili di default  e/o implementarne dei nuovi dal sito ufficiale dell'applicativo. Esempio:

<pre>~$ cat README

These fonts are the following:

big.flf (also contains Greek)
banner.flf (also contains Cyrillic and Japanese katakana)
block.flf
bubble.flf
digital.flf
ivrit.flf (right-to-left, also contains Hebrew)
lean.flf
mini.flf
script.flf
shadow.flf
slant.flf
small.flf
smscript.flf
smshadow.flf
smslant.dld
standard.flf
term.flf

~$ ./figlet -f mini linux box

|o._       |_  _
||| ||_|&gt;&lt; |_)(_)&gt;&lt;</pre>

Ottenuto il logo con il fonts preferito, non resta che incollarlo nello script di base precedentemente creato che quindi diventerà simile a questo:

<pre>#!/bin/bash
# Esempio script con logo che visualizza informazioni di sistema al login 

date=`date |awk '{ print $4 }'`
uptime=`uptime | awk '{print $3,$4}' | sed -e 's/,/ /g'`
box=`uname -o`
arch=`arch`
#version=`cat /etc/*version`
kernel=`uname -r`
hostname=`hostname`
users=`users`
# Spazio disponibile: dove hda* è la partizione prescelta
disk_size=`df -h | grep hda1 | awk '{print $2}'`
disk_used=`df -h | grep hda1 | awk '{print $3, $5}'`
disk_avail=`df -h | grep hda1 | awk '{print $4}'`
disk_mount=`df -h | grep hda1 | awk '{print $6}'`
disk_size1=`df -h | grep hda2 | awk '{print $2}'`
disk_used1=`df -h | grep hda2 | awk '{print $3, $5}'`
disk_avail1=`df -h | grep hda2 | awk '{print $4}'`
disk_mount1=`df -h | grep hda2 | awk '{print $6}'`
# Info
echo -e ""
echo -e ""
echo -e " 33[1;37m   _ _                    _               "
echo -e " 33[1;37m  | (_)_ __  _   ___  __ | |__   _____  __"
echo -e " 33[1;37m  | | | '_ | | |  / / | '_  / _  / /"
echo -e " 33[1;37m  | | | | | | |_| |&gt;  &lt;  | |_) | (_) &gt;  &lt; "
echo -e " 33[1;37m  |_|_|_| |_|__,_/_/_ |_.__/ ___/_/_"
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;34m  *33[1;32m Admin:33[1;37m pippo"
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;34m  *33[1;32m Machine:33[1;37m $box"
echo -e " 33[1;34m  *33[1;32m Kernel:33[1;37m $kernel"
echo -e " 33[1;34m  *33[1;32m Architetture:33[1;37m $arch"
echo -e " 33[1;34m  *33[1;32m Hostname:33[1;37m $hostname"
echo -e " 33[1;34m  *33[1;32m User Logged:33[1;37m $users "
echo -e " 33[1;34m  *33[1;32m Disk: 33[1;30mAvail: 33[1;37m$disk_avail 33[1;30mUsed: 33[1;37m$disk_used 3$
echo -e " 33[1;34m  *33[1;32m Disk: 33[1;30mAvail: 33[1;37m$disk_avail1 33[1;30mUsed: 33[1;37m$disk_used1 $
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;34m  *33[1;32m Login Time:33[1;37m $date "
echo -e " 33[1;34m  *33[1;32m Uptime:33[1;37m $uptime "
echo -e " 33[1;30m  -----------------------------------------------------------------------"
echo -e " 33[1;00m"</pre>

**NB**: I colori,il logo e lo script in se, sono solo un  esempio. Il tutto è ampiamente personalizzabile seguendo le variabili di bash.

\# End