---
id: 1005
title: 'Le espressioni regolari: regexp.'
date: 2010-01-17T08:00:26+01:00
author: ax
excerpt: Introduzione, sintassi/annuario e regole di utilizzo generale delle espressioni regolari associate a vari applicativi ed utilities come ls,grep,sed,awk.
layout: post
guid: http://linuax.wordpress.com/?p=1005
permalink: /2010/01/17/le-espressioni-regolari-regexp/
categories:
  - Bash
  - How-to
  - Linux
  - terminal
tags:
  - bash shell
  - How-to
  - Linux
  - regexp
---
Le espressioni regolari _**regexp**_ sono alla base di tutto ciò che comprende la modifica, ricerca e sostituzione di una sequenza/stringa di caratteri;  risultano essenziali per l'utilizzo di applicazioni ed utilities da sempre sovrane nei sistemi UNIX e derivati come **grep,sed,awk,** oltre che essere alla base della programmazione e della conoscenza di **bash**. Nascono appunto per definire stringhe di caratteri e di metacaratteri. Per stringhe di caratteri si intendono caratteri semplici _(numeri,lettere minuscole/maiuscole,accentate)_ e simboli _( .;*^[]{}/ etc..)_ .

Le **regexp** sono spesso utilizzate in vari linguaggi di programmazione (**bash,perl**,etc..) anche se a volte possono essere scritte e interpretate in maniera differente da un linguaggio all'altro in linea generale  il concetto di base legato alle **regexp** resta lo stesso. Come già detto in precedenza se ne fà largo uso in molti applicativi, tra cui <a href="http://www.gnu.org/software/sed/" target="_blank"><strong>sed</strong></a>.

**Sed** è un manipolatore di testo più che conosciuto da tutti gli utenti GNU/Linux che agendo su un file ricerca e all'occorrenza sostituisce stringhe e/o classi di caratteri e simboli definiti. Tali stringhe sono opportunamente suddivise per funzione. La regex singola legata all'utilizzo di **sed** si identifica perchè posta all'interno di due _slash_ _( / )_ come da esempio:

<pre>/regexp/</pre>

**NB**: se i caratteri contenuti negli slash risultano essere testo semplice, sarà ricercata esattamente quella sequenza di caratteri avente distinzione **_"case sensitive"_** (per caratteri minuscoli e MAIUSCOLI). Spesso però al testo semplice si aggiungono determinati simboli che definiscono meglio e più specificatamente la regex da ricercare.

**NB**: non necessariamene una regex deve essere definita negli slash. Infatti solitamente anche comandi comuni quale ad esempio potrebbe essere un semplice **grep** potrebbero richiedere l'utilizzo di una particolare classe di caratteri facente parte delle regexp _(il cui significato vedremo in seguito)_. Esempio:

<pre><tt>~$ </tt><tt>grep<strong> [[:digit:]]</strong><strong> </strong>file.test
abc=123</tt></pre>

Per prima cosa, come già detto precedentemente è bene elencare i simboli utilizzati nelle regexp e il loro significato.

**SIMBOLI**:

<pre><strong>*</strong>   "asterisco" - accetta zero o più ripetizioni del carattere che lo precede.
<strong>
. </strong>  "punto" - qualunque carattere.
<strong>
^</strong><strong> </strong>  "apice" - messo all'inizio di una regex impone che la stringa cominci con quello che segue l'apice. Se usato con le parentesi quadre e prima di un carattere assume significato di negazione (tranne ciò che segue).
<strong>
$</strong>   "dollaro" - se inserito alla fine della regex impone che la stringa finisca con ciò che precede il dollaro.

<strong>+</strong>   "più" - accetta una singola o più ripetizioni del carattere che lo precede.

<strong>[ ]</strong> "parentesi quadre" - tutti i caratteri all'interno delle parentesi sono presi in considerazione.
<strong>
[^ ]</strong> "parentesi e apice interno" - tutti i caratteri interni le parentesi sono esclusi.
</pre>

