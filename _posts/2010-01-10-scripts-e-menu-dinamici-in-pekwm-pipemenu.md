---
id: 975
title: 'Scripts e menu dinamici in PekWM: pipemenus.'
date: 2010-01-10T07:00:20+01:00
author: ax
excerpt: Esempio generale di configurazione e abilitazione di menu dinamici (pipe menus) attraverso il richiamo di scripts in PekWM.
layout: post
guid: http://linuax.wordpress.com/?p=975
permalink: /2010/01/10/scripts-e-menu-dinamici-in-pekwm-pipemenu/
categories:
  - How-to
  - Linux
  - menu
  - PekWM
  - Scripts
  - Tips
tags:
  - Linux
  - pekwm
  - pipemenus
  - script bash
  - wm
---
Un pò di tempo fà, nel post che trattava la configurazione generale di **OpenBox** si è accennato all'utilizzo di script (bash,perl,python...etc) che potessero essere implementati nel menu desktop _"statico"_ del **wm**. Anche in **PekWM** questa funzione è supportata attraverso ciò che in gergo tecnico viene definito <a href="http://icculus.org/openbox/index.php/Openbox:Pipemenus" target="_blank"><strong>pipemenu</strong></a>. I **pipemenus** infatti sono menu _"dinamici"_ (contenenti categorie,sottocategorie,link,funzioni,etc) che fanno capo al richiamo di semplici e/o complessi scripts.

Un esempio generico che sembra essere molto richiesto, può essere quello della visualizzazione della data e dell'ora nel menu di **PekWM**.

**NB**: per prima cosa ci procuriamo e/o scriviamo con il nostro editor preferito lo script necessario. Nel nostro esempio **pkw**-**dateclock.sh** (bash).

<pre> ~$ vi pkw-dateclock.sh
#!/bin/bash
# pkw-dateclock.sh
date=$(date +%A %d %b %H:%M)
echo "Dynamic {"
echo " Entry = "$date"    { Actions = "Exec xclock & " }"
echo "}"
exit
</pre>

**NB**: lo script stampa ad output di menu dinamico ciò che ci interessa (data e ora) ed apre l'utility xclock al click singolo del mouse.

Salvato lo script, è buona norma mantenere ordine e quindi spostiamo lo script creato in una sottodirectory **/scripts** creata nella directory di riferimento del wm (nel nostro caso **PekWM** la directory di riferimento resta **~/.pekwm/** ) . Ovviamente essendo uno script eseguibile provvederemo a dargli i permessi di esecuzione. Esempio:

<pre>~$ mkdir ~/.pekwm/scripts
~$ cp pkw-dateclock.sh ~/.pekwm/scripts/
~$ su -
~# chmod +x ~/.pekwm/scripts/pkw-dateclock.sh</pre>

Finito il lato script, non resta che richiamare tale funzione modificando il menu statico di **PekWM** ed aggiungendo l'Entry necessaria come da esempio:

<pre>~$ vi ~/.pekwm/menu
Submenu = "Data/ora" { Icon = "dateclock.png" ;
        Entry = "" { Actions = "Dynamic  /home/utente/.pekwm/scripts/pwk-dateclock.sh"}
 }</pre>

**NB**: la modifica del menu statico di **PekWM** segue i soliti parametri. Se ci fossero dubbi in merito consultare il post: <a href="http://linuax.wordpress.com/2009/12/25/un-altro-cugino-della-famiglia-box-wm-pekwm/" target="_blank"><strong>Un altro cugino della famiglia *box: PekWM</strong></a> - sezione **MENU**.

Il risultato finale sarà come da screen:

[<img class="alignnone size-full wp-image-976" title="data_e_ora" src="http://linuax.files.wordpress.com/2010/01/data_e_ora.png" alt="" width="300" height="304" />](http://linuax.files.wordpress.com/2010/01/data_e_ora.png)

Allo stesso modo, si possono eseguire scripts in **perl** che richiamino statistiche di sistema, come il consumo della memoria RAM, percentuali, come da esempio:

<pre>~$ vi ~/.pekwm/scripts/stat.pl
#!/usr/bin/perl

print "Dynamic {n";
($parta,$partb) = split(/Mem:/,`free -m`);
($partc,$partd) = split(/^           /,$partb);
($partf,$partg) = split(/        /,$partd);
$partg =~ s/ //g;
print "Submenu = "Memory" {n";
 print "Entry = "$partg/$partf mb" { Actions = "Exec " }n";
print "}n";    

($pa,$pb) = split(/COMMANDn/,`ps -e -o pcpu,cpu,nice,state,cputime,args --sort pcpu | sed '/^ 0.0 /d'`);
(@array) = split(/n/,$pb);

print "Submenu = "CPU Usage" {n";

foreach $temp (@array) {
 $temp =~ s/^ //;
 ($percent,$process) = split(/ .{19}/,$temp);
 if ($process =~ ///) {
 ($pops,$use) = split(//(?!.*/)/,$process);
 } else {
 $use = $process;
 }    
 print "Entry = "$use:  $percent" . "%" { Actions = "Exec " }n";

}

print "}n";    
print "}n";</pre>

Modifichiamo il menu statico di **PekWM** aggiungendo anche in questo caso la solita Entry relativa****:

<pre>SubMenu = "Stat" { Icon = "boxstats.png" ;
        Entry = "" { Actions = "Dynamic /home/user/.pekwm/scripts/stat.pl" }
 }</pre>

Il risultato sarà:

[<img class="alignnone size-full wp-image-977" title="memoria_pekwm" src="http://linuax.files.wordpress.com/2010/01/memoria_pekwm.png" alt="" width="330" height="300" />](http://linuax.files.wordpress.com/2010/01/memoria_pekwm.png)

[<img class="alignnone size-full wp-image-978" title="percentuale_pekwm_mem" src="http://linuax.files.wordpress.com/2010/01/percentuale_pekwm_mem.png" alt="" width="373" height="351" />](http://linuax.files.wordpress.com/2010/01/percentuale_pekwm_mem.png)

\# End