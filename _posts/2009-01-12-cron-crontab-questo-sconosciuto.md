---
id: 239
title: 'Cron, crontab: questo sconosciuto!'
date: 2009-01-12T00:03:39+01:00
author: ax
excerpt: "Spesso c'è la necessita' di eseguire un comando, uno script o un programma ad intervalli regolari di tempo, un mese, un'ora, un giorno, una settimana o altri. Un meccanismo presente di default in Linux che e' rappresentato dal demone cron."
layout: post
guid: http://linuax.wordpress.com/?p=239
permalink: /2009/01/12/cron-crontab-questo-sconosciuto/
categories:
  - Arch
  - How-to
  - Linux
  - Slackware
tags:
  - How-to
  - Linux
  - Scheduling
---
Spesso c'è la necessita' di eseguire un comando, uno script o un programma ad intervalli regolari di tempo, un mese, un'ora, un giorno, una settimana o altri. Un meccanismo presente di default in Linux che e' rappresentato dal demone **cron.**

Cron viene attivato ogni minuto e legge degli opportuni file di configurazione dove vengono specificate le operazioni da compiere e gli intervalli relativi. Se trova qualcosa da eseguire nel minuto in cui si e' attivato, allora procede all'operazione. Tutto cio' che viene scritto sullo standard output, viene inviato via email al proprietario del cron, tipicamente "root".  
Cron non deve essere riavviato per ogni modifica. Ci sono modi diversi di specificare operazioni ed intervalli.

**-Configurazioni e modalità-**

La via piu' semplice e' la creazione di uno script "sh, bash, perl, etc.."  che esegue cio' che vogliamo. Tale script andra' inserito in una delle directory seguenti

<pre>/etc/cron.daily
/etc/cron.hourly
/etc/cron.monthly
/etc/cron.weekly</pre>

