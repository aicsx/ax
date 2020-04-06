---
id: 783
title: Compilazione nuovo Kernel 2.6.x
date: 2009-08-03T09:00:18+01:00
author: ax
excerpt: Direttive principali ed esempio di compilazione di un nuovo kernel serie 2.6.x partendo dal source ufficiale.
layout: post
guid: http://linuax.wordpress.com/?p=783
permalink: /2009/08/03/compilazione-nuovo-kernel-2-6-x/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Kernel
  - Linux
  - Slackware
tags:
  - How-to
  - kernel
  - Linux
  - slackware
---
Di seguito sono elencati con una breve spiegazione i passi necessari per la compilazione di base di un nuovo kernel serie 2.6.x , nello specifico, la distro di riferimento è  Slackware.

**NB**: Indifferentemente dalla distro, i comandi utili risultano essere sempre gli stessi, come da esempio seguente.

I vari kernel sono disponibili dal <a href="http://www.kernel.org/" target="_blank">Link Ufficiale</a> nel formato che si desidera.

Scarichiamo il kernel scelto:

<pre>~$ wget <a href="http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.30.tar.bz2">linux-2.6.30.tar.bz2</a></pre>

Spostiamo i sorgenti nella directory di riferimento generica per i sorgenti del kernel **/usr/src/ :  
** 

<pre>~$ su
Password:
~# cp linux-2.6.30.tar.bz2 /usr/src/</pre>

Ci spostiamo nella directory di riferimento e decomprimiamo il file contenente il kernel scelto:

<pre>~# cp /usr/src/
~# tar jxvf linux-2.6.30.tar.bz2</pre>

Estratto il kernel avremo la directory /usr/src/linux-2.6.versione. Provvederemo per comodità a cancellare e ricreare il link simbolico puntandolo alla versione prescelta:

<pre>~# rm linux ; ln -s linux-2.6.x linux</pre>

**NB**: la "_**x**_" chiaramente indica la versione (18-19-20-21...30) che cambierà in base al kernel scelto.

Creato il link prepariamo l'ambiente:

<pre>~# cd /usr/src/linux
~# make mrproper</pre>

**NB**: make mrproper pulità l'albero dei sorgenti del kernel da qualunque file di configurazione. E' buona norma prima di startare la compilazione del kernerl ricordarsi di questo comando.

A questo punto dobbiamo cominciare scegliendo cosa abilitare e cosa no nel nostro nuovo kernel. Questa fase viene fatta richiamando un menù di opzioni che registrerà i cambiamenti fatti da noi su un file _**.config**_ . Prima di questo però è possibile scegliere la configurazione partendo da 0 "default" oppure utilizzando un file di config preesistente del kernel che stiamo usando attualmente.

**NB**: è buona norma scegliere di utilizzare un _**pre**_ **.config** per assicurarsi che cmq, anche in casi particolari, siano avviati i moduli necessari con il kernel preesistente. Questo comportamento ci garantirà salvo casi "spiacevoli" l'avvio del riconoscimento hardware e di tutti i servizzi necessari per poter tornare a riportare modifiche. E' chiaro che in caso di kernel panic, l'alternativa sarà quella obbligata del chroot di ripristino (articolo presente e consultabile nel blog).

Nel caso _default_ partendo da 0 saltate il prossimo punto, altrimenti, assodato che utilizzeremo un config esistente a cui si aggiungeranno in ogni caso le modifiche fatte da noi procediamo copiandolo nella directory del kernel source:

<pre>~# cp /boot/config-generic-2.6.24.5 /usr/src/linux/.config
~# make oldconfig</pre>

**NB**: questo comando si occuperà di settare le opzioni presenti nel config preesistente che era allocato nella directory **/boot/config-versione** e di applicarle al nuovo kernel.

