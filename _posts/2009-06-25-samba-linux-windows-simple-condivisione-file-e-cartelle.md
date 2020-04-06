---
id: 746
title: 'Samba Linux - Windows "simple": condivisione file e cartelle'
date: 2009-06-25T16:19:03+01:00
author: ax
excerpt: Un esempio semplice di configurazione base di Samba, tool che implementa i demoni necessari per lo scambio e la condivisione di file e cartelle tra PC collegati in rete, aventi magari sistemi operativi differenti tra loro.
layout: post
guid: http://linuax.wordpress.com/?p=746
permalink: /2009/06/25/samba-linux-windows-simple-condivisione-file-e-cartelle/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Linux
  - Reti LAN
  - Samba
  - Slackware
tags:
  - How-to
  - Linux
  - samba linux
---
Le operazioni di condivisione all' interno di una rete Windows sono gestite da un protocollo chiamato **SMB** _Short Message Block_. Linux dispone di un' ottima implementazione **SMB** all' interno della suite <a href="http://www.samba.org" target="_blank"><strong>Samba</strong></a>.

Samba offre le seguenti caratteristiche:  
 ****

  1. Condivisione file e stampanti in rete
  2. Supporto gruppo di lavoro (Workgroup)
  3. Supporto controller di dominio (Primary Domain Controller)
  4. Supporto come membro di Active Directory
  5. Supporto autenticazione in dominio Windows
  6. Supporto Ldap
  7. Supporto Wins
  8. Gestione Master Browser List
  9. Autenticazione Kerberos

**NB**: Installiamo l'ultima versione di  Samba se non fosse presente. Su Slackware distro di riferimento del post, dovrebbe essere già presente ad installazione default. In ogni caso, o si scelgono i pacchetti precompilati oppure agite direttamente da source.

Samba è composto da due demoni **nmbd** e **smbd**: il primo gestisce la risoluzione dei nomi, è responsabile della gestione della _Master Browser List_ e del servizio _Wins_; **smbd** gestisce le operazioni di condivisione e le connessioni.  
Il file di configurazione di Samba si trova nella directory _**/etc/samba/**_ e si chiama **smb.conf** ed è il file che necessita di essere editato per creare le condivisioni con macchine Windows.

**NB**: Installato Samba procederemo con una configurazione _"semplice"_ del file_ **/etc/samba/smb.conf**_ partendo da 0, per abilitare le funzioni di base alla condivisione. Tutte le operazioni saranno effettuate da utente root.

Come da esempio analizziamo le seguenti voci pastate e/o create nel config:

<pre>~$ su
Password:

~# vi /etc/samba/smb.conf

[global]
 workgroup = WORKGROUP
 netbios name = SLACKWARE
 server string = SLACKWARE SERVER
 security = USER
 smb passwd file = /etc/samba/smbpasswd
 encrypt passwords = YES
 log file = /var/log/samba/%m.log
 max log size = 100
 log level = 1</pre>

**[global]** è obbligatorio ed indica la configurazione globale.  
**workgroup** **= WORKGROUP** indica di quale gruppo di lavoro fa parte la macchina Linux. Se cambiato, va effettuata la stessa modifica sulla macchina avente Windows, questo perchè la rete richiede che tutte le macchine abbiano lo stesso workgroup.  
**netbios name = SLACKWARE** (il nome della mia macchina) indica il nome macchina nel gruppo di lavoro.  
**server string = SLACKWARE SERVER** indica la descrizione della macchina.  
**security = USER** indica il livello di sicurezza delle condivisioni. E' modificabile in SHARE per creare condivisioni libere: "non consigliato".  
**smb passwd file = /etc/samba/smbpasswd** indica il file in cui vengono salvate le password di Samba.  
**encrypt passwords = YES** indica la criptazione delle password.  
**log file = /var/log/samba/%m.log** indica la directory in cui verranno salvati i log di Samba. Il parametro %m.log definisce per ogni macchina il log: nomemacchina.log automaticamente.  
**max log size = 100** indica la dimensione max del file di log, superata la quale verrà creato un nuovo file.  
**log level = 1** indica il livello di dettaglio dei log. Va da 1 a 3 ma esagerando con il livello si rischia di appesantire il sistema in maniera notevole.