per poter essere eseguito ad intervalli definiti ( un giorno, un'ora, un mese e una settimana). E' possibile anche mantenere tutti gli script in una directory standard (ad esempio "/avvio") e poi creare un soft link "ln -s" in una di quelle sopra elencate.

Per intervalli di tempo diversi da quelli standard, e' possibile usare la directory

<pre>/etc/cron.d</pre>

oppure il file

<pre>/etc/crontab</pre>

In **cron.d** possono essere inseriti file con formato particolare (lo stesso di **crontab**), indicanti operazioni ed intervalli di tempo piccoli anche solo minuto.

**-FORMATO**:

Possono essere inseriti in ogni file

  * commenti
  * linee vuote
  * assegnazioni di variabili di ambiente
  * comandi cron :i commenti sono linee che iniziano con il simbolo "#" e come le linee vuote vengono ignorati.

Per quanto riguarda le variabili di ambiente, fra le possibili utilizzabili vi sono

  * **SHELL**, ovvero la shell da usare per l'esecuzione dei comandi
  * **PATH**, ovvero il path dove cercare i comandi indicati
  * **MAILTO**, ovvero a chi mandare via email l'output dei comandi. L'email viene inviata anche se questa variabile non viene settata

Una tecnica comunemente usata per evitare l'invio della mail, o evitare la scrittura del risultato dell'operazione sulla mail che viene inviata, e' la redirezione dell'output sul dispositivo "/dev/null", esempio:

<pre>/prova/test.sh&gt;/dev/null</pre>

I comandi cron rappresentano la parte piu' complessa della configurazione.  
Il formato di un comando cron e' il seguente:

<pre>s1 s2 s3 s4 s5 Proprietario Comando</pre>

Analizzando:

**Comando** e' un comando da eseguire (con i relativi parametri ed eventuali redirezioni di output).  
**Proprietario** indica a chi appartiene il comando cron (es. root o altri utenti).  
****

**s1**, **s2**, **s3**, **s4**, **s5** sono stringhe che specificano l'intervallo di esecuzione. Ognuna di queste stringhe puo' contenere il carattere "*" per indicare tutti i valori possibili.

_s1_ rappresenta i minuti. I valori permessi vanno da 0 a 59. E' possibile specificare anche intervalli (piu' avanti spiegati brevemente).  
_s2_ rappresenta le ore. I valori permessi vanno da 0 a 23. E' possibile specificare anche intervalli (piu' avanti spiegati brevemente).  
_s3_ rappresenta i giorni all'interno di un mese. I valori permessi vanno da 1 a 31. E' possibile specificare anche intervalli (piu' avanti spiegati brevemente).  
_s4_ rappresenta i mesi. I valori permessi vanno da 1 a 12 (piu' i nomi in lettere). E' possibile specificare anche intervalli (piu' avanti spiegati brevemente).  
_s5_ rappresenta i giorni della settimana. I valori permessi vanno da 0 a 7 (piu' i nomi in lettere, 0 e 7 rappresentano entrambi la Domenica). E' possibile specificare anche intervalli (piu' avanti spiegati brevemente).

**NB:** Tutte e cinque le stringhe possono rappresentare un numero, tutti i valori oppure un intervallo.  
Specificando un numero, si indica un momento preciso. Ad esempio, mettere 2 in s4, significa eseguire a Febbraio (vanno considerate poi anche tutte le altre 4 stringhe).  
Se il particolare campo non interessa, cioe' non e' utile alla definizione del tempo di attivazione, puo' essere usato l'asterisco ("*") per indicare tutti i valori.

Molto spesso, pero' quello che serve e' un certo intervallo e non un momento unico e preciso. Per esempio, potremmo volere eseguire un comando ogni 2 minuti invece che sempre e solo nel secondo minuto. In questo caso, possiamo scrivere le cinque stringhe (o solo una parte di esse) sotto forma di intervallo. Vediamone qualche formato (non tutti potrebbero essere supportati su tutti i sistemi):

  * ***/n** indica ogni "n". Esempio, "*/2" indica ogni 2.
  * **n1-n2** indica da n1 a n2. Esempio, "5-8" indica 5,6,7,8.
  * **n1-n2/n3** indica ogni n3 all'interno del range n1-n2. Esempio, 3-21/6 indica 3,9,15,21 (ongi 6 nel range 3-21).
  * **n1,n2,...** indica i numeri specificati. Esempio, 4,7,8 indica tutti i numeri scritti.
  * **n1-n2,n3-n4** indica da n1 a n2 e da n3 a n4. Esempio, 3-5,7-9 indica 3,4,5,7,8,9.

Sono possibili anche altre combinazioni.  
Qualche esempio:

<pre class="codice"><em class="codice"># Esegue il comando indicato ogni cinque minuti, solo il 15 di ogni mese,
# con proprietario "root"</em>
*/5 * 15 * * root /bin/ls /var/log/ls&gt;/tmp/ls.output

<em class="codice"># Manda una mail con il testo indicato ogni giorno alle 8:00 di mattina,
# con proprietario "root". Se MAILTO vale "root" allora la mail arriva a root.</em>
0 8 * * * root echo "Salve!!!"

<em class="codice">#Esegue ogni minuto!!! Il proprietario e' "test".</em>
* * * * * test /usr/bin/mousepad</pre>

Un utente diverso da root, per usare dei propri comandi cron personalizzati, può agire direttamente sulla directory:

<pre>/var/spool/cron/crontabs/</pre>

Il file non dovrebbe essere editato direttamente e di solito non puo' esserlo da parte di un utente diverso da root in quanto viene gestito dal comando **/usr/bin/crontab**.

L'idea e' che venga mantenuto un _file master_ con i cron di un utente.  
Il file master puo' risiedere ovunque nel filesystem.  
Con l'installazione, crontab ne copia il contenuto all'interno del file "/var/spool/cron/crontabs/nomeutente".  
Sempre con crontab ("man crontab") e' possibile rimuovere i cron installati.  
La politica per cui un utente puo' usare il comando "crontab" e' regolata dai file

<pre>/etc/cron.allow
/etc/cron.deny</pre>

Supponiamo di usare come test l'utente "**utente1**" con home directory "**/home/utente1**". Creiamo il file "**/home/utente1/test.cron**" con il seguente contenuto:

<pre class="codice"><em class="codice"># Test di utilizzo di crontab</em>

MAILTO=utente1@localhost
* * * * * utente1 echo "ciao utente1"</pre>

Questo cron setta la mail per inviare l'output a utente1 e scrive il messaggio "ciao utente1" ogni minuto.

Per installare il contenuto del file indicato, eseguire nel caso si stia lavorando da utente root:

<pre>/usr/bin/crontab -u utente1 /home/utente1/test.cron</pre>

oppure, se si sta lavorando come utente "utente1":

<pre>/usr/bin/crontab /home/utente1/test.cron</pre>

Si notera' la comparsa del file "**/var/spool/cron/crontabs/utente1**" con il seguente contenuto (o simile):

<pre class="codice"><em class="codice"># DO NOT EDIT THIS FILE - edit the master and reinstall.
# (/home/user1/test.cron installed on *)
# (Cron version -- $Id: crontab.c etc....)</em>
MAILTO=utente1@localhost
* * * * * utente1 echo "ciao utente1"</pre>

_**NB**:_ L'installazione dei cron di un utente, sovrascrive i cron eventualmente gia' presenti per l'utente stesso.

Per aggiungere comandi cron senza eliminare quelli precedenti, e senza inserirli prima nel master, usare se si sta lavorando come utente root:

<pre>/usr/bin/crontab -u utente1 -e</pre>

se si sta lavorando come utente "user1":

<pre>/usr/bin/crontab -e</pre>

Si aprira' l'editor "vi" con i comandi gia' presenti e ci sara' la possibilita' di inserirne di nuovi oppure di modificare, oltre che eliminare, quelli esistenti. Tutto cio' non tocca il file master ma solo il file "_/var/spool/cron/crontabs/user1_", evitando la procedura di reinstallazione.  
Tipicamente si edita il file master e lo si reinstalla, in modo da evitare di perdere le modifiche inserite con "-e".  
Per eliminare i cron inseriti, usare se si sta lavorando come utente root:

<pre>/usr/bin/crontab -u user1 -r</pre>

se si sta lavorando come utente "utente1":

<pre>/usr/bin/crontab -r</pre>

#