---
id: 721
title: 'Trasferimento di file cel Nokia su Slackware Obexftp-Fronted'
date: 2009-05-18T06:00:02+01:00
author: ax
excerpt: ObexFtp è un applicativo che ci consente di trasferire file dal nostro cellulare Nokia al pc e viceversa, in linux, come avviene per un normale scambio di file tramite applicativi che sfruttano transazioni FTP. Grazie ad udev, obexftp ed hai frontend risulta estremamente facile e veloce, il tutto maggiormente supportato dalla semplicità della grafica... spazio a ObexFTP-frontend.
layout: post
guid: http://linuax.wordpress.com/?p=721
permalink: /2009/05/18/trasferimento-di-file-e-collegamento-del-cellulare-nokia-su-slackware-obexftp-fronted/
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
  - 'a:0:{}'
image_url:
  - http://linuax.wordpress.com/files/2009/05/obextool.png
  - http://linuax.wordpress.com/files/2009/05/obextool.png
  - http://linuax.wordpress.com/files/2009/05/obextool.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/05/obextool.png" class="alignnone size-full wp-image-724" title="obextool"   alt="obextool"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/05/obextool.png" class="alignnone size-full wp-image-724" title="obextool"   alt="obextool"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/05/obextool.png" class="alignnone size-full wp-image-724" title="obextool"   alt="obextool"    />'
image_size:
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"631";i:2;s:1:"3";i:3;s:24:"width="800" height="631"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"631";i:2;s:1:"3";i:3;s:24:"width="800" height="631"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"631";i:2;s:1:"3";i:3;s:24:"width="800" height="631"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"631";i:2;s:1:"3";i:3;s:24:"width="800" height="631"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"631";i:2;s:1:"3";i:3;s:24:"width="800" height="631"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"800";i:1;s:3:"631";i:2;s:1:"3";i:3;s:24:"width="800" height="631"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
categories:
  - How-to
  - Linux
  - Slackware
tags:
  - Linux
  - nokia
  - obexftp
  - obextools
  - slackware
  - Tips
---
Per una serie di ragioni (utenti,gruppi,permessi,device), il collegamento di cellulari _**nokia**_ sia via **USB** che **Bluetooth** su slackware può creare una serie di problematiche. Per questa ragione in questo how-to vedremo un esempio di collegamento corretto via **USB** per trasferire file tra il cellulare e un sistema Gnu/Linux, nello specifico su Slackware.

Per prima cosa è bene essere certi di avere nel sistema l'utility di base che permetterà lo scambio di file tra cellulare e pc come se stessimo usando un comune applicativo **ftp** per lo scambio di file.

L'applicativo in questione è **_Obextool (grafico)_** che comprende **_ObexFtp_** _**(testuale)**_ con relative **_openobex-apps_**_.  
_ 

**NB**: l'how-to è scritto per Slackware 12.1 e superiori, in cui il pacchetto _openobex-apps_ e anche _obexftp_ è già presente. Se per una ragione strana non dovesse essere così sarà facimente reperibile dal web e nel caso specifico per Slackware anche già pacchettizato per l'uso.

Come primo passo quindi, accertata la presenza di **_obexftp_** e relative dipendenze ci assicureremo che il kernel riconosca il cellulare come un normale dispositivo di scambio di file in **USB**.

**NB**: ai fini di una buona riuscita bisogna considerare che tutto il procedimento si basa sul demone  _**udev**_ . Se non fosse stato abilitato di default all'avvio di _**Slackware**_ sarà bene dapprima verificare la sua presenza e in caso contrario startarlo da user root:

<pre>~# ps x | grep udev

1002 ?        S&lt;s    0:02 /sbin/udevd --daemon</pre>

se non restituisce il processo provvederemo a startarlo:

<pre>~# /etc/rc.d/rc.udev start</pre>

Quindi colleghiamo  il cavetto **USB** da utente semplice e digitiamo:

<pre>~$ dmesg</pre>

che dovrebbe restituire un output simile al seguente:

> <pre>usb 4-1: new full speed USB device using uhci_hcd and address 2
usb 4-1: configuration #1 chosen from 1 choice
cdc_acm 4-1:1.8: ttyACM0: USB ACM device
usbcore: registered new interface driver cdc_acm
drivers/usb/class/cdc-acm.c: v0.25:USB Abstract Control Model driver for USB modems and ISDN adapters</pre>

Accertato che il cellulare **Nokia** sia stato riconosciuto come dispositivo **USB** provvederemo  a riconoscere il device di appartenenza:

<pre>~$ /sbin/lsusb</pre>