Finita la fase 1, sceglieremo di startare l'interfaccia testuale o grafica scelta per controllare le opzioni da abilitare e/o disabilitare in maniera Statica,Dinamica e/o modulare. Si può scegliere il _**make**_ in metodo testuale tramite **menuconfig** oppure il metodo grafico mediante  **xconfig** e/o **gconfig**. La differenza sostanziale è che menuconfig usa ncurses, mentre xconfig e gconfig richiedono l'uso di X (server grafico) ed usano rispettivamente le librerie QT e GTK.

Supponiamo di scegliere il metodo classico da shell:

<pre>~# make menuconfig</pre>

Abiliteremo e disabiliteremo i moduli prescelti mediante Y-N oppure in base alle esigenze sceglieremo di applicare a delle opzioni il caricamento modulare M , che richiamerà il modulo solo in caso di necessità. Completata la scelta di cosa abilitare e cosa no, usciamo <EXIT> e salviamo la configurazione.

A questo punto compiliamo:

<pre>~# make</pre>

**NB**: Si può scegliere di passare l'opzione **-j2** e/o  **-j3** al **make** . Questa opzione non fà altro che lanciare un numero di processi espresso numericamente in parallelo. In sostanza velocizza il processo di compilazione e creazione dell'immagine kernel. Questo avviene perchè normalmente un singolo processo di "make" non dovrebbe occupare tutta la CPU, per evitare di sovraccaricare la macchina. Lanciando invece 2 o 3 processi assieme la CPU risulterà interamente occupata con carico 100% e il tempo di compilazione diminuirà.

Dopo un tempo indefinito, che solitamente dipende dalla macchina che si sta utilizzando, l'immagine kernel sarà stata creata e si troverà nella directory _**/usr/src/linux/arch/i386/boot/**_ con il nome **bzImage**.

Creata l'immagine che ci servirà dopo, procederemo con l'installazione dei moduli annessi all'immagine creata:

<pre>~# make modules_install</pre>

**NB**: il comando provvede ad installare i moduli scelti nella directory _**/lib/modules/versione-kernel**_.

Installati anche i moduli il processo è terminato. I file di riferimento adesso sono tre:

<pre>1) /usr/src/linux/arch/i386/boot/bzImage  =&gt; l'immagine kernel creata
2) /usr/src/linux/System.map              =&gt; Mappatura locazioni di memoria del kernel
3) /usr/src/linux/.config                 =&gt; config relativo al kernel compilato</pre>

Non dobbiamo fare altro che spostare i file  e l'immagine kernel nella directory di sistema dedicata all'avvio: **/boot/** ,tenendo presente che non ci siamo mai spostati dalla directory **/usr/src/linux/** :

<pre>~# cp arch/i386/boot/bzImage /boot/vmlinuz-nome_che_preferiamo_dare_al_nostro_kernel
~# cp System.map /boot/System.map-versione_compilata
~# cp .config /boot/config_versione_compilata</pre>

**NB**: i nomi sono chiaramente una scelta soggettiva dell'utente. Il file immagine è necessario per l'avvio al boot del nuovo kernel. Il config è da conservare per una ricompilazione successiva. Il file System.map è da conservare in caso di errata mappatura della locazione di memoria kernel.

Ultimo passo è la configurazione del boot manager "LILO - GRUB" con le direttive della nuova immagine del kernel.

<pre>~# vi /etc/lilo.conf</pre>

modifichiamo o creiamo la nuova voce boot in lilo come da esempio:

<pre># Image kernel 2.6.x
image = /boot/vmlinuz-nome_dato_precedentemente_allimmagine_kernel       =&gt; nome e percorso dell'immagine kernel
root = "/dev/hdaX"                                                       =&gt; device corrispondente alla partizione radice /
label = "Kernel2.6.x"                                                    =&gt; nome che apparirà al boot lilo
vga = 791                                                                =&gt; modalità di framebuffer della console scelto
read-only
# Image kernel 2.6.x end</pre>

Riavviamo lilo in verbose per salvare le modifiche e controllare  gli eventuali errori:

<pre>~# lilo -v</pre>

Riavviamo la macchina e bootiamo con il nuovo kernel.

\# End