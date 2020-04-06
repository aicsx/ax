---
id: 222
title: 'Socket UDP - request/response e multiplexing'
date: 2009-01-08T23:41:49+01:00
author: ax
excerpt: "L'UDP può diventare una scelta obbligata, nel caso in cui manchino risorse per una implementazione dello stack TCP. Spedizione, consegna, protocolli request/response, multiplexing : UDP."
layout: post
guid: http://linuax.wordpress.com/?p=222
permalink: /2009/01/08/socket-udp-requestresponse-e-multiplexing/
categories:
  - TCP/UDP/ICMP
tags:
  - How-to
  - Linux
  - socket
  - udp
---
**INTRO**

Ci sono molti casi in cui il **TCP** può **non** offrire le massime performance e ci sono hardware per cui uno stack TCP è troppo oneroso ( quali ad esempio le schede ethernet che consentono di fare il boot via rete). Ci sono altre applicazioni ad esempio per il quale il TCP non è pratico, quali ad esempio: la trasmissione di dati in broadcast. Il tutto per dire che spesso per alcune applicazioni è spesso necessario l'utilizzo del protocollo **UDP**, che non è orientato alla connessione, ma si preoccupa solo di recapitare un pacchetto dal mittente al destinatario con poche aggiunte rispetto al protocollo IP. La più importante di queste è la presenza delle porte di **origine e di destinazione**, che permettono di identificare non solo l'host mittente e/o destinatario di un pacchetto, ma anche il processo mittente e/o destinatario dello stesso. Un normale server può avere diverse porte **UDP** in ascolto contemporaneamente, quali ad esempio: la porta 53 per il servizio **DNS**,etc.

**NB**: Al contrario del TCP, il protocollo UDP non gestisce in alcun modo la perdita, la duplicazione e la consegna fuori ordine dei pacchetti. Se l'applicazione necessita di utilizzare UDP, ma deve garantire alcuni meccanismi di affidabilità è necessario che il programmante implementi da solo questi meccanismi, finendo per reimplementare un pezzo del TCP. Per questo è buona norma fare una analisi prima di stabilire quali dei due protocolli usare. Tanto per inciso se cercate la consegna affidabile e la gestione della sequenza; UDP non fà per voi. Chiaramente ogni caso è un caso a se, queste sono valutazioni generali.

**Send a UDP packet**

Al contrario di ciò che avveniva con TCP, un server ed un client UDP utilizzano più o meno le stesse chiamate di sistema per fare processo.

Di seguito troverete due listati:

  * udp-send-client.c: che spedisce pacchetti UDP all'indirizzo e alla porta specificati tramite il primo e il secondo argomento della linea di comando. I dati da spedire vengono letti dallo standard input una riga per volta e, ad ogni **"invio"**, viene spedito un pacchetto UDP.

  * udp-print-server.c:  contrariamente a ciò che fa il client, questo listato legge i pacchetti UDP specificati e scrive il loro contenuto nello standard output.

Per il test dei listati basterà quindi aprire due emulatori di terminali in locale;

Nel primo, lato server eseguiremo dopo averli compilati:

<pre>~$ ./udp-print-server &lt;porta&gt;</pre>

Nel secondo, lato client, da cui spediremo i pacchetti:

<pre>~$ ./udp-send-client &lt;host&gt; &lt;porta&gt;</pre>

**NB:** <porta> = porta da voi scelta in locale per testare fra client e server la comunicazione di pacchetti UDP: esempio 12345

***NB:** Tutto ciò che verra scritto dalla parte client apparirà nel terminale del lato server, nel caso il test avvenga in locale. In caso contrario attraverso internet alcuni pacchetit potrebbero essere persi, duplicati o consegnati fuori ordine; Alcune righe potrebbero non arrivare dall'altra parte (succede spesso se la connessione è in idle).

Come detto in precedenza, a differenza di TCP, i due listati "client e server" sono molto simili fra loro. Le uniche differenze sono date dalla presenza della chiamata di sistema **bind()** solo in **udp-print-server.c**

I due programmi aprono un socket UDP grazie alla chiamata di sistema _socket_.