> <pre>Bus 005 Device 001: ID 0000:0000  
Bus 004 Device 002: ID 0421:043a Nokia Mobile Phones N70 USB Phone Parent
Bus 004 Device 001: ID 0000:0000  
Bus 003 Device 001: ID 0000:0000  
Bus 002 Device 001: ID 0000:0000  
Bus 001 Device 003: ID 0c45:602c Microdia Clas Ohlson TWC-30XOP WebCam
Bus 001 Device 001: ID 0000:0000</pre>

Accertato che il cellulare è montato sul **Device 002** identificato da udev come _**dev ttyACM0**_ , verificheremo come buona norma insegna da utente root quali sono i permessi e il gruppo assegnati di default dal sistema  a tale device. Tale parsimonia potrebbe tornarci utile in seguito.

<pre>~#  ls -al /dev/ttyACM0
crw-rw---- 1 root uucp 166, 0 2009-05-17 22:35 /dev/ttyACM0</pre>

Verificati i permessi  come _**utente root**_ appartenente al gruppo _**uucp**_ ed identificato il device, dovremo dire ad **udev** quale dispositivo è collegato. Per farlo dovremo munirci di due parametri: dell' IdProduct e dell'IdVendor del cellulare nokia che intendiamo collegare.

I due parametri sono riconosciuti dal sistema già dal precedente comando "_**/sbin/lsusb**_" che ci ha restituito:

<pre>0421:043a</pre>

ma per essere sicuri di non sbagliare e confonderne uno con l'altro:

<pre>~#  lsusb -v | grep idVendor                                     
 idVendor           0x0000
 idVendor           0x<span style="text-decoration:underline;"><span style="color:#ff0000;"><strong>0421</strong></span></span> Nokia Mobile Phones
 idVendor           0x0000
 idVendor           0x0000
 idVendor           0x0000
 idVendor           0x0c45 Microdia
 idVendor           0x0000
~#  lsusb -v | grep idProduct                                    
 idProduct          0x0000
 idProduct          0x<strong><span style="text-decoration:underline;"><span style="color:#ff0000;">043a</span></span></strong> N70 USB Phone Parent
 idProduct          0x0000
 idProduct          0x0000
 idProduct          0x0000
 idProduct          0x602c Clas Ohlson TWC-30XOP WebCam
 idProduct          0x0000</pre>

<span style="color:#000000;">Segnamo dove preferiamo i due parametri sottolineati in rosso. Trovati i due parametri dovremmo dire ad udev che una volta collegato il dispositivo non dovranno esserci problemi di permessi. </span>

**NB**: in casi generali _**udev**_ permetterà di vedere il dispositivo e quindi la transazione FTP solo all'utente **root**. Per evitare questo, e far in modo che il normale **user** posso effettuare le transazioni da pc a cellulare e viceversa aggiungeremo una regola ad _udev_ nella directory **_/etc/udev/rules.d/_** con il nostro editor preferito, essendo già da operazioni precedenti muniti dei parametri _**id"Vendor & Product"**_ del cellulare come segue.

<pre>~# vi /etc/udev/rules.d/80-nokia.rules
ACTION!="add", GOTO="nokia_rules_end"
#SUBSYSTEM!="usb_device", GOTO="nokia_rules_end"

LABEL="nokia_rules_start"

ATTR{idVendor}=="0421", ATTR{idProduct}=="043a", OWNER="root", GROUP="users", MODE="0660"

LABEL="nokia_rules_end"</pre>

**NB**: in tutte le altre distribuzioni l'aggiunta della regola udev per identificare il dispositivo prevede come parametro **GROUP=""** l'appartenenza al gruppo _**dialout**_.  Date le differenze di presenza del gruppo in Slackware utilizzeremo semplicemente il gruppo **users**.

> **Nota da non sottovalutare**: é buona norma essere certi che il proprio utente sia appartenente ai gruppi generici di sfruttamento di periferiche che in casi generali dovrebbero risultare come i seguenti:
> 
> <pre>~ $  id                                                                    
uid=1000(...) gid=100(users) gruppi=5(tty),7(lp),11(floppy),17(audio),18(video),19(cdrom),81(messagebus),82(haldaemon),83(plugdev),93(scanner),100(users)</pre>
> 
> In caso contrario provvederemo ad aggiungere il nostro utente ai gruppi citati sopra e a refreshare l'aggiunta dell'utente ai gruppi citati:
> 
> <pre>~# usermod -G floppy,audio,video,cdrom,haldaemon,plugdev,scanner,tty,uucp mio_utente
~# newgrp nostro_gruppo_di_appartenenza_utente            
esempio:
~# newgrp users</pre>

Una volta salvato il file con la regola _udev_ e il gruppo **_users_** abilitato alle transazioni con _**obexftp**_ provvederemo a staccare il cavetto _**usb**_ e restartare **udev**.

<pre>~# /etc/rc.d/rc.udev restart</pre>

