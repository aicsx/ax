---
id: 1209
title: 'Editor di testo non interattivo: sed'
date: 2010-02-01T07:00:31+01:00
author: ax
excerpt: 'Guida generale alla sintassi, esecuzione e selezione di comandi operatore di un editor che si occupa della manipolazione di file/flussi di testo in modalità non interattiva: sed.'
layout: post
guid: http://linuax.wordpress.com/?p=1209
permalink: /2010/02/01/editor-di-testo-non-interattivo-sed/
email_notification:
  - "1265004037"
  - "1265004037"
categories:
  - Bash
  - How-to
  - Linux
  - terminal
tags:
  - bash shell
  - editor
  - How-to
  - Linux
  - sed
  - terminale
---
Ad oggi non esiste sistema GNU/Linux che non sia dotato delle utility di elaborazione di testo: **<a href="http://www.gnu.org/software/sed/" target="_blank">sed</a>, awk**. In specifico, in questo post si tratterà una panoramica generale su **sed.**

### **Cos'è:**

Sed è un editor che si occupa della manipolazione di file di testo in modalità non interattiva. Come tutti gli _**editor**_ di testo tale manipolazione opera attraverso la modifica e trasformazione di un file. La differenza però con gli editor comuni sta nel fatto che **sed** opera in standard non interattivo ed agisce in ordine: ricevendo un _testo/file/stdin_ come input che poi sarà opportunamente trattato/modificato attraverso i parametri scelti con la specificazione delle righe, _una alla volta_; terminata questa fase si preoccuperà di inviare il risultato ad un _file/stdout_. Sostanzialmente più che un editor vero e proprio funge da _filtro trasformatore_.

**NB**: l'utilizzo di **sed** in tutte le sue varianti presuppone una discreta/media conoscenza delle regexp. A tal proposito è consigliata la lettura del post relativo; **<a href="http://linuax.wordpress.com/2010/01/17/le-espressioni-regolari-regexp/" target="_blank">le espressioni regolari: regexp</a>**.

### **Sintassi del comando:**

<pre>~$ sed --h

<strong>NB</strong>: sono elencate e commentate le opzioni generali di utilizzo. In specifico, le ritenute fondamentali.

-n, --quiet, --silent              <strong>    # </strong>evita che venga mostrato ciò che viene fatto <em>"modalità silent"</em> (uguale al parametro --<strong>quiet</strong> e/o <strong>-n</strong>)

-e script, --expression=script<strong> </strong>     <strong>   # </strong>aggiunge lo script definito da riga di comando ai comandi da eseguire. Script definito all'interno degli apici <strong>''</strong> e/o <strong>""</strong> <em>(più comandi sono separati dal punto e virgola</em> "<strong>;</strong>"<em>)</em>

-f script.sh, --file=script.sh <strong>        #</strong> aggiunge questa volta il contenuto del file <em>script.sh</em> ai comandi da eseguire.
</pre>

<pre>-i[SUFFIX], --in-place[=SUFFIX] <strong> #</strong> scriverà il risultato direttamente sul file originale modificandolo. 

<strong>NB</strong>: l'opzione <strong>-i</strong> può essere associata ad un suffisso che verrà preso in considerazione per creare un file backup come dagli esempi:
sed -i <strong>                 #</strong> modificherà il file originario senza creare alcun backup
sed --in-place <strong>         #</strong> identico all'esempio precedente -i
sed -i.backup <strong>          #</strong> modificherà il file originario creando salvando una copia di backup dello stesso
sed --in-place=.backup <strong> #</strong> come l'esempio precedente  -i.backup</pre>

### **Comandi** operatori:

**NB**: sed tende a suddividere i comandi generali secondo due categorie. La prima definisce quali e che tipo di operazioni eseguire. La seconda categoria invece racchiude le righe su cui operare e quindi delle specifiche delle stesse in termini di numero riga, classe, etc. Analizziamo in dettaglio:

<pre>p                   <strong>   #</strong> <em>"print"</em> stampa la riga o il file</pre>

<pre><strong>NB</strong>: Questa opzione è particolare. Per comprenderla analizzare gli esempi seguenti:
sed -e '' file    <strong> #</strong> stampa il file e non esegue nessuna operazione definita negli apici ''
sed -n -e '' file <strong> #</strong> non esegue nessuna operazione e non stampa il file (opzione -n)
sed -e 'p' file   <strong> #</strong> stampa il file "operazione definita da p" (ogni riga trovata due volte)
sed -n -e 'p' file <strong>#</strong> come sopra ma questa volta associa l'operazione print alla modalità silent (stampa quindi una sola volta ogni riga)</pre>

<pre>d                   <strong>   #</strong> <em>"delete"</em> elimina la riga

/regexp/p           <strong>   #</strong> stampa solo le righe corrispondenti alla regexp espressa

/regexp/d           <strong>   #</strong> elimina solo le righe corrispondenti alla regexp espressa
</pre>

<pre>s/testo/sostituzione/ <strong> # </strong>sostituisce la stringa <em>"testo</em>" con <em>"sostituzione"</em> 

y/abcd/ABCD/      <strong>     #</strong> come sopra ma analizza tutti i caratteri presi come riferimento nel modello1 "abcd" con i corrispondenti del modello2 "ABCD". Opera in sostituzione solo sui caratteri corrispondenti. Nell'esempio da abcd minuscolo a MAIUSCOLO.

g                      <strong>#</strong> definisce il parametro <em>"global"</em>. Agisce su tutte le verifiche d'occorrenza di ogni riga trovata.Utile nelle sostituzioni recursive su più righe e/o per lasciare inalterate il resto delle righe (vedi più avanti nel post).

