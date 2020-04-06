---
id: 562
title: 'Indagine " Recupero di dati cancellati: Foremost'
date: 2009-03-20T15:56:17+01:00
author: ax
excerpt: 'Foremost è un tool di indagine &amp; ripristino dati che agisce a patto che lo spazio  su cui pogiavano i dati da recuperare "se cancellati per errore e/o volutamente" non sia stato utilizzato e sovrascritto da altri dati.'
layout: post
guid: http://linuax.wordpress.com/?p=562
permalink: /2009/03/20/indagine-recupero-di-dati-cancellati-foremost/
categories:
  - How-to
  - Linux
  - Programmi
  - 'Recovery &amp; Backup'
  - scanner
  - Tips
tags:
  - foremost
  - How-to
  - Linux
  - recovery
  - security
---
Quante e quante volte è capitato, erroneamente, di cancellare particolari file e/o di avere un **_hd_** con dati nascosti che compromettono la visione degli stessi ?

Esistono molteplici tool per l'indagine e recupero dei dati in ambito linux; tra questi sicuramente è bene conoscere: **Foremost**.

Link Ufficiale: **<a href="http://sourceforge.net/projects/foremost/" target="_blank">Foremost</a>**

**Foremost** è un tool di indagine & ripristino dati che agisce a patto che lo _**spazio**_ su cui pogiavano i dati da recuperare "se cancellati per errore e/o volutamente" non sia stato utilizzato e sovrascritto da altri dati.

Come prima cosa chiaramente, preoccupiamoci di installare il tool aggiornato all'ultima release disponibile:

<pre>~$ wget <a id="showfiles_download_file_pkg0_1rel0_1" class="sfx_qalogger_element sfx_qalogger_clickable" href="http://downloads.sourceforge.net/foremost/foremost-1.5.5.tar.gz?use_mirror=switch">foremost-1.5.5.tar.gz</a>

~$ tar zxvf foremost-1.5.5.tar.gz
~$ cd foremost-1.5.5
~$ make
~$ su
Password:
~# make install</pre>

**Sintassi**:

$ foremost \[-v|-V|-h|-T|-Q|-q|-a|-w-d\] \[-t <type>\] \[-s <blocks>\] \[-k <size>\]

\[-b <size>\] \[-c <file>\] \[-o <dir>\] \[-i <file\] ****

  * **-H** Stampa il messaggio d'aiuto ed esce
  * **-V** Stampa le informazioni di copyright ed esce
  * **-v** Modalità Verbose
  * **-q** Modalità Quick. Ricerca l'header solo all'inizio del settore
  * **-i** Legge i file da analizzare nel file passato come parametro
  * **-o** Imposta la directory nella quale saranno salvati i file recuperati
  * **-c** Imposta il file di configurazione da utilizzare
  * **-s** Salta il numero di bytes specificato prima di iniziare la ricerca

**File di configurazione:**  
Il file di configurazione guida il comportamento di foremost durante la ricerca ed è sostanzialmente un elenco delle caratteristiche da ricercare per ogni file. Le righe che iniziano col carattere # sono considerate commenti e non vengono prese in considerazione dal programma. Ogni riga è divisa in sezioni con gli attributi dei file:

<table border="0" cellspacing="0" cellpadding="0" width="100%">
  <col width="43"></col> <col width="43"></col> <col width="43"></col> <col width="43"></col> <col width="43"></col> <col width="43"></col> <tr valign="top">
    <td width="17%">
      <p align="center">
        <strong>extension</strong>
      </p>
    </td>
    
    <td width="17%">
      <p align="center">
        <strong>case sensitive</strong>
      </p>
    </td>
    
    <td width="17%">
      <p align="center">
        <strong>size</strong>
      </p>
    </td>
    
    <td width="17%">
      <p align="center">
        <strong>header</strong>
      </p>
    </td>
    
    <td width="17%">
      <p align="center">
        <strong>footer</strong>
      </p>
    </td>
    
    <td width="17%">
      <p align="center">
        <strong>opzioni</strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td width="17%" valign="top">
      <p align="center">
        </td> 
        
        <td width="17%" valign="top">
          <p align="center">
            </td> 
            
            <td width="17%" valign="bottom">
              <p align="center">
                </td> 
                
                <td width="17%" valign="top">
                  <p align="center">
                    </td> 
                    
                    <td width="17%" valign="top">
                      <p align="center">
                        </td> 
                        
                        <td width="17%" valign="top">
                          <p align="center">
                            </td> </tr> 
                            
                            <tr>
                              <td width="17%" valign="top">
                                <p align="center">
                                  mpg
                                </p>
                              </td>
                              
                              <td width="17%" valign="top">
                                <p align="center">
                                  y
                                </p>
                              </td>
                              
                              <td width="17%">
                                <p align="center">
                                  4000000
                                </p>
                              </td>
                              
                              <td width="17%" valign="top">
                                <p align="center">
                                  x00x00x01xba
                                </p>
                              </td>
                              
                              <td width="17%" valign="top">
                                <p align="center">
                                  x00x00x01xb9
                                </p>
                              </td>
                              
                              <td width="17%" valign="top">
                                <p align="center">
                                  REVERSE
                                </p>
                              </td>
                            </tr>
                            
                            <tr valign="top">
                              <td width="17%">
                                <p align="center">
                                  </td> 
                                  
                                  <td width="17%">
                                    <p align="center">
                                      </td> 
                                      
                                      <td width="17%">
                                        <p align="center">
                                          </td> 
                                          
                                          <td width="17%">
                                            <p align="center">
                                              </td> 
                                              
                                              <td width="17%">
                                                <p align="center">
                                                  </td> 
                                                  
                                                  <td width="17%">
                                                    <p align="center">
                                                      </td> </tr> </tbody> </table> 
                                                      
                                                      <p>
                                                        Foremost nomina numericamente ogni file recuperato, partendo da 00000000 ed aggiungendo l'estensione specificata nel campo extension. E' possibile inserire <strong>NONE</strong> nel suddetto campo per fare in modo che nessuna estensione sia aggiunta al nome del file.
                                                      </p>
                                                      
                                                      <pre><strong>Case sensitive</strong> può essere impostato 'y' o 'n' e riguarda il trattamento di header e footer.</pre>
                                                      
                                                      <pre><strong>Size</strong> rappresenta il massimo numero di byte che foremost recupera se non trova un footer.</pre>
                                                      
                                                      <p>
                                                        L'header ed il footer possono essere specificati con valori esadecimali, ottali o in caratteri, lo spazio è rappresentato da <strong>s</strong>. I valori esadecimali sono rappresentati come x[0-f][0-f], quelli ottali come [0-3][0-7][0-7] . L'esempio riportato nel file di configurazione stesso è il seguente:
                                                      </p>
                                                      
                                                      <p>
                                                        <strong>x4f123IsCCI </strong>è equivalente a “OSI CCI”<br /> Un'utile accorgimento è l'inserimento di una wildcard per stringhe contenenti byte variabili : il carattere utilizzato di default è '<strong>?</strong>' (es. ????????x6dx6fx6fx76) che può essere cambiato modificando la relativa stringa nel file di configurazione.<br /> Il campo footer è l'unico opzionale e, in molti casi, è utile eliminarlo per affidarsi alla dimensione massima specificata.<br /> Oltre a questi parametri esistono due opzioni che possono essere appese alla riga di specifica per plasmare il comportamento di foremost in casi particolari:
                                                      </p>
                                                      
                                                      <ul>
                                                        <li>
                                                          REVERSE – Foremost esegue la scansione dall'header alla dimensione massima specificata, poi ripercorre il percorso a ritroso fino alla prima occorrenza del footer. Utile nei casi di file con più istanze del footer all'interno (i file PDF appartengono a questa categoria, troverete infatti il parametro REVERSE settato di default nel file di configurazione originale)
                                                        </li>
                                                        <li>
                                                          NEXT – La scansione si arresta alla prima occorrenza del footer che è escluso dal file recuperato. In questo modo è possibile concludere il recupero quando è presente una stringa che sappiamo per certo non appartenere alla tipologia del file ricercato ma, è altresì possibile recuperare file dei quali non conosciamo il footer concludendo il recupero ed avviando il successivo alla prima occorrenza dello stesso header.
                                                        </li>
                                                      </ul>
                                                      
                                                      <p>
                                                        Spiegata la sintassi e il conf non resta che recuperare i dati persi.<strong></strong>
                                                      </p>
                                                      
                                                      <p>
                                                        <strong>Uso Comune</strong>:
                                                      </p>
                                                      
                                                      <p>
                                                        <strong>NB</strong>: E' buona norma in caso di <em><strong>partizioni</strong></em> creare un mount point "che potrebbe essere un ennesimo<strong> hd Dati</strong>" e/o un hard disk <strong>usb</strong> esterno e quindi:
                                                      </p>
                                                      
                                                      <p>
                                                        - Supponiamo di voler recuperare i dati dell'hard disk <strong><em>Hda2</em>;</strong> creo la cartella <em><strong>foremost</strong></em> nel disco esterno usb  che chiamerò "<strong>recupero</strong>", grande a sufficienza da contenere tutto <em><strong>hda2</strong></em> "disco/partizione sul quale si tenta la scansione e quindi il recupero di dati":
                                                      </p>
                                                      
                                                      <pre>~# mount /dev/hda3 /mnt/recupero   "disco usb"