Restartato il demone udev, riattaccheremo il cavetto **USB** e verificheremo la connessione tra Slackware e il nostro amato Nokia da terminale  come segue:

<pre>~$  obex_test -u
Using USB transport, querying available interfaces
Interface 0: Nokia Nokia N70 SYNCML-SYNC
<strong><span style="text-decoration:underline;"><span style="color:#ff0000;">Interface 1</span></span></strong>: Nokia Nokia N70 <strong><span style="color:#ff0000;"><span style="text-decoration:underline;">PC Suite Services</span></span></strong>
Use 'obex_test -u interface_number' to run interactive OBEX test client</pre>

Verificato il riconoscimento da obex verificheremo se la transazione da utente via terminale è gi à disponibile:

<pre>~$  obexftp -u 1 -c C: -l
If USB doesn't work setup permissions in udev or run as superuser.
Connecting...done
Sending "C:"... done
Receiving "(null)"... &lt;?xml version="1.0"?&gt;
&lt;!DOCTYPE folder-listing SYSTEM "obex-folder-listing.dtd"
 [ &lt;!ATTLIST folder mem-type CDATA #IMPLIED&gt;
 &lt;!ATTLIST folder label CDATA #IMPLIED&gt; ]&gt;
&lt;folder-listing version="1.0"&gt;
 &lt;parent-folder /&gt;
 &lt;folder name="cache" modified="20070821T120632Z" user-perm="RWD" mem-type="DEV"/&gt;
 &lt;folder name="Data" modified="20081118T024304Z" user-perm="RWD" mem-type="DEV"/&gt;
 &lt;folder name="Nokia" modified="20060101T020012Z" user-perm="RWD" mem-type="DEV"/&gt;
 &lt;folder name="Notepad" modified="20081113T010836Z" user-perm="RWD" mem-type="DEV"/&gt;
 &lt;file name="BlasterSettings" size="73" modified="20070919T140206Z" user-perm="RWD"/&gt;
 &lt;file name="CellTrack" size="47" modified="20070914T215856Z" user-perm="RWD"/&gt;
 &lt;file name="DSPROFILEEDITOR.EXE" size="3881" modified="20070822T095218Z" user-perm="RWD"/&gt;
 &lt;file name="MP" size="47" modified="20070920T222404Z" user-perm="RWD"/&gt;
 &lt;file name="PDPhotoLibCache.txt" size="16" modified="20090509T213150Z" user-perm="RWD"/&gt;
 &lt;file name="Photo Express M-Style" size="42" modified="20070904T160312Z" user-perm="RWD"/&gt;
 &lt;file name="PowerMP3" size="46" modified="20070902T191346Z" user-perm="RWD"/&gt;
&lt;/folder-listing&gt;done
Disconnecting...done</pre>

**NB**: la prima stringa _"**If USB doesn't work setup permissions in udev or run as superuser.**"_ è un messaggio di warning_,_ che normalmente deve restituire **udev**. Non pregiudica il funzionamento e il trasferimento dei file tra i dispositivi.

Finalmente vedremo il contenuto della memoria del nostro cellulare **Nokia** e da questo momento potremo effettuare via **obexftp** le transazioni come se stessimo usando un normale applicativo **FTP**.

Come da esempio seguente utilizzeremo:

> <pre>-p (PUT) per l'upload
-g (GET) per il download



<div class="codecolorer-container text dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="text codecolorer">
    $ obexftp -u 1 -c 'E:/muvee' -p esempio.3gp<br />
    $ obexftp -u 1 -c 'C:/Nokia/Sounds/Digital' -g 'Sound.amr'
  </div>
</div>

</pre>

Chiaramente può essere utile nel caso del trasferimento di una gran mole di file, utilizzare un interfaccia grafica che eviti di farci digitare perennemente e/o per 10 minuti di fila, sempre gli stessi comandi. Le possibilità al momento risultano essere due.

Utilizzare come da citazione ad inizio pagina **Obextool** (un interfaccia grafica di _**openobex in cui è nativo "ObexFTP"**_ ) oppure utilizzare **ObexFtp-Frontend** ( in _**java**_).  Io personalmente li ho utilizzati entrambi e preferisco il secondo se pur in java e quindi un pizzico più pesante del primo che però a differenza di questo implementa funzionalità comode. Per parcondicio quindi, illustrerò entrambe le possibilità.

 **IMPLEMENTAZIONE DELL'INTERFACCIA GRAFICA AL COLLEGAMENTO "CEL - PC" TRAMITE OBEXFTP.**

**Frontend via ObexTool:**

Scarichiamo <a href="http://www.tech-edv.co.at/programmierung/en/gplsw.html#download" target="_blank"><strong>Obextool.</strong></a>

Scaricato; lo scompatteremo dove preferiamo, supponiamo la dir : /home/mio_utente/obextool rinominata da noi e modificheremo tre direttive di percorso ed eseguibili predefinite come da esempio seguente. Quindi riassumendo:

<pre>~$ wget <a href="http://www.tech-edv.co.at/downloads/obextool-0.35.tar.gz" target="_blank">obextool</a>
~$ tar zxvf obextool-0.35.tar.gz
~$ rm obextool-0.35.tar.gz
~$ mv obextool-0.35/ obextool
~$ cd obextool</pre>

Poi modficheremo i seguenti file come segue:

**1)** 