**NB**: il simbolo **/** _"slash"_ come già detto apre e chiude l'espressione. Il simbolo **** _"slash rovesciato"_ serve invece per trattare uno dei caratteri speciali che lo segue come un normale carattere. Esempio: **$** ricerca il simbolo del dollaro nel nostro file o nella nostra stringa.

**Esempi di Set di caratteri semplici definiti dalle parentesi quadre**:

<pre><strong>[abc]</strong>     -  cerca i caratteri <strong>a b c </strong>nella stringa.

<strong>[a-k] </strong>    -  si riferisce a tutti i caratteri compresi nel range tra la lettera <strong>a</strong> e la<strong> k.</strong>
<strong>
[C-In-o]  </strong>-  come prima con distinzioni <strong>"case sensitive"</strong>. Cerca caratteri di range <strong>C</strong> e <strong>I</strong> ed <strong>n</strong> e <strong>o</strong>.
<strong>
[0-9] </strong>    -  tutti i numeri.
<strong>
[a-z] </strong>    -  tutti le lettere minuscole.
<strong>
[A-Z]</strong>     -  tutte le lettere MAIUSCOLE.

<strong>[a-z0-9]</strong>  -  tutte le lettere minuscole e i numeri.
</pre>

**Esempi di negazione e combinazioni di parentesi quadre**:

<pre><strong>[^A-D]</strong>    -  corrisponde a tutti i caratteri tranne quelli contenuti nel range da <strong>A</strong> a <strong>D</strong>.

<strong>[Pp][Il][Pp][Oo]</strong> - corrisponde a tutte le combinazioni maiuscole e minuscole: PIPO,pipo,Pipo,pIpo,PIpo,piPo,PIPo,pipO,piPo..etc.
</pre>

**Classi di caratteri:**

Oltre ai simboli e i set di caratteri semplici (lettere,numeri) ci sono classi di caratteri definite che in determinati casi possono risultare estremamente utili:

<pre><strong>[:alnum:] </strong> -  corrisponde a tutti i numeri e le lettere <em>(sia minuscole che MAIUSCOLE comprese quelle accentate)</em>.Equivalente di <strong>[A-Za-z0-9]</strong>.

<strong>[:alpha:]</strong>  -  corrisponde a tutte le lettere comprese quelle accentate <em>(sia minuscole che MAIUSCOLE)</em>. Equivalente di <strong>[A-Za-z]</strong>.

<strong>[:digit:]</strong>  -  corrisponde a tutti i numeri. Equivalente di <strong>[0-9]</strong>.

<strong>[:lower:]</strong>  -  corrisponde a tutte le lettere minuscole. Equivalente di <strong>[a-z]</strong>.

<strong>[:upper:]</strong>  -  corrisponde a tutte le lettere MAIUSCOLE. Equivalente di <strong>[A-Z]</strong>.

<strong>[:blank:]</strong>  -  corrisponde a tutti gli spazi o tab.

<strong>[:space:]</strong>  -  corrisponde agli spazi.

<strong>[:cntrl:]</strong>  -  corrisponde ai caratteri di controllo.

<strong>[:print:]</strong>  -  corrisponde ai caratteri di range ASCII 32 - 126 compresi gli spazi.

<strong>[:graph:]</strong>  -  come [:print:] corrisponde ai caratteri di range ASCII 33 - 126 ma esclude gli spazi.

<strong>[:punct:]</strong>  -  corrisponde a tutti i caratteri di punteggiatura.

<strong>[:xdigit:]</strong> -  corrisponde a tutti i caratteri che possono formare un numero esadecimale. Equivalente di <strong>[0-9A-Fa-f]</strong>.
</pre>

**NB**: Allo stesso modo dei set di caratteri semplici, anche in questo caso  le classi di carattere vanno specificate e racchiuse in altre parentesi quadre []. Di conseguenza una classe esempio [:alnum:] và definita come:

<pre><strong>[[:alnum:]]</strong></pre>

**Esempi di utilizzo di regexp all'interno delle specifiche di sed**:

**NB**: chiarite le specifiche per simbologia e classi è bene fare qualche esempio di espressione mirata all'utilizzo di **sed** come da inizio post.

**Esempi caratteri e simboli:**

<pre><strong>
/./</strong>              - corrisponde a qualunque stringa contenente almeno un carattere.<strong>

/^#/</strong>             - qualunque stringa comincia con il carattere <strong># </strong><em>(solitamente usato per definire un commento)</em>.
<strong>
/ *$/</strong>            - qualunque stringa che finisce <em>(definito con $)</em> con uno o più spazi <em>(spazi ripetuti definiti da *)</em>.
<strong>
/[fui]/</strong>          - qualunque stringa che contiene anche solo una delle lettere <strong>f u</strong> ed <strong>i </strong>minuscole.
<strong>
/^[fui]/</strong>         - qualunque stringa che comincia con uno dei caratteri <strong>f u</strong> ed <strong>i</strong> minuscoli.
<strong>
/[^f]/ </strong>          - qualunque stringa che non contiene la lettera <strong>f </strong>minuscola.
<strong>
/^linux$/</strong>        - qualunque stringa che comincia con la parola linux e finisce con la stessa. Se ci sono stringhe contenenti altre parole e/o caratteri <em>(prima e dopo) </em>non saranno prese in considerazione.
<strong>
/^[Ll]inux/</strong>      - qualunque stringa comincia con la parola linux con iniziale sia MAIUSCOLA che minuscola.
<strong>
/^[Ll]inux+$/</strong>    - come l'esempio precedente ma comprende una o più ripetizioni della lettera <strong>x</strong>. Esempio: Linuxx , linuxxx, linuxxxxxx.
</pre>

**Esempi con set di caratteri:**

<pre><strong>/^[A-Z]+$/</strong>       - qualunque stringa composta da lettere MAIUSCOLE con almeno 1 carattere <em>(lettere accentate escluse)</em>.
<strong>
/^[a-z]+$/</strong>       - come il precedente esempio ma con lettere minuscole.
<strong>
/^[0-9]+$/ </strong>      - ancora uguale al precedente con i numeri.
</pre>

**Esempi con caratteri speciali (slash rovesciato):**

<pre><strong>/^[0-9]{8}$/</strong>    - qualunque riga che comincia con dei numeri ripetuti per <strong>8 </strong>volte e che finisce con essi definito dal simbolo <strong>$</strong> <em>(esempio si date espresse in sequenza 16012010)</em>.
<strong>
/^l{5}$/        - </strong>qualunque stringa contenente solo la lettera l minuscola ripetuta 5 volte e nient'altro <em>(lllll)</em>.
<strong>
/x{10}/</strong>         - qualunque stringa contenente la lettera <strong>x</strong> ripetuta almeno 10 volte <em>(linux Linuxxxxxxxxxxxxx pippo)</em>.
<strong>
/x{2}$/</strong>         - qualsiesi stringa contenente la lettera <strong>x</strong> ripetuta almeno due volte e nient'altro dopo di essa <em>(linux Linuxxx)</em>.
</pre>

Fatta una panoramica generale con qualche esempio utile, passiamo all'utilizzo base di queste espressioni. Prendiamo un file di testo semplice con delle stringhe esempio fatte da noi ed esercitiamoci attraverso l'uso di **sed**. Quindi:

<pre>~$ echo 12456 &gt; prova.txt
~$ echo lettere_miste: f g h l o ; punteggiatura .:;, tab: 000000 &gt;&gt; prova.txt
~$ echo linuxxxxx : questo è un esempio &gt;&gt; prova.txt
~$ echo accentate: è ò à &gt;&gt; prova.txt
~$ echo Linux &gt;&gt; prova.txt
</pre>

Salvato il nostro file, con stringhe esempio, non resta che provare a ricercare quelle interessate con la regexp giusta seguendo la sintassi di sed:

<pre>~$ sed -n -e '/regexp_da_inserire_qui/p' prova.txt</pre>

**NB**: Con il parametro finale **p** dopo gli **slash /** sed si limiterà a stampare ad output il risultato della ricerca delle stringhe corrispondenti la regexp scritta da voi nel file esempio.

Allo stesso modo si possono operare sostituzioni di parti di testo:

<pre>~$ sed -e 's/regexp/testo_da_sostituire_alla_regexp/g' prova.txt
</pre>

In specifico, prendendo in esempio il file prova.txt creato precedentemente:

<pre>~$ sed -i.backup -e 's/^[Ll]inux/GNU/Linux/g' prova.txt
</pre>

**NOTA**:  bisogna fare un distinguo tra la fase di ricerca e quella di sostituzione. Se operiamo su una regexp per una semplice ricerca, sed si limiterà a stampare a schermo tutte le stringhe che iniziano o contengono la regexp da noi imposta. Esempio: **'/^Linux/p' .**Al contrario se operiamo su una sostituzione del tipo:

<pre><strong>'s/^[Ll]inux/GNU/Linux/' </strong></pre>

**Sed** sostituirà la parola Linux _(iniziale sia MAIUSCOLA che minuscola)_ con GNU/Linux solo a patto che sia la prima parola della stringa.

**NB:** Come si nota dall'esempio lo **slash rovesciato** farà in modo di trattare lo slash successivo / come un normale carattere e non come la fine della regexp. In più operando su tale file e modificandolo creerà una copia di backup prova.txt.backup con le vecchies tringhe non sostituite.

**NB**: questo post non entra nelle specifiche del comando **sed** ma si limita ad una panoramica generalizzata di ciò che sed utilizza _(**regexp**)_ per effettuare manipolazioni e sostituzioni di flussi di testo, così come avviene per **awk**,**grep**,**ls** o per gli script **bash** da noi generati. Tali specifiche saranno spiegate in post successivi riguardo le singole utilities.

**Considerazioni Personali:** talvolta ci sono più modi per arrivare allo scopo. Il miglior modo per imparare è sperimentare. Il post, non è altro che una sorta di annuario di ciò che è stato ampiamente descritto da <a href="http://www.faqs.org/docs/abs/HTML/regexp.html" target="_blank">link</a> ufficiali di riferimento (**advanced bash scripting**) comprensivi di  qualche esempio personale mirato. Divertitevi, visto che il mondo delle **regexp** è molto più ampio di ciò che ci si aspetta _(come direbbe un caro amico: **questo argomento è una vera materia!**)_. Alla prossima.

\# End