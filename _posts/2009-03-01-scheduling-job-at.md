---
id: 408
title: 'Scheduling "job": at'
date: 2009-03-01T00:37:18+01:00
author: ax
excerpt: |
  Il tool Cron trattato nei precedenti post consente l'esecuzione di script, comandi e proceduri ad intervalli di tempo regolari per mezzo del daemon crond e di un proprio file di configurazione.
  Diversamente da esso, il tool At,  consente anche di eseguire operazioni pianificate dei job ma impostando tali operazioni direttamente dalla shell piuttosto che utilizzare un file di configurazione.
layout: post
guid: http://linuax.wordpress.com/?p=408
permalink: /2009/03/01/scheduling-job-at/
categories:
  - How-to
  - Linux
  - Tips
tags:
  - at
  - How-to
  - job
  - Linux
  - Scheduling
  - Tips
---
Il tool _**Cron**_ trattato nei precedenti post consente l'esecuzione di script, comandi e proceduri ad intervalli di tempo regolari per mezzo del daemon **crond** e di un proprio file di configurazione.  
Diversamente da esso, il tool _**At**_,  consente anche di eseguire operazioni pianificate dei job ma impostando tali operazioni direttamente dalla shell piuttosto che utilizzare un file di configurazione.

**NB**: _**At**_ è da preferire per lo scheduling di quelle operazioni che si vogliono abilitare direttamente da shell. A differenza di **cron**, le operazioni eseguite e pianificate con _**At**_ saranno perse al reboot.

#### Sintassi del comando:**  
** 

Digitando il comando _**at**_ seguito dall’orario o dalla data ci si trova di fronte ad un prompt a cui passare i comandi che si vogliono eseguire :

<pre>~$ at 1:10
warning: commands will be executed using /bin/sh
at&gt;</pre>

L’orario può essere specificato in formato 24h o am-pm, in quest’ultimo caso va specificato (at 4:00 pm). La data può essere espressa in formato MM/DD/YYYY oppure YYYY-MM-DD . E’ possibile inoltre indicare il nome del mese (jan, feb, etc..) e il giorno tramite il numero (1,2..31) e non indicare l’anno. I seguenti comandi sono equivalenti

> <pre>at 11/21/2008</pre>
> 
> <pre>at 2008-11-21</pre>
> 
> <pre>at nov 21</pre>

E’ possibile utilizzare l’operatore + per indicare un orario relativo secondo lo schema:

<pre>+ &lt;numero&gt; &lt;unità di tempo&gt;<strong> </strong></pre>

dove l’unità di tempo può essere **minutes, hours, days** o **weeks**. Alcuni esempi possono essere:

<pre>at 22:30 + 2 days - il comando sarà eseguito fra due giorni alle 22 e 30</pre>

Una volta inseriti i comandi da eseguire, per uscire dal prompt di _**at**_ si usa la combinazioni di tasti: **ctrl+d** che ci restituirà come output:

<pre>at&gt; &lt;EOT&gt;
job 5 at Sun Mar  1 01:10:00 2009</pre>

La lista dei job impostati può essere visualizzata digitando il comando _**atq**_ o anche con _**at -l****:**_

<pre>~$ atq                                                                  
6    Sun Mar  1 01:20:00 2009 ax ax</pre>

Si può poi decidere di rimuovere il job tramite _**atrm**_ seguito dal **numero corrispondente**.

**NB**: Ovviamente i risultati di questi comandi si riferiscono ai job (o tasks) relativi all’utente con il quale si è loggati.

La sintassi di _**at**_ comprende anche dei parametri _informali_ con i quali è possibile configurare un job in modo amichevole esempio :

<pre>~$ at teatime
at&gt; echo “è ora del thè”
at&gt; &lt;EOT&gt;</pre>

Restituirà come output “è ora del thè” alle 4 del pomeriggio. I parametri di questo tipo sono:

<pre>teatime = alle 16
now = esegue il comando immediatamente
noon = 12 am
midnight = 24 o 12 pm
today, tomorrow = oggi e domani</pre>

**NB**: Se si avvia _**at**_ indicando un orario **già** passato, il comando verrà eseguito immediatamente. Stessa cosa accade se il sistema è spento nel momento in cui avrebbe dovuto eseguire un job di _**at**_; al riavvio verrà eseguito immediatamente.

**NB**: Il sistema di autorizzazioni per l’esecuzione dei job per gli utenti del sistema è simile a quello di _**cron**_, sono presenti infatti due file **at.deny** e **at.allow** con la liste degli utenti abilitati o meno. ****

**Esempi di uso comune:**

1. Eseguire uno script ad un orario prefissato:

<pre>~# at 1:50
at&gt; /sbin/script.sh
at&gt; [Ctrl]+[D]</pre>

2. Eseguire uno script e/o un comando al trascorrere di 1 minuto:

<pre>~# at now +1min
at&gt; shutdown -h now
at&gt; [Ctrl] +[D]</pre>

3. Esempio scritto in maniera differente "con identico risultato" mostra come eseguire uno script e/o comando alle 16:00 di dopo domani:

<pre>~# at 4pm +2 days</pre>

e/o

<pre>~# at 16:00 08/12/2004
at&gt; echo -n "esecuzione script in corso..."
at&gt; sh /home/ax/script.sh
at&gt; echo  "script eseguito."</pre>

\# End