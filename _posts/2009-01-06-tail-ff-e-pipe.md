---
id: 211
title: 'Tail [-f/F] e pipe (|)'
date: 2009-01-06T20:51:00+01:00
author: ax
excerpt: 'Amministrazione server: il comando Tail.'
layout: post
guid: http://linuax.wordpress.com/?p=211
permalink: /2009/01/06/tail-ff-e-pipe/
categories:
  - How-to
  - Linux
  - Tips
tags:
  - How-to
  - Linux
  - pipe
  - tail
  - Tips
---
Alcuni comandi sono necessari, spesso, nella normale amministrazione di un server. **Tail** è un comando che ritorna come output l'ultima parte del contenuto di un file.

Lanciato con l'opzione **-f** diventa uno strumento fondamentale per l'analisi di file che cambiano di continuo come ad esempio un file log di un server web o SMTP, in quanto lanciando il comando con l'opzione -f rimarrà in sospeso in un loop sulla fine del file, mostrandoci le nuove entri in tempo reale.

**NB:** Ovviamente esistono limitazioni a tale operazione; nel caso specifico non è possibile effettuare operazioni che coinvolgono più di un pipe **( | ).** I pipe sono un modo semplice per effettuare una serie di operazioni senza restituzione di output ad ogni singola operazione ma che anzi possono evitarci di riscrivere ogni volta le singole operazioni implementandole tutte in una stringa singola.

Esempi:

<pre>~# tail -f /var/log/maillog |grep -i Sent</pre>

Ci mostrerà tutti i messaggi che sta spedendo in questo momento Sendmail.

Limitazione:

<pre>~# tail -f /var/log/maillog |grep pippo |grep -i Sent</pre>

Nel caso specifico, se volessimo controllare tutti i messaggi spediti a user pippo non si otterrebbe alcun output in quanto non sono supportati pipe multipli. Si potrebbe ovviamente pensare di esprimere due stringhe di ricerca dei due **grep** in un'unica espressione regolare; in effetti è così, ma se volessimo qualcosa di più complesso ?

Per intenderci, se desiderassimo vedere una cosa del tipo:

<pre>~# tail -f /var/log/maillog |grep -i Sent |sed 's/pippo/name@name.it/g'</pre>

o in generale:

<pre>~# tail -f &lt;file&gt; |&lt;comm_1&gt; |&lt;comm_2&gt; | ... | &lt;comm_N&gt;</pre>

dovremmo utilizzare:

<pre>~# tail -f &lt;file&gt; |while read line: do echo $line |&lt;comm_1&gt; |&lt;comm_2&gt; | ... |&lt;comm_N&gt;: done</pre>

e quindi, preso come riferimento l'esempio di prima dovremmo utilizzare:

<pre>~# tail -f /var/log/maillog |while read line: do echo $line |grep -i pippo.*Sent |sed 's/pippo/name@name.it/g' : done</pre>

**NB:** Un'ultima osservazione riguarda la stessa opzione -f: il loop viene effettuato sul descrittore del file e non sul suo nome; per intenderci, se il file venisse ruotato da syslog, il nostro comando perderebbe la sua efficacia, smettendo di funzionare e senza restituire alcun errore. Per ovviare è disponibile l'opzione **-F** (_-follow=name -retry_) che si basa sul nome del file e tenta ad intervalli regolari di accedere al file.

#