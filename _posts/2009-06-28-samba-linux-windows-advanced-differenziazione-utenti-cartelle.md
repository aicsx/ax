---
id: 758
title: 'Samba Lin-Win advanced diff utenti e cartelle'
date: 2009-06-28T17:01:21+01:00
author: ax
excerpt: |
  <p>Un esempio di differenziazione attraverso Samba dei criteri di accesso,di condivisione e dei permessi delle cartelle di una minirete domestica tra sistemi operativi differenti: Linux-Windows.</p>
layout: post
guid: http://linuax.wordpress.com/?p=758
permalink: /2009/06/28/samba-linux-windows-advanced-differenziazione-utenti-cartelle/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Linux
  - Reti LAN
  - Samba
tags:
  - samba linux
  - security
  - shell
  - slackware
---
Abbiamo già visto nel **<a href="http://linuax.altervista.org/blog/2009/06/25/samba-linux-windows-simple-condivisione-file-e-cartelle/" target="_blank">post "simple" precedente</a>** come ottenere una configurazione di base  di condivisione di una singola cartella in una minirete **LAN** avente come server il nostro sistema GNU/Linux in condivisione con il Pc Windows. In questo post, invece, vedremo come differenziare le cartelle condivise su di un server ospitante Samba.

Il vecchio config **/etc/samba/smb.conf**, risultava essere:

<pre>[global]
 workgroup = WORKGROUP
 netbios name = SLACKWARE
 server string = SLACKWARE SERVER
 security = USER
 smb passwd file = /etc/samba/smbpasswd
 encrypt passwords = YES
 log file = /var/log/samba/%m.log
 max log size = 100
 log level = 1</pre>

<pre>[public]
 comment = cartella condivisa
 path = /home/utente/cartella-condivisa
 public = YES
 writable = YES</pre>

**NB**: I parametri (per chi non li conosca già) sono spiegati nel **<a href="http://linuax.altervista.org/blog/2009/06/25/samba-linux-windows-simple-condivisione-file-e-cartelle/" target="_blank">post "simple" precedente</a>**.

Questa configurazione di base ci permetteva appunto di dare le direttive fondamentali a Samba partendo dalla condivisione di una cartella singola su di un singolo utente. E se avessimo più utenti sul server Linux? E se volessimo dire a samba quali sono le cartelle relative ad un utente anzichè un altro ? E se volessimo specificare l'accesso e le restrizioni delle stesse ?

Facciamo un esempio creando due utenti di prova **_(__pippo, pluto)_:**

<pre>~# adduser <span style="color: #0000ff;">pippo</span>
~# adduser <span style="color: #339966;">pluto</span></pre>

Addati gli utenti e settate le password di shell login per gli stessi al sistema dobbiamo logicamente dire a samba quali sono gli utenti addati ad accedere alle sue risorse e quindi provvederemo a settare la password per entrambi gli utenti creati,così come avveniva per il primo utente:

<pre>~# smbpasswd -a <span style="color: #0000ff;">pippo</span>
~# smbpasswd -a <span style="color: #339966;">pluto</span></pre>

**NB**: Se si creano gli stessi utenti con relative password anche sul Pc Windows si evita la richiesta di nome utente e password ogni volta che si tenta di accedere alle risorse di un utente nella condivisione. In specifico se l'user <span style="color: #0000ff;">pippo</span> loggato nel pc Windows, cerchera di accedere al server Linux mediante Samba non avrà alcun bisogno di specificare nome utente e password, ed avrà acceso libero alle cartelle ad egli assegnate.

Per prima cosa abilitiamo samba alla visualizzazione di una cartella  accessibile solo agli utenti proprietari.

**NB**: Questa direttiva fa in modo che l'utente <span style="color: #0000ff;">pippo</span> condivida la sua home utente se sul pc Windows ci loggeremo con lo stesso user.

Per questo samba dispone della direttiva: **homes**

<pre>[homes]
 comment = cartella utente
 writable = YES
 browsable = NO
 valid users = %S</pre>

Dove i parametri risultano essere:

**[homes]** indica la configurazione delle directory utente _(/home/utente_loggato)_.  
**comment =** **cartella utente** è solo un commento.  
**writable =** **YES** indica che la cartella è accessibile in scrittura.  
**browsable = NO** indica che la cartella è visibile solo dagli utenti propietari.  
**valid users = %S** indica che la cartella è accesibile solo dagli utenti propietari.Questa variabile permette di settare per ogni singola home utente i permessi giusti evitandone il setting manuale.

Adesso passiamo alla creazione delle cartelle dedite alla condivisione, esempio:

<pre>~# mkdir /home/public
~# mkdir /home/private
~# mkdir /home/<span style="color: #339966;">pluto_private</span></pre>

Diamo i permessi relativi alle cartelle di esempio create seguendo criteri di differenziazione:

<pre>~# chmod 777 /home/public</pre>

**NB**: In questo modo la cartella **public** sarà accessibile in **_lettura,scrittura ed esecuzione_** da tutti gli utenti avente accesso a samba. Mentre sulla cartella **private** non verranno effettuate modifiche.

