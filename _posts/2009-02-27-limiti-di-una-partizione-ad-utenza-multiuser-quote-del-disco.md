---
id: 401
title: 'Set limit partizione multiuser Quote disc'
date: 2009-02-27T00:12:42+01:00
author: ax
excerpt: |
  Sempre più spesso, nell'amministrazione server, capita di avere l'esigenza di gestire una partizione usata in multiutenza (una sola partizione con numero N utenti) e assicurare che l'utente1 non consumi tutto lo spazio dedicato a N utenti. Lo strumento che si utilizza per l'assegnazione di un limite dello spazio disco a ciascun utente è chiamato comunemente: "Quota disk".
layout: post
guid: http://linuax.wordpress.com/?p=401
permalink: /2009/02/27/limiti-di-una-partizione-ad-utenza-multiuser-quote-del-disco/
categories:
  - How-to
  - Linux
  - terminal
  - Tips
tags:
  - hd
  - How-to
  - Linux
  - quote
  - Tips
---
Sempre più spesso, specie nell'**amministrazione server**, capita di avere l'esigenza di gestire una partizione usata in multiutenza (una sola partizione con **numero N** utenti) e assicurare che l'utente1 non consumi tutto lo spazio dedicato a **N** utenti.

Lo strumento che si utilizza per l'assegnazione di un limite dello spazio disco a ciascun utente è chiamato comunemente: "**Quota** disk". Le quote difatti, consentono il controllo dello spazio disco, assegnando ad un utente e/o ad un gruppo di sistema una determinata quota disco, espressa in byte.

In primo luogo è necessario specificare all'interno del file **/etc/fstab** le partizioni che dovrebbero adottare le quote per _**user**_ e/o _**group**_, e quindi modificare la partizione scelta come nell'esempio:

<pre>~# vi /etc/fstab
/dev/hda4        /home            ext3        defaults,usrquota,grpquota         1   2</pre>

Stabilita la partizione (nell'esempio **hda4=/home**) procediamo con la creazione dei file **aquota.user** e **aquota.group** nella radice del filesystem in questione, nel nostro esempio in **/home** . Esempio:

<pre>~# cd /home
~# touch aquota.user aquota.group</pre>

**NB**: E' importante dare a tali file i permessi in lettura e scrittura **solo** per l'user **root** onde evitare che un normale utente possa accedervi e/o modificare il file.

<pre>~# chmod 600 aquota.*</pre>

**NB**: prima di continuare sarà bene un restart della macchina per rendere effettive le modifiche sul fs della partizione scelta per le quote disk.

**NB**: Al riavvio eseguiremo (se non sia già stato eseguito al boot)  il comando _**quotacheck**_ che si occuperà di esaminare tutti i filesystem con l'opzione _**quota**_ attiva e di costruire le relative tabelle disco verificando le quote attive.

<pre>~# quotacheck -avugm</pre>

Stabilita la partizione sul quale applicare le _**quote**_ disk non resta che assegnarle agli utenti. Di seguito sono elencati vari esempi.

**- Assegnazione di una quota di disco all'utente esempio "pippo" pari a 5000 Byte (5 MB) -  
** 

<pre>~# edquota -u pippo</pre>

**NB**: modificare nell'output i valori (**soft,hard** etc..); la modifica di tali valori avviene di default tramite l'editor di testo **vi**.

<pre>Disk quotas for user pippo (uid 1000):
Filesystem                                 blocks          soft         hard       inodes
/dev/hda4                                      32          5000            0            8          0    0</pre>

In specifico:

**blocks** = 32 (32 KB)

**soft** = limite massimo a disposizione dell'utente, nell'esempio pari a 5 MB

**hard** = "periodo di grazia" entro il quale l'utente può superare il limite imposto (solo se specificato con un altro comando).

**- Impostazione del "periodo di grazia" (hard)  
** 

Come dall'esempio precedente è possibile concedere ad un utente un "_periodo di grazia_"(**hard**) entro il quale potrà superare il limite quota imposto(**soft**) per un dato numero di giorni. Per prima cosa è necessario impostare il tempo generico corrispondente a tale _periodo di grazia_, espresso in "giorni,ore,minuti o secondi":

<pre>~# edquota -t
Grace period before enforcing soft limits for user:
Time units may be: days, hours, minutes, or seconds
Filesystem                                 Block grace period          Inode grace period
/dev/hda4                                      7days                          7days</pre>

**NB**: se impostato il _periodo di grazia_ (7 giorni) si potrà definire nel campo **hard** il massimo spazio occupabile da parte dell'utente durante tale periodo. Esempio:

<pre>~# edquota -u pippo
Filesystem                                 blocks          soft         hard       inodes
/dev/hda4                                      32          5000         7000            8          0    0</pre>

In alternativa si può decidere di assegnare una quota disk unica procedendo per gruppi invece che per utenza.

**- Quota disco per gruppo -** 

<pre>~# edquota -g users
Disk quotas for group users (gid 100):
Filesystem                                 blocks          soft         hard       inodes
/dev/hda4                                      0          1000000           0            0          0    0</pre>

**NB**: Questo modo di procedere non è il massimo, in quanto esso, potrebbe avere lo svantaggio che un utente potrebbe occupare tutto lo spazio a disposizione del gruppo. Nell'esempio la quota disk è pari a **1GB.**

**- Uso dei Templates -** 

Per evitare di effetturare le solite "dispersive" operazioni per tutti gli utenti del nostro server (come fatto per l'utente **pippo**)  sarà più che utile ricorrere ai templates per copiare le medesime impostazioni sul resto dell'**utenza comune**. Esempio:

<pre>~# edquota -up pippo pluto giggino</pre>

**NB**: nell'esempio si applicano le stesse impostazioni attive per l'utente **pippo** ai successivi utenti, ossia: **pluto** e **giggino.**

Potrebbe però essere "più semplicemente" espandibile a tutti gli utenti del sistema. Esempio:

<pre>~# edquota -p pippo `awk -F: '$3 &gt; 499 {print $1}' /etc/passwd`</pre>

**- Comandi utili -  
** 

I comandi disponibili per **l'attivazione-disattivazione e la verifica delle quote disco, spazio utilizzato, spazio disponibile,  periodo di grazia** sono:

<pre>~# quotaon /home</pre>

Per attivare le quote.

<pre>~# quotaoff /home</pre>

Per disattivare le quote.

**NB**:  **/home** è la partizione attivata precedentemente con i dovuti accorgimenti mediante la configurazione di **/etc/fstab** all'attivazione delle quote utente.

**STATS DELLE QUOTE DISK**

<pre>~# repquota -a</pre>

Ci restituirà come output informazioni sulle quote disco (**spazio utilizzato, spazio disponibile e periodo di grazia**) per ciascun utente configurato precedentemente. Esempio:

<pre>*** Report for  user quotas on device /dev/hda4
Block grace  time: 24:00; Inode grace time: 24:00
Block limits                File limits
User          used          soft         hard     grace     used      
-----------------------------------------------------------------------------------
root    --   40000             0            0         4        0          0       0
pippo   --    7000         10000        20000                101          0       0      
pluto   --     990         10000        20000                 12          0       0      
giggino --     990         10000        20000                 12          0       0</pre>

\# End
