---
id: 308
title: 'Trasferimento file via ssh: scp'
date: 2009-01-19T22:39:43+01:00
author: ax
excerpt: |
  Inviare file e/o intere cartelle con l'ausilio di "SSH" in maniera sicura, facile e veloce: scp (secure copy).
layout: post
guid: http://linuax.wordpress.com/?p=308
permalink: /2009/01/19/trasferimento-file-via-ssh-scp/
categories:
  - How-to
  - Linux
  - terminal
  - Tips
tags:
  - Linux
  - scp
  - ssh
  - Tips
---
Molto spesso per operare sul trasferimento file da locale a remoto e viceversa si ricorre ai metodi comuni quali può essere un "server ftp". Senza alcun dubbio è un metodo consono a questo utilizzo ma che comporta in caso di servizi già attivi sulla box all'utilizzo di ulteriori demoni e quindi di risorse. Sulle linuxbox spesso è già attivo e/o in uso di default un servizio necessario all'amministrazione remota, ovvero ssh (porta 22). Può essere sicuramente più comodo qualora decidessimo di inviare file o intere cartelle con l'ausilio dei servizzi minimi e della criptatura di pacchetti di dati durante l'invio, così da ovviare magari ad un possibile sniffing sulla rete.

Si può quindi usufruire del comando: **scp** (secure copy).

Al contrario del demone **SSHD, SCP** non permette la connessione all’host, ma solo il trasferimento dei files sfruttando quest'ultimo come strumento di invio e ricezione di trasferimento di dati criptati.

Affinché il comando _scp_ funzioni, il demone **sshd** deve quindi essere attivo.

Per intenderci non si stà facendo altro che sfruttare il demone sshd per il trasferimento file attraverso l'uso di scp. La sintassi è estremamente semplice quanto intuitiva:

Per il trasferimento di un singolo file da cartella locale a host remoto:

<pre>~$ scp -P9696 /home/user/file.esempio user@host:/home/esempio/nuovo_nome_Del_file.esempio</pre>

**NB:** **-P9696** è la porta su cui è avviato il demone sshd.

Per il trasferimento di un singolo file da remoto a locale:

<pre>~$ scp -P9696 user@remote_host:/remotedir/file /home/utente/newfile.esempio</pre>

**NB:** Un'altra ed utile caratteristica di scp ci permette di copiare ricorsivamente le directory con l'opzione **'-r'**.