<pre>~# chown pluto /home/<span style="color: #339966;">pluto_private/</span>
~# chgrp pluto /home/<span style="color: #339966;">pluto_private/</span></pre>

**NB**: Abbiamo deciso che la cartella **pluto_private** sia accessibile in _**lettura,scrittura ed esecuzione**_ solo dall'utente _**"pluto"**_.

Definiti i parametri passiamo le direttive a samba:

<pre>[public]
 comment = cartella pubblica
 path = /home/public
 public = YES
 writable = YES</pre>

Dove i parametri sono:

**[public]** indica il nome con cui sarà visualizzata la cartella nel Workgroup.  
**comment = cartella pubblica** indica un commento.  
**path = /home/public** indica il percorso su Linux della cartella.  
**public = YES** indica che l' accesso alla cartella è libero.  
**writable = YES** indica che la cartella è accessibile in scrittura.

<pre>[private]
 comment = cartella ristretta in scrittura
 path = /home/private
 public = YES
 writable = NO</pre>

Dove i parametri sono:

**[software]** indica il nome con cui sarà visualizzata la cartella nel Workgroup.  
**comment = cartella ristretta in scrittura** indica un commento.  
**path = /home/private** indica il percorso su Linux della cartella.  
**public = YES** indica che l' accesso alla cartella è libero.  
**writable = NO** indica che la cartella non è accessibile in scrittura, ma solo in lettura.

<pre>[pluto]
 comment = cartella pluto
 path = /home/<span style="color: #339966;">pluto_private</span>
 valid users = pluto
 writable = YES</pre>

Dove i parametri sono:

**[pluto]** indica il nome con cui sarà visualizzata la cartella nel Workgroup.  
**comment = cartella pluto** indica un commento.  
**path = /home/<span style="color: #339966;">pluto_private</span>** indica il percorso su Linux della cartella.  
**valid users = pluto** indica che l' utente autorizzato ad accedere alla cartella è l' utente pluto.  
**writable = YES** indica che la cartella è accessibile in scrittura.

**NB**: E' solo un esempio di differenziazione delle cartelle, poteva essere necessario magari, abilitare anche l'utente pippo ad essere proprietario di una cartella etc...Spazio alla fantasia in base alle esigenze.

Definiti i vari parametri il file config di samba **/etc/samba/smb.conf** diventerà ricapitolando il tutto:

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

[homes]
 comment = cartella generica
 writable = YES
 browsable = NO
 valid users = %S

 [public]
 comment = cartella pubblica
 path = /home/public
 public = YES
 writable = YES

 [private]
 comment = cartella ristretta in scrittura
 path = /home/private
 public = YES
 writable = NO

 [pluto]
 comment = cartella pluto
 path = /home/pluto_private
 valid users = <span style="color: #339966;">pluto</span>
 writable = YES</pre>

Cosa abbiamo fatto con queste direttive?

Se proviamo ad accedere da Windows alle risorse di rete "loggandoci con uno degli utenti creati in ugual modo sulla box linux (<span style="color: #0000ff;">pippo</span>,<span style="color: #339966;">pluto</span>)", noteremo che:

  * L'utente <span style="color: #0000ff;"><strong>Pippo</strong></span> (loggato sulla macchina Win) vede le cartelle: **_public, pippo, private e pluto_private_**. Su di esse avrà accesso libero alle prime tre, di cui su cartella _**private**_ solo in lettura. In più gli sarà negato l'accesso su **pluto_private** (garantito solo in caso di immissione di utente e password).

Se usciamo e ci rilogghiamo da Windows con utente <span style="color: #339966;"><strong>pluto</strong></span> noteremo che:

  * La cartelle visibili adesso sono: _**public,<span style="color: #339966;">pluto</span>,private e <span style="color: #339966;">pluto_private</span>**_<span style="color: #000000;">.</span> In specifico la cartella dell'utente <span style="color: #000000;"><strong>pippo</strong></span> di prima è diventata la cartella <span style="color: #339966;"><strong>pluto</strong></span> proprietaria del nuovo utente. I permessi per le altre cartelle restano gli stessi, con la differenza che se cerchiamo di accedere a <span style="color: #339966;"><strong>pluto_private</strong></span>, samba riconoscerà l'utente pluto come proprietario evitandoci di immettere la password, e dandoci su di essa i pieni diritti in scrittura,lettura ed esecuzione.

Definito il nuovo config, avviamo l'utility di test per verificare che tutto sia apposto:

<pre>~# testparm
Load smb config files from /etc/samba/smb.conf
Processing section "[public]"
Processing section "[brother]"
Loaded services file OK.
Server role: ROLE_STANDALONE
Press enter to see a dump of your service definitions</pre>

Quindi restartiamo Samba:

<pre>~# /etc/rc.d/rc.samba restart
Starting Samba:  /usr/sbin/smbd -D
                 /usr/sbin/nmbd -D</pre>

\# End