s/testo/sostituzione/g <strong>#</strong> sostituisce tutte le parole testo con sostituzione. Agisce su tutto il file e non si ferma solo alla prima riga trovata.
</pre>

**NB**: l'ordine di suddivisione comandi vi risulterà inverso a quello descritto precedentemente. Ma vi rendererà il tutto di più facile comprensione.

### **Selettori di riga e specifiche operatori:**

<pre>3                      <strong>#</strong> corrisponde alla terza riga

3p                     <strong>#</strong> stampa la terza riga di default due volte e il resto una sola volta (se associato ad opzione -n precedentemente descritta la stamperà una sola volta)

,                      <strong>#</strong> <em>"virgola"</em> corrisponde al range da prima a dopo il simbolo. Esempio: 1,3 (le righe da 1 a 3)

~                      <strong>#</strong> <em>"infinito"</em> è usato per definire ciò che c'è prima come partenza e ciò che c'è dopo come fine. Esempio: 1~3 (parte da riga 1 e poi ogni 3)
</pre>

### Esempi comandi,selettori di riga e specifiche operatori:

<pre>1,4d                   <strong> #</strong> le prime quattro righe sono eliminate

2,4d                   <strong> #</strong> le righe da 2 a 4 sono eliminate

5,10p                   <strong>#</strong> le righe da 5 a 10 sono stampate

1~2p                   <strong> #</strong> le righe dispari sono stampate (parte dalla prima riga e poi ogni due)

2~2p                   <strong> #</strong> le righe pari sono stampate (parte dalla seconda riga e poi ogni due)

4~3p                   <strong> #</strong> stampa le righe 4 7 10 13 ... etc (parte dalla quarta riga e poi ogni 3)

4~4d                    <strong>#</strong> elimina le righe 4 8 12 16 ... etc (parte dalla quarta riga e poi ogni 4)

/regexp/,/regexp-sost/p <strong>#</strong> stampa tutte le righe comprese tra la prima regexp espressa e la seconda.

2~2s/pippo/PIPPO/g      <strong>#</strong> sostituisce la parola pippo con PIPPO ma solo agendo sulle righe pari.Come da esempio la prima parte definisce le righe e la seconda l'operazione.

/^$/d                   <strong>#</strong> cancella tutte le righe vuote

/Pippo/p                <strong>#</strong> stampa tutte le righe in cui è presente Pippo (da usare con opzione -n)

/PIPPO/d                <strong>#</strong> cancella tutte le righe in cui è presente PIPPO e procede alla cancellazione del resto della riga in cui è presente la parola

s/PIPPO//g             <strong> #</strong> cancella la parola PIPPO in ogni riga sostituendola con nulla e lasciando il resto della riga intatto.

<strong>IMPORTANTE</strong>: questo esempio di doppio slash applicato alla frase seguente chiarisce il significato di una sostituzione applicata con nulla seguita dal simbolo <strong>g</strong> <em>"global"</em> che lascierà intatto il resto della riga.
</pre>

**NB**: L'espressione di tipo s/PIPPO,//g nella riga <span style="color:#993300;">io adoro PIPPO, pluto e paperone</span> opportunamente creata modificherà la frase in modo da risultare come <span style="color:#333399;">io adoro pluto e paperone</span>.

Prova su campo con sed:

<pre>~$ echo io adoro PIPPO, pluto e paperone &gt;&gt; prova.txt
~$ cat prova.txt
io adoro PIPPO, pluto e paperone

~$ sed -i -e 's/PIPPO,//g' prova.txt
~$ cat prova.txt
io adoro pluto e paperone</pre>

**NB:** i più temerari ricorderanno leggendo il post precedente su regexp che spesso poteva capitare di dover usufruire dello slash **/** come simbolo da ricercare che quindi era considerato come un carattere speciale _(come lo possono essere il punto,la virgola,etc)_ e che quindi per arrivare allo scopo era necessario utilizzare lo slash rovesciato **""** seguito dal carattere speciale da ricercare**.** Ovviamente in molte regexp di tipo **sostitutivo 's// '** potrebbe risultare fastidioso quindi scrivere continuamente **/** per effettuare una sostituzione come lo era l'esempio:

<pre>s/Linux/GNU<strong>/</strong>Linux/g    <strong>#</strong> lo slash rovesciato  tratta il seguente slash normale / come un carattere speciale. Altrimenti sarebbe considerato come fine regexp.Sostituzione della parola Linux con GNU/Linux in ogni riga.</pre>

Per evitare questa cosa si può scegliere di sostituire gli slash con altri limitatori come ad esempio i due punti " **:** "come da esempio seguente.

<pre>s:regexp:sostituzione:g
In specifico per provare creaiamo un file prova.txt come da esempio:

~$ echo Linux &gt;&gt; prova.txt

~$ cat prova.txt
<span style="color:#993300;">Linux</span>

~$ sed -i -e 's:Linux:GNU/Linux:g' prova.txt

~$ cat prova.txt
<span style="color:#333399;">GNU/Linux</span>
</pre>

**Considerazioni**: come sempre è utile capire che la potenza è nascosta nelle cose apparentemente banali. Bisogna tener conto che l'utilizzo di sed è fondamentale per la creazione di una pipe e che molto spesso è associato ad altre utility del suo genere per operazioni complesse all'interno di scripts. Il miglior modo per imparare quindi è come al solito: cominciare e provare, provare, provare. Saluti.

**NB**: questo post è una sorta di sommario e di appunti e quindi i riferimenti e la linea generale di trattamento del post, come nel precedente su **regexp**, sono legati al **<a href="http://tldp.org/LDP/abs/html/x22440.html" target="_blank">link ufficiale</a>** di riferimento su Advanced Bash-Scripting Guide.

\# End