<pre>~$ vi ~/obextool/etc/obexwrap.sh</pre>

sostituendo l'ultima riga presente

<pre>obexftp -b 00:1A:75:59:62:62 -B 7 "$@"</pre>

**NB**:  varia a seconda della versione di obextool in uso, ma la modifica è la stessa.

in:

<pre>obexftp -u 1 "$@"
<strong>2) </strong>
~$ vi ~/obextool/etc/obextool.cfg</pre>

sostituendo il valore della stringa successiva se risulta essere ad **1** (riferita a telefonini Siemens) spostandola a **** (per cellulari Nokia):

**NB**: questa modifica è da fare nelle vecchie versioni di obextool, l'ultima disponibile non necessita di questa modifica essendo già sul valore 0.

<pre>set ObexConfig(config,memstatus) 1</pre>

in:

<pre>set ObexConfig(config,memstatus) 0</pre>

**3) Obextool** richiede <a href="http://www.nemethi.de/" target="_blank"><strong>Tablelist</strong> </a>che andrà scompattato nella directory di obextool. Scaricato ed estratto tablelist andremo a modificare l'ultimo file in base alla versione scaricata di **Tablelist** dal sito ufficiale come da esempio**:**

<pre>~$ cd ~/obextool/
~$ wget <a href="http://www.nemethi.de/tablelist/tablelist4.11.tar.gz" target="_blank">Tablelist</a>
~$ tar zxvf tablelist4.11.tar.gz
~$ ls
contrib/  doc/  etc/  images/  lang/  lib/  obextool.tk*  plugins/  tablelist4.11/
~$ rm tablelist4.11.tar.gz</pre>

Scaricate le tablelist nella directory **obextool** creeremo il file _**startup-obextool.sh**_ definito in base al percorso di destinazione delle tablelist che abbiamo scaricato come da esempio:

<pre>~$ cd ~/obextool/
~$ vi startup-obextool.sh
#!/bin/sh
TCLLIBPATH=~/obextool/tablelist4.11/ wish ~/obextool/obextool.tk</pre>

Salviamo e gli diamo i permessi di esecuzione:

<pre>~$ su -
~# chmod +x startup-obextool.sh
~# exit</pre>

Effettuate le modifiche e creato il  file di start avviamo il Frontend Obextool.tk  presente in home:

<pre>~$ sh ~/obextool/startup-obextool.sh

<img class="alignnone size-full wp-image-724" title="obextool" src="http://linuax.files.wordpress.com/2009/05/obextool.png" alt="obextool" width="455" height="358" /></pre>

**Frontend via <a href="http://sourceforge.net/projects/obexftpfrontend/" target="_blank">Obexftp-Frontend</a> in java.**

Il procedimento è nettamente più semplice del precedente, dato che il frontend è in formato **binario** e quindi è sufficiente scaricarlo ed avviarlo:

<pre>~$ wget <a href="http://garr.dl.sourceforge.net/sourceforge/obexftpfrontend/obexftp-frontend-0.6.6-bin.zip" target="_blank">Obexftp-Frontend</a>
~$ unzip obexftp-frontend-0.6.6-bin.zip
~$ cd obexftp-frontend-0.6.6-bin
~$ sh obexftp-frontend</pre>

Avviato il Frontend bisognerà modificare alcuni semplici parametri nella sezione in alto

<pre>F12 Options =&gt; Configuration</pre>

e modificare:

<pre><strong>ObexFTP path</strong> inserendo il percorso =&gt;  <em><strong>/usr/bin/obexftp</strong></em>
<strong>Connection type</strong> =&gt; selezionare <strong>USB</strong>
<strong>Connection line</strong> =&gt; <strong>1</strong></pre>

**NB**: Per un corretto funzionamento bisogna spuntare l'opzione **"Device info fetching"** disattivando l'opzione nel caso di connessione **USB**.

<pre><img class="alignnone size-full wp-image-725" title="obexftp-frontend" src="http://linuax.files.wordpress.com/2009/05/obexftp-frontend.jpg" alt="obexftp-frontend" width="455" height="341" /></pre>

\# End
