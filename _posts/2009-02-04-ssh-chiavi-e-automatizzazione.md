---
id: 342
title: 'SSH - chiavi e automatizzazione'
date: 2009-02-04T22:28:16+01:00
author: ax
excerpt: Tips sulla pigrizia nella gestione delle connessioni "client/server" e autenticazioni SSH grazie alle chiavi DSA.
layout: post
guid: http://linuax.wordpress.com/?p=342
permalink: /2009/02/04/ssh-chiavi-e-automatizzazione/
categories:
  - How-to
  - Linux
  - terminal
  - Tips
tags:
  - Linux
  - ssh
  - Tips
---
Le chiavi **SSH** sono come indica il nome: delle chiavi. Esse permettono di "_**aprire**_" una sessione **SSH** senza dover inserire la password o in altri casi di velocizzare l'avvio di una **sessione** singola o di multiple sessioni **SSH** evitando di ripetere in maniera forzata gli stessi comandi.

Molto spesso infatti durante la giornata "**tipo**" dell'amministratore medio può capitare di connettersi "**più e più volte**" ad una sessione aperta con demone ssh sul proprio server remoto. Per evitare di digitare ogni volta gli stessi comandi e alle volte "dannatamente" ripetitivi ci sono due strade:

>   * **1° CASO**: **CONNESSIONE AUTOMATICA SENZA IMMISSIONE DI PASSWORD**.

Per prima cosa è necessario  creare la chiave SSH. Lo strumento che ci evita rogne e ci semplifica la vita è: **ssh-keygen**.

<pre>~$ ssh-keygen -t dsa -N ''</pre>

**NB:** Il comando genera una chiave random, che non prevede l'autenticazione via password. Verranno create due chiavi nella directory **~/.ssh/** ossia:

<pre><strong>id_dsa</strong> : che è la chiave client locale "PRIVATE KEY".</pre>

<pre><strong>id_dsa.pub : </strong>che è la chiave che provvederemo a copiare sul server "PUBLIC KEY".</pre>

Esempio:

<pre>~$ scp -P 12345 ~/.ssh/id_dsa.pub user@host:/home/user_remoto_server/.ssh/authorized_keys2</pre>

Oppure è possibile ottenere lo stesso risultato con:

<pre>~$ cat ~/.ssh/id_dsa.pub | ssh example_server.org "cat &gt;&gt; ~/.ssh/authorized_keys"</pre>

Spostata la chiave sul server sarà sufficiente una connessione ssh al server e verremo loggati in **shell** login senza immettere la password.

**NB:** Anche con tale operazione saremo costretti ogni volta a digitare per loggarci una cosa simile a:

<pre>~$ ssh user@host.it -P porta</pre>

  * Per evitare anche questo passaggio e magari per gli utenti più esigenti è possibile creare un alias a un nome dominio fittizio. Sarà sufficiente modificare e/o  se non c'è creare un file **config** nella directory client **~/.ssh/** e quindi:

<pre>~$ vi ~/.ssh/config</pre>

Dentro di esso incolleremo stringhe simili a queste:

<pre>Host servermio
Hostname 100.100.80.71
User pippo
Port 12345

Host virtuale
Hostname macchina.dominio.it
User esempio
Port 22222</pre>

Creato il conf per connetterci senza inserire password e senza incollare ogni volta "dati utente, porta etc..", basterà dare da shell il comando:

<pre>~$ ssh servermio</pre>

e/o

<pre>~$ ssh virtuale</pre>

Saremo quindi autologgati.

**NB**: Nei casi di gestione privata di un client che _**droppa**_ tutti gli accessi esterni da "**classi ed utenza**" verso l'interno e quindi negli stessi casi in cui si **autoaccerta** di **non avere** servizzi che potrebbero causare e/o compromettere la sicurezza del sistema **client** principale può essere davvero utile l'utilizzo di tali chiavi per gli amanti dell'aministrazione "_**pigra**_". Nel caso contrario invece: avere chiavi **SSH senza password** non è la cosa migliore da fare. Se infatti la chiave viene rubata, chiunque potrà connettersi ai vostri server. Quindi prima di tali operazioni, si spera che vi siate preoccupati della gestione del vostro client.

>   * **2° CASO: CONNESSIONE AUTOMATIZZATA ATTRAVERSO L'IMMISSIONE DI UNA PASSWORD GESTITA DA SSH-AGENT.**

Come nel 1° caso sarà necessario creare una public key, questa volta però **con password** accettando il percorso predefinito della destinazione della key. Esempio:

<pre>~$ ssh-keygen -t dsa
Generating public/private dsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_dsa):</pre>

Premeremo **enter** per accettare il percorso predefinito, e quindi inseriremo la password e la confermeremo:

<pre>Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user/.ssh/id_dsa.
Your public key has been saved in /home/user/.ssh/id_dsa.pub.
The key fingerprint is:
16:53:66:fa:b5:d3:6d:a5:9f:a9:55:73:ef:48:55:0e user@esempio</pre>

Sempre come nel 1° caso, provvederemo a copiare la public key  **id_dsa.pub** sul nostro server sotto il nome di **authorized_keys**. Esempio:

<pre>~$ cat ~/.ssh/id_dsa.pub | ssh example.it "cat &gt;&gt; ~/.ssh/authorized_keys</pre>

Completata la fase di generazione key con password si procede con la configurazione di **SSH-Agent**.

**NB**: SSH-agent opera semplicemente attendendo in background, aspettando quindi che vi connettiate a un server ssh. Quindi, alla prima connessione utilizzando la chiave ssh, vi chiederà la password della chiave. La volta seguente in cui vi connetterete utilizzando quella chiave, otterrete automaticamente l'accesso senza ulteriori richieste di password, grazie a ssh-agent (dovrete in pratica inserire la password una volta per ciascuna sessione di ssh-agent).

Ad ogni modo, dovrete comunque prima avviare ssh-agent. Per utilizzare ssh-agent "standard" dovrete utilizzare un comando simile a questo:

<pre>eval `ssh-agent -s`</pre>

Il comando seguente sarà inserito in un file che si avvia prima del vostro terminale di shell, per es. **~/.xinitrc**. Quindi, una volta eseguito il login, date il comando **ssh-add**: vi verrà chiesto di inserire la password della vostra chiave ssh e da quel momento in poi potrete utilizzare ssh senza necessitare della password.

**NB:** Un alternativa automatica alla gestione di ssh-add è **keychain**. Un applicativo che si occupa della gestione di ssh-add automatizzando e quindi richiedendo se necessario la nuova password per una nuova gestione ssh con nuova password e nuova public key.<a title="keychain" href="http://agriffis.n01se.net/keychain/" target="_blank"></a>

<pre><a href="http://agriffis.n01se.net/keychain/" target="_blank">Official Site</a></pre>

<pre><a href="http://slackbuilds.org/repository/12.2/misc/keychain/" target="_blank">Slackware</a> Slackbuild</pre>

\# End