---
id: 373
title: Sudo su GNU/Linux Slackware
date: 2009-02-23T20:35:22+01:00
author: ax
excerpt: |
  Sudo  è un programma per i sistemi operativi Unix e Unix-like che, con dei vincoli, permette di eseguire altri programmi assumendo l'identità "e di conseguenza anche i privilegi" di altri utenti. Può essere la soluzione ideale nei casi in cui si intende sostituire l'user root senza trascurare la sicurezza di base di un sistema multiutente.
layout: post
guid: http://linuax.wordpress.com/?p=373
permalink: /2009/02/23/sudo-su-gnulinux-slackware/
categories:
  - How-to
  - Linux
  - Slackware
  - Tips
tags:
  - How-to
  - Linux
  - slackware
  - sudo
---
Uno degli aspetti che rende gradevole la nostra cara amata **Slackware** è sempre stato quello di essere da principio installazione: "default", una distro senza troppi fronzoli, che dal primo avvio **non** contiene particolari personalizzazioni sul sistema di base. **Non** ci sono personalizzazioni di login shell "testuale o grafico", **non** ci sono script oltre quelli di init e rc.applicativi di avvio etc...

Resta quindi scelta dell'utente stabilire **se e cosa** modificare in base alle proprie esigenze pratiche o anche estetiche, "uso casalingo" e/o "uso server" o semplicemente smanettamento.

In questo post vedremo come configurare ed usare _**sudo**_ che può contribuire alla personalizzazione dell'usabilità della nostra distro preferita.

  * **Utilizzo e praticità; sudo**:

**Sudo** è un programma per i sistemi operativi Unix e Unix-like che, con dei vincoli, permette di eseguire altri programmi assumendo l'identità "e di conseguenza anche i privilegi" di altri utenti.

I vincoli sono espressi nel file di configurazione <tt>/etc/sudoers</tt>, che normalmente è modificabile solo dall'utente root: in esso sono definiti gli utenti che possono eseguire comandi tramite _sudo_, le identità che possono assumere ed i comandi che possono eseguire con eventuali vincoli sui parametri, con o senza richiesta di autenticazione.

**NB:** Alcuni sistemi come Mac Os X e Ubuntu installano _sudo_ di serie, mentre negli altri va eventualmente installato in un secondo tempo. Quindi se non è presente installatelo.

Un esempio banale per capire la grandezza di sudo può essere una normale operazione di **_reboot_** e/o _**shutdown**_ del sistema, che richiede chiaramente i privilegi di root.

**NB**: tali privilegi richiederebbero che ogni volta bisognerebbe ripetere le stesse operazioni e cioè:

login come root e passaggio del parametro. Per ovviare a tale e dispersiva operazione ci sono due modi:

  1. Dare ai binari e/o link simbolici i permessi user.
  2. Utilizzare magari _**sudo**_ per aggirare la cosa.

Nel primo caso sarà sufficente un:

<pre>~# chmod +4755 /sbin/reboot
~# chmod +4755 /sbin/shutdown</pre>

In questo modo da utente basterà dare il comando:

<pre>~$ /sbin/reboot</pre>

per riavviare il sistema senza loggarsi come root.

**NB**: Questo non è il migliore dei sistemi perchè ovviamente, permette ad un normale utente di riavviare la box in qualsiasi momento senza particolari regole restrittive. Per ovviare magari ad un "poco" divertente scherzo di un amico al quale abbiamo dato login user nella nostra box, capite bene, che è decisamente più conveniente utilizzare il secondo metodo e cioè ricorrere a _**sudo**_.

Su Slackware dovrebbe essere già presente, in caso contrario potete installarlo <a href="http://packages.slackware.it/" target="_blank">scaricando</a> il pacchetto precompilato dal link per la versione slackware in uso, oppure compilando il source dal <a href="http://www.sudo.ws/sudo/download.html" target="_blank">link ufficiale</a>.

Installato sudo, per aggiungere un utente ai sudo user (sudoer) basta usare il comando visudo.

<pre>~# visudo</pre>

Per dare all'utente pieni privilegi usando "sudo" prima di un comando, aggiungere la riga seguente:

<pre>USER_NAME   ALL=(ALL) ALL</pre>

dove USER_NAME è il nome dell'utente prescelto. Esempio:

<pre>pippo       ALL=(ALL) ALL</pre>

**NB**: Una volta abilitato l'utente _pippo_ è però necessario specificare che _pippo_ deve essere prensente nel gruppo _**wheel.**_ Quindi dopo aver verificato, assegnamo all'user pippo l'appartenenza al gruppo che permetterà l'esecuzione di sudo/su da parte di tale utente.

<pre>~$ id
uid=1000(pippo) gid=100(users) gruppi=11(floppy),17(audio),18(video),19(cdrom),100(users)
~$ su -
~# usermod -G wheel pippo</pre>

Abilitato l'utente pippo all'esecuzione di sudo a pieni permessi non resta che lanciare il reboot del sistema, da user:

<pre>~$ sudo /sbin/reboot
Password:</pre>

**NB**: Inserire la password dell'utente pippo ed il sistema sarà rebootato con gli stessi privilegi dell'utente root.

**NB**: In alcune **distro** l'utilizzo di sudo può essere utile per sostituire magari i privilegi in scrittura su periferiche ottiche e di masterizzazione ad utenti appartenenti al gruppo _**optical**_.

Per i "cugini" pigri, può essere ancora meno dispersivo l'uso di un alias che limiti i caratteri di comando da digitare. In tal caso basta modificare il file generico _**/etc/profile**_ ed aggiungere gli alias ai comandi:

<pre>alias reboot="sudo /sbin/reboot"
alias shutdown="sudo /sbin/shutdown -h now"</pre>

Tali alias ci permetteranno di arrivare allo stesso risultato in maniera ancora più veloce:

<pre>pippo@Slack:~$ reboot
Password:

REBOOT NOW.</pre>

\# END