Entrambi i listati presentano una struttura che rappresenta un indirizzo internet, che il server utilizzerà per bindare il socket appena creato ad un indirizzo, in modo da poter ricevere pacchetti che arrivano sulla porta scelta dall'utente.

Il client invece utilizza una struttura con la chiamata _sendto_: equivalente di chiamata di sistema per far sapere allo stesso a chi inviare il pacchetto UDP.

**NB:** Essendo il protocollo UDP non connesso, possiamo usare lo stesso socket per spedire diversi pacchetti UDP a diversi host e in diverse porte. Il fato che UDP non è connesso spiega anche il perchè non necessitiamo delle chiamat:

  * _listen_ 
  * _accept_ 
  * _connect_

Non c'è alcun backlog da settare in _listen_, nè alcuna connessione da accettare con _accept_. Tuttavia, anche se molto raramente usata, la chiamata _connect_ è applicabile ai socket **UDP**: server a connettere "virtualmente" un socket UDP ad un indirizzo destinatario, in modo da poter spedire pacchetti utilizzando la chiamata di sistema _send_ o _write_, invece che la _sendto_. In breve, l'utilità di connettere il socket si limita a dire al sistema operativo di spedire i pacchetti UDP ad un determinato indirizzo ed è solo un artificio software che non ha nulla a che fare con il protocollo sottostante. Anche se non è  illustrato nei listati, il client potrebbe benissimo leggere dal socket UDP, oltre che scriverci e viceversa per il server. Come avviene per le socket TCP, anche quelle UDP possono essere utilizzate sia per spedire che ricevere dati. Un server UDP sa a chi spedire una eventuale risposta perchè può avere, grazie alla chiamata di sistema _recvfrom_, informazioni sull'indirizzo e la porta del client che ha spedito un particolare pacchetto, come illustrato nel programma **udp-print-server.c** (che infatti stampa l'indirizzo IP dei pacchetti UDP all'inizio di ogni righa di output).

**udp-send-client.c**

<pre>/* udp-send-client.c - send lines read standard input to UDP &lt;port&gt; of &lt;host&gt;
   usage: udp-send-client &lt;host&gt; &lt;port&gt; 
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;sys/types.h&gt;
#include &lt;sys/socket.h&gt;
#include &lt;unistd.h&gt;
#include &lt;netinet/in.h&gt;
#include &lt;arpa/inet.h&gt;
#include &lt;netdb.h&gt;

#define BUFLEN 1024

int main(int argc, char **argv) {
    char buf[ BUFLEN] ;
    int s;
    struct sockaddr_in sa;
    int dport;

    if (argc != 3) {
        fprintf(stderr, "Usage: udp-send-client &lt;host&gt; &lt;port&gt;n");
        exit(1);
    }
    dport = atoi (argv[ 2] );

    /* open the udp socket */
    if ((s = socket(AF_INET, SOCK_DGRAM, 0)) == -1) {
        perror("socket");
        exit(1);
    }

    /* fill the address structure */
    memset(&sa, 0, sizeof(sa));
    sa.sin_family = AF_INET;
    sa.sin_port = htons(dport);

    /* resolve the name */
    if (inet_aton(argv[ 1], &sa.sin_addr) == 0) {
        struct hostent *he;

        /* argv[ 1] doesn't appear a valid IP address. try to resolve as name */
        he = gethostbyname(argv[ 1]);
        if (!he) {
            fprintf(stderr, "Can't resolve '%s'n", argv[ 1]);
            exit(1);
        }
        sa.sin_addr = * (struct in_addr*) he-&gt;h_addr;
    }

    /* loop: send an UDP packet to host/port for every input line */
    while(fgets(buf, BUFLEN, stdin)) {
        int wlen;

    wlen = sendto(s, buf, strlen(buf), 0, (struct sockaddr*)&sa, sizeof(sa));
    if (wlen == -1) {
        perror("sendto");
        exit(1);
        }
    }
    close(s);
    return 0;
}</pre>

**-Connessione e meccanismo di consegna dei pacchetti UDP**

Con il protocollo TCP il tutto avviene in maniera abbastanza naturale, poichè i socket TCP sono connessi, il kernel deve semplicemente spedire al socket TCP aperto da un determinato processo i dati che arrivano su quella connessione.