Per ora salviamo il file **smb.conf** allo stato attuale e procediamo con la creazione della cartella e dei giusti permessi per condividere i file con Windows:

<pre>~$ mkdir /home/utente/cartella-condivisa
~$ su
Password:

~# chmod 777 /home/utente/cartella-condivisa</pre>

**NB**: La cartella sarà accessibile in lettura, scritura ed esecuzione da chiunque sia autorizzato ad accedere a Samba.

Successivamente setteremo la password di Samba per l'utente scelto. "Non necessariamente deve essere uguale alla password dell'utente al login shell":

<pre>~# smbpasswd -a utente
Password:</pre>

Creata la cartella da condividere in rete e settata la password utente per Samba "che sarà assieme al nome utente richiesta da Windows per accedere alla rete" modificheremo nuovamente smb.conf dicendogli quale cartella utilizzare, quindi aggiungeremo a quello che c'era precedentemente le stringhe:

<pre>~# vi /etc/samba/smb.conf

[public]
comment = cartella condivisa
path = /home/utente/cartella-condivisa
public = YES
writable = YES</pre>

**[public]** indica il nome con cui sarà visualizzata la cartella nel workgroup.  
**comment = cartella condivisa** indica un semplice commento.  
**path = /home/a/cartella-condivisa** indica il percorso su Linux della cartella.  
**public = YES** indica che l' accesso alla cartella è libero.  
**writable = YES** indica che la cartella è accessibile in scrittura.

Quindi ricapitolando il file **/etc/samba/smb.conf** risulterà come il seguente:

<pre>[global]
 workgroup = WORKGROUP
 netbios name = SLACKWARE
 server string = SLACKWARE SERVER
 security = USER
 smb passwd file = /etc/samba/smbpasswd
 encrypt passwords = YES
 log file = /var/log/samba/%m.log
 max log size = 100
 log level = 1
</pre>

<pre>[public]
 comment = cartella condivisa
 path = /home/utente/cartella-condivisa
 public = YES
 writable = YES</pre>

Salvate le opzioni generali, procederemo alla verifica del file config e all'avvio di Samba con i relativi demoni:

<pre>~# testparm
Load smb config files from /etc/samba/smb.conf
Processing section "[public]"
Processing section "[brother]"
Loaded services file OK.
Server role: ROLE_STANDALONE
Press enter to see a dump of your service definitions</pre>

**NB**: Stabilita la validità del config procediamo con l'avvio dei servizzi di Samba, chmoddando per il successivo riavvio.

<pre>~# chmod +x /etc/rc.d/rc.samba
~# /etc/rc.d/rc.samba start

Starting Samba:  /usr/sbin/smbd -D
                 /usr/sbin/nmbd -D</pre>

Conclusa la configurazione di Samba lato Server, procederemo a configurare la macchina Windows per accedere alla rete "cosa che non verrà spiegata in questo post data l'estrema facilità della stessa".

**NB**: E' solo un esempio di configurazione semplice per creare una cartella condivisa tra la box slackware visibile al pc Windows di riferimento. In altri post saranno trattati altri tipi di configurazione.

**NB**: Creata la rete anche su Windows supponiamo di creare la cartella **C:condivisa** e di dargli i permessi di condivisione a Everyone.

Per accedere dalla box **Slackware** alla cartella condivisa sul Pc **Windows** sarà sufficiente creare il mount point e succissivamente montare la cartella come da esempio:

Quindi su Slackware

<pre>~# mkdir /mnt/rete_win</pre>

Che crea la directory scelta per il mount point.

<pre>~# smbmount //NomeMacchinaWin/condivisa /mnt/rete_win -o username=utentewin</pre>

Che provvederà a montare la cartella condivisa di windows nel mount point scelto.

\# End