~# mkdir  /mnt/recupero/foremost   "creazione punto di montaggio sul disco usb (dove finiranno i dati)"</pre>
                                                      
                                                      <p>
                                                        Creato il punto di montaggio scelto come in esempio, basterà avviare foremost in maniera generica (cercando il recupero di tutti i dati) indicando l'hd su cui verificare la scansione e il punto di montaggio che di default risulta essere altrimenti la directory "<em><strong>/output</strong></em>" :
                                                      </p>
                                                      
                                                      <pre>~# foremost -i /dev/hda2 -o /mnt/recupero/foremost</pre>
                                                      
                                                      <p>
                                                        <strong>NB</strong>: la durata di scansione e recupero varia a seconda della grandezza della partizione e/o dell'hd su cui si lavora.
                                                      </p>
                                                      
                                                      <p>
                                                        A lavoro finito la cartella o il punto di montaggio scelto si presenta come segue:
                                                      </p>
                                                      
                                                      <pre>audit.txt  dll/  gif/  jpg/  mpg/  png/  rif/  sxc/  vis/  xls/
avi/       doc/  htm/  mbd/  ole/  ppt/  sdw/  sxi/  wav/  zip/
bmp/       exe/  jar/  mov/  pdf/  rar/  sx/   sxw/  wmv/</pre>
                                                      
                                                      <p>
                                                        <strong>NB</strong>: Come da esempio, i file sono suddivisi in cartelle per estensione. La nota dolente è che all'interno delle cartelle i file recuperati sono rinominati  numericamente perdendo il nome originario.
                                                      </p>
                                                      
                                                      <p>
                                                        <strong>Rapporto</strong>:
                                                      </p>
                                                      
                                                      <p>
                                                        Nella directory in cui sono immagazzinati i dati recuperati, Foremost crea un file dal nome audit.txt, ovvero un report contenente i dettagli dell'operazione effettuata tra i quali è da evidenziare l'offset di partenza originale del file recuperato ( <strong>Found at Byte</strong>). Questo parametro risulta molto utile per approfondire una prima ricerca di base. Cercando ad esempio alcune immagini create con Photoshop, possiamo impostare l'header in modo che trovi una caratteristica unica dell'immagine ( la stringa AdobesPhotoshop). Sapendo che l'occorrenza dell'header è collocata all'offsett 144 (0x90) del file possiamo recuperare per intero l'immagine calcolando il byte di partenza del file impostandolo come punto di avvio della ricerca con l'opzione <strong>-s</strong>.
                                                      </p>
                                                      
                                                      <p>
                                                        <strong>Uso Specifico per tipi di file</strong>:
                                                      </p>
                                                      
                                                      <p>
                                                        Potrebbe essere utile scansionare la partizione e/o l'hd scelto cercando solo ed esclusivamente determinati tipi di file:
                                                      </p>
                                                      
                                                      <pre>~# foremost -t jpg -i /dev/hda2 -o /mnt/recupero/foremost</pre>
                                                      
                                                      <p>
                                                        <strong>NB</strong>: Come da esempio l'estensione è configurabile con il tipo di file che si intende recuperare <em><strong>Gif, Avi, Zip, Rar, Doc, Html</strong></em> etc ...
                                                      </p>
                                                      
                                                      <p>
                                                        Le possibilità di indagine e ripristino con Foremost sono varie, e anche se può risultare non troppo comodo permette di creare file di configurazione vari per i vari tipi di file che si intende ricercare fuori dal contesto della ricerca default; ricerca che di suo risulta essere davvero semplice all'user comune.
                                                      </p>
                                                      
                                                      <p>
                                                        Riferimenti: <a href="http://www.cybercrimes.it/" target="_blank"><strong>Cybercrimes</strong></a>
                                                      </p>
                                                      
                                                      <p>
                                                        #
                                                      </p>