A differenza del TCP, il protocollo UDP invece non ha connessione, quindi l'unico parametro per la consegna rimane l'**IP** e la porta di destinazione del datagramma UDP.

Riassumendo i passi fondamentali per la ricezione dei pacchetti UDP:

  * Quando un processo crea un socket UDP, questo viene associato ad un indirizzo. Tramite la chiamata di sistema _bind_ è possibile forzare l'associazione del socket ad un indirizzo IP e una porta scelti dall'utente, altrimenti la porta viene scelta direttamente dal kernel, tra quelle non occupate, e per indirizzo sarà scelto il wildcard **INADDR_ANY,** ovvero il socket sarà associato a tutti gli indirizzi IP delle interfacce presenti nel sistema.
  * All'arrivo di un pacchetto UDP il kernel controlla se esiste un processo che ha un socket UDP associato a quell'indirizzo IP e porta. Se non ne trova nessuno, spedisce un errore, ovvero un pacchetto ICMP "_destination unreachable / port unreachable_", al mittente. Se invece c'è un processo che ha un socket UDP associato il pacchetto viene accodato al buffer di ricezione e potrà essere letto dal processo tramite le solite chiamate di sistema.

**NB:** L'ultimo punto è interessante perchè ci fa capire che con UDP la consegna sia affidata praticamente solo ad un intero di 16 bit, che è la porta di destinazione. In più, un pacchetto UDP può essere spedito da un indirizzo _fake_ (ossia può essere spedito utilizzando l'IP spoofing) senza alcun problema, poichè, non ci sono i numeri di sequenza del TCP da indovinare per riuscire ad inserirsi in una connessione instaurata. Queste sono alcune delle ragioni che ci fanno capire che se un applicativo non implementa di suo dei meccanismi di autenticazione dei pacchetti, può essere facile vittima di attacchi che possono avere esiti molto pesanti (vedi attacchi SNMP e DNS, negli archivi di BUGTRAQ).

**udp-print-server.c**

<pre>/* udp-print-server.c - print lines read from UDP &lt;port&gt;
   usage: udp-print-server &lt;port&gt; 
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;sys/types.h&gt;
#include &lt;sys/socket.h&gt;
#include &lt;unistd.h&gt;
#include &lt;netinet/in.h&gt;
#include &lt;arpa/inet.h&gt;
#include &lt;netdb.h&gt;

#define BUFLEN 1024

int main(int argc, char **argv) {

    char buf[ BUFLEN];
    int s;
    struct sockaddr_in sa;
    int dport;

    if (argc != 2) {
        fprintf(stderr, "Usage: udp-print-server &lt;port&gt;n");
        exit(1);
    }
    dport = atoi (argv[ 1] );

    /* open the udp socket */
    if ((s = socket(AF_INET, SOCK_DGRAM, 0)) == -1) {
        perror("socket");
        exit(1);
    }

    /* fill the address structure */
    memset(&sa, 0, sizeof(sa));
    sa.sin_family = AF_INET;
    sa.sin_port = htons(dport);
        sa.sin_addr.s_addr = htonl(INADDR_ANY);

    /* bind the socket */
    if (bind(s, (struct sockaddr*) &sa, sizeof(sa)) == -1) {
        perror("bind");
        exit(1);
    }

    /* loop: read UDP data from &lt;port&gt; and print to standard output */
    while(1) {
        int rlen;
        struct sockaddr_in ca;
        socklen_t calen = sizeof(ca);

        rlen = recvfrom(s, buf, BUFLEN-1, 0, (struct sockaddr*)&ca, &calen);
        if (rlen == -1) {
            perror("sendto");
            exit(1);
    } else if (rlen == 0) {
        break;
    }
    buf[ rlen] = '' ;
    printf("[ %s] %s", inet_ntoa(ca.sin_addr), buf);
    }
    close(s);
    return 0;
}</pre>

**Protocolli request/response UDP**

Come citato, UDP presenta alcuni svantaggi rispetto a TCP ma,come già detto tra le righe, alcuni vantaggi chiave.

Uno di questi sfrutta il protocollo **DNS** che si occupa della risoluzione dei nomi degli host, è la bassisima latenza di un protocollo **request/response** implementato grazie a UDP.

Con **TCP** sarebbe necessaria aprire una connessione ad ogni richiesta **DNS**. Per spedire una semplice richiesta ed ottenere una risposta utilizzando TCP il prezzo da pagare è l'_RTT_ (tempo di andata e ritorno di un pacchetto) tra il client e il server moltiplicato per due, la prima: per scambiare i numeri di sequenza tramite due pacchetti TCP, uno con flag _SYN_ e l'altro con flag _SYN/ACK_; la seconda: per il terzo pacchetto "_ACK_" che contiene la richiesta ed un quarto pacchetto di ritorno dal server con la risposta. Senza contare che dopo tale spreco di processi è necessario chiudere la connessione TCP. Alla fine avremmo ottenuto una risposta con un tempo minimo di RTT*2, spendendo circa 10 pacchetti inclusa l'apertura e lo shotdown nella connessione TCP.

Con UDP invece il tempo di RTT è sufficiente a spedire una richiesta ed ottenere una risposta, perchè servono semplicemente due pacchetti: andata e ritorno.  Questa è una delle ragioni principali per un protocollo come il DNS, utilizzato continuamente da tutti i client ed i server su internet, non può permettersi di utilizzare TCP, con un tempo medio duplicato per ottenere una risposta dal server ed una quantità di pacchetti spediti cinque volte maggiore.

**NB:** Ovviamente con UDP può capitare non di rado la perdita e di pacchetti. Ma è una condizione che si verifica raramente, e solo in casi di congestione di rete o problemi hardware.

**UDP e multiplexing**

Anche con UDP il **multiplexing** e l'I/O non bloccante giocano un ruolo importante nella creazione di applicazioni complesse, poichè permettono, come non accadeva con le socket TCP, di monitorare allo stesso tempo più socket associati, per esempio a diversi client, e/o di implementare operazioni di timeout nella lettura e nella scrittura dei dati.

La natura dell'**UDP** permette di gestire più client simultaneamente utilizzando un solo processo in maniera molto più naurale di quanto non accade con TCP, perchè solitamente tutto quello che deve fare il server è leggere continuamente da un solo socket tutti i pacchetti che arrivano dai vari client e rispondere alle richieste in arrivo singolarmente.

**NB:** Chiaramente anche per UDP la scrittura può riempire il buffer di output del socket e rendere bloccante la chiamata di sistema _sendto_, tuttavia almeno nei casi pi ùsemplici di protocolli resuest/response il server può limitarsi a far finta di aver spedito il pacchetto UDP, anche se questo non è partito. Questo accade perchè in ogni caso il pacchetto potrebbe essere stato perso ancora prima di essere arrivato al client. Per aumentare in questo caso il buffer di scrittura e lettura del socket si ricorre alla chiamata di sistema _setsockopt_, per avere meno probabilità di incappare in buffer saturi.

Ovviamente questo sistema non aiuta in caso che il server sia constantemente congestionato dalle richieste, ma può attutire il saturamento del buffer di lettura e scrittura nei casi di latenza alta, o quando ci sono per qualche motivopicchi di richieste che durano per poco tempo.

Per aumentare la dimensione dei buffer dei socket si può utilizzare la chiamata di sistema **setsockopt** .

Per il buffer di input:

<pre>int size = new_buffer_size;
setsockopt(s, SOL_SOCKET, SO_RCVBUF, &size, sizeof(size);</pre>

Per il buffer di output:

<pre>int size = new_buffer_size;
setsockopt(s, SOL_SOCKET, SO_SNDBUF, &size, sizeof(size);</pre>

NOTE: i socket UDP sono qualcosa da conoscere bene, un giorno potrebbe capitare di dover sviluppare un applicativo che si basa su semplici richieste request/response molto frequenti o di implementare un protocollo che utilizza il broadcast. Magari potrebbe capitare di connettere ad internet dell'hardware che non ha abbastanza risorse per una implementazione dello stack TCP.

In entrambe le circostanze, l'**UDP,** risulterà qualcosa a cui poter pensare o addirittura una scelta obbligatoria.

\# END