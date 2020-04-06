---
id: 793
title: 'Patch di sicurezza per kernel linux: Grsecurity " PaX'
date: 2009-08-13T19:21:11+01:00
author: ax
excerpt: |
  <p>Guida all'implementazione e abilitazione della patch Grsecurity su kernel linux e alla ricompilazione successiva dello stesso. Breve spiegazione delle migliorie di sicurezza che si ottengono applicando la patch e abilitando Pax.</p>
layout: post
guid: http://linuax.wordpress.com/?p=793
permalink: /2009/08/13/patch-di-sicurezza-per-kernel-linux-grsecurity-pax/
image_url:
  - http://linuax.wordpress.com/files/2009/08/screenshot_menuconfig_gr.png
  - http://linuax.wordpress.com/files/2009/08/screenshot_menuconfig_gr.png
image_tag:
  - '<img src="http://linuax.wordpress.com/files/2009/08/screenshot_menuconfig_gr.png" class="alignnone size-full wp-image-794" title="screenshot_menuconfig_gr"   alt="screenshot_menuconfig_gr"    />'
  - '<img src="http://linuax.wordpress.com/files/2009/08/screenshot_menuconfig_gr.png" class="alignnone size-full wp-image-794" title="screenshot_menuconfig_gr"   alt="screenshot_menuconfig_gr"    />'
image_colors_bg:
  - 'a:0:{}'
  - 'a:0:{}'
image_colors_fg:
  - 'a:0:{}'
  - 'a:0:{}'
image_size:
  - 'a:6:{i:0;s:3:"700";i:1;s:3:"424";i:2;s:1:"3";i:3;s:24:"width="700" height="424"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"700";i:1;s:3:"424";i:2;s:1:"3";i:3;s:24:"width="700" height="424"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"700";i:1;s:3:"424";i:2;s:1:"3";i:3;s:24:"width="700" height="424"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
  - 'a:6:{i:0;s:3:"700";i:1;s:3:"424";i:2;s:1:"3";i:3;s:24:"width="700" height="424"";s:4:"bits";s:1:"8";s:4:"mime";s:9:"image/png";}'
categories:
  - How-to
  - Kernel
  - Linux
  - Sicurezza
  - Slackware
tags:
  - grsecurity
  - How-to
  - kernel
  - Linux
  - patch
  - pax
  - process
  - security
  - slackware
---
La grandezza dei sistemi GNU/Linux è quella di permettere agli utenti stessi di creare volendo e/o apportare le modifiche ricercate al cuore dei sistemi linux; il kernel.

Le migliorie di sicurezza molto spesso sono implementate in versioni di kernel già pronte per alcune distribuzioni linux e distribuite nei formati riconosciuti dalle stesse. Siccome però niente è definitivo e tutto è modificabile, vediamo assieme come rendere il nostro sistema linux più sicuro applicando una delle più diffuse patch di sicurezza ad un kernel vanilla _"ufficiale"._ 

Si sta parlando del progetto: **<a href="http://www.grsecurity.net/" target="_blank">Grsecurity</a>**

**NB**: L'esempio si riferisce ad un kernel source serie 2.6.24.5 ; predefinito della distibuzione Slackware 12.1 . Chiaramente in base alla versione di riferimento del kernel indipendentemente dalla distribuzione e in base alla disponibilità della patch dal sito ufficiale **Grsecurity** è possibile ripetere l'identica procedura.

Vediamo in prima fase come applicare la patch al kernel, analizzando, le varie funzionalità.

Scarichiamo la patch per la versione del kernel di riferimento e la estraiamo:

<pre>~$ wget http://grsecurity.net/grsecurity-2.1.11-2.6.24.5-200804211829.patch.gz
~$ gunzip grsecurity-2.1.11-2.6.24.5-200804211829.patch.gz</pre>

Copiamo la patch e ci spostiamo dove risiedono i source del kernel:

<pre>~$ su
Password:
~# cp grsecurity-2.1.11-2.6.24.5-200804211829.patch /usr/src/linux
~# cd /usr/src/linux</pre>

Puliamo l'albero dei collegamenti del kernel:

<pre>~# make mrproper</pre>

<span style="color: #000000;"><strong>NB</strong>: Volendo possiamo scegliere di partire da un config preesistente, che solitamente è salvato nella directory <strong>/boot</strong>. Se non è necessario saltate le stringhe in verde.</span>

<pre><strong><span style="color: #99cc00;">~# cp /boot/config_versione_scelta /usr/src/linux/.config</span></strong>
<strong><span style="color: #99cc00;">~# make oldconfig</span></strong></pre>

Applichiamo la patch:

<pre>~# patch -p1 &lt; grsecurity-2.1.11-2.6.24.5-200804211829.patch</pre>

A questo punto lanciamo il comando per scegliere le opzioni da abilitare, dando un accenno alle differenze di configurazione.

<pre>~# make menuconfig</pre>

**NB**: Si può scegliere xconfig e/o gconfig. Per dubbi  consultare i post: compilazione e ricompilazione kernel 2.6.x nel blog sezione kernel.

**NB**: La traduzione delle voci del kernel è relativa alla patch e/o localizzazione tramite catalogo. Per info e dubbi consultare il post <a href="http://linuax.wordpress.com/2009/08/02/traduzione-e-localizzazione-italiana-del-kernel-linux/" target="_blank"><strong>traduzione e localizzazione italiana del kernel linux</strong></a> nel blog.

Dal menu principale ci spostiamo nella sezione che ci interessa, ossia: **Opzioni di Sicurezza** ****

All'interno saranno presenti le categorie: **Grsecurity & Pax**

&nbsp;

**NB**:  Abilitando **Grsecurity** si richiamerà automaticamente l'abilitazione di Pax. Ci spostiamo quindi nell'apposita sezione senza considerare **Pax** per il momento.

&nbsp;

L'abilitazione di **Grsecurity** in maniera statica ci presenterà al menu molte opzioni. La configurazione di tali opzioni avviene scegliendo il _**Livello di Sicurezza**_ _"Security Level"_. Tale livello che in maniera predefinita è impostato su _Low (basso)_ può essere impostato manualmente su:

**Basso - Medio - Alto e/o Custom**

**NB**: Come è facile intuire il livello **Custom** permette di scegliere le opzioni da _**abilitare/disabilitare**_ in maniera manuale. In alternativa scegliendo uno dei livelli di sicurezza preimpostati si attivano le seguenti caratteristiche di sicurezza. Cerchiamo di capire le voci in dettaglio.

**Livello Basso** _"Low"_ , attiva le seguenti caratteristiche:

> <pre>linking restrictions
fifo restrictions
random pids enforcing nproc on execve()
restricted dmesg
random ip ids
enforced chdir(”/”) on chroot</pre>

**NB**: Questo tipo di settaggio consene di abilitare le funzioni di sicurezza standard, ed è compatibile con la maggior parte del software in uso. Può essere la soluzione migliore di compatibilità quando si usano software _particolari_ o magari un pò datati. Garantisce in ogni caso un livello minimo aggiuntivo di sicurezza a quello standard del kernel.

**Livello Medio** _"Medium"_, attiva le seguenti caratteristiche:

> <pre>random tcp source ports
failed fork logging
time change logging
signal logging
deny mounts in chroot
deny double chrooting
deny sysctl writes in chroot
deny mknod in chroot
deny access to abstract AF_UNIX sockets out of chroot
deny pivot_root in chroot
denied writes of /dev/kmem, /dev/mem, and /dev/port
/proc restrictions with special gid set to 10 (generalmente wheel)
address space layout randomization
removal of addresses from /proc//[maps|stat]</pre>

**NB**:  Questo settaggio aggiunge le funzione elencate a quelle del livello basso precedente. Garantisce quindi un notevole livello di sicurezza; **medio-alto**. Può però causare problemi di non compatibilità con software datati o _particolari_. Se si sceglie di attivare questo livello di sicurezza bisogna essere certi che sia attivo il servizio di autenticazione_ **"identd"**_ sul sistema in uso e che sia settato a **gid 10** _(corrispondente normalmente al gruppo wheel)_.

**Livello Alto** _"High"_, attiva le seguenti caratteristiche:

> <pre>additional /proc restrictions
chmod restrictions in chroot
no signals, ptrace, or viewing processes outside of chroot capability restrictions in chroot
deny fchdir out of chroot
priority restrictions in chroot
segmentation-based implementation of PaX
mprotect restrictions
kernel stack randomization
mount/unmount/remount logging
kernel symbol hiding</pre>

**NB**: Questa scelta aggiunge le funzioni elencate a quelle dei precedenti livelli. E' il massimo in quanto a sicurezza con la patch **Grsecurity**. Il livello **Alto** in più, consente l'abilitazione di **Pax** e delle sue opzioni. Bisogna però prestare attenzione al livello di compatibilità con software _particolari_ e/o datati. In alcuni casi infatti le restrizioni dettate da questo livello di sicurezza potrebbero causare particolari problemi di compatibilità. Per il corretto funzionamento e per ridurre le restrizioni di incompatibilità software sono valide ( è quindi consigliato attivare) le stesse opzioni di sistema del livello medio: **Servizio identd avviato sotto gid 10**.

**Cos'è Pax ?** 

PaX introduce una coppia di meccanismi di sicurezza che rende difficile per degli attaccanti sfruttare bug che coinvolgano la corruzione della memoria. Solitamente le tecniche comune di exploit risultano essere tre:

  1. introdurre/eseguire codice arbitrario
  2. eseguire codice esistente al di fuori del normale flusso di esecuzione del programma originale
  3. eseguire codice esistente nel normale flusso di esecuzione del programma originale con dati arbitrari

I metodi di prevenzione di **PaX** impediscono che il codice eseguibile possa essere memorizzato in aree di memoria scrivibile.

Il primo metodo di prevenzione di Pax, chiamato **NOEXEC**, ha come scopo quello di fornire del controllo sulla generazione a runtime di codice eseguibile. Le pagine di memoria che non contengono codice eseguibile vengono marcate come non-eseguibili. Questo significa che heap e stack, che contengono solo dati e non dovrebbero contenere codice eseguibile, sono marcati come non-eseguibili. Gli exploit che inseriscono del codice in questa area con l'intento di mandarlo in esecuzione falliranno.

Il secondo metodo di prevenzione di Pax, chiamato **ASLR** (Address Space Layout Randomization), rende casuali gli indirizzi dati alle richieste di memoria. Mentre precedentemente la memoria veniva assegnata sequenzialmente (il che significa che gli exploit sanno dove le regioni di memoria di un processo sono situate) ASLR rende casuali questa allocazione, rendendo vane le tecniche che contano su queste informazione.

**NB**: Il Livello **Custom** può essere combinato ad uno dei **tre** livelli descritti. E' possibile infatti partire da una scelta di uno dei tre livelli preconfigurati e successivamente scegliere il livello **Custom**. In questo modo le opzioni dettate da uno dei tre livelli di sicurezza rimarranno attive. Successivamente la scelta del livello custom ci consentirà di apportare modifiche ai moduli. E' chiaro che le modifiche di selezione dei moduli da attivare/disabilitare saranno consentite solo se la disattivazione di alcuni moduli non andrà a diminuire il livello di base di sicurezza scelto prima della selezione **Custom**.

<span style="color: #ff0000;"><strong>NOTA PARTICOLARE</strong></span>:

E' molto frequente che l'abilitazione di **Grsecutiry** sui livelli **medio**/**alto** applichi restrizioni pesanti alla directory di sistema **/proc** . Questo tipo di restrizioni potrebbe causare il mal funzionamento di comandi e gestione di tutto ciò che concerne tale directory, rendendo difficile anche il funzionamento di un applicativo semplice come lo può essere ad esempio un contatore di stat come: conky,torsmo e/o anche potrebbe impedire all'utente comandi di sistema come **ifconfig** etc...

Per evitare tale restrizione è sufficiente **<span style="color: #0000ff;">disabilitare</span>** l'opzione <span style="color: #0000ff;"><strong>Proc Restrictions</strong></span> come da screen seguente. Tale voce è  raggiungibile tramite menu:

> <pre>Opzioni di Sicurezza ---&gt; 
                 Grsecurity ---&gt;
                     Filesystem Protections ---&gt;
                                   [ ] Proc restrictions</pre>

&nbsp;

Scelti i moduli e le opzioni da attivare. Salviamo ed usciamo dall'apposita opzione di menu:<span style="color: #ff0000;"><span style="color: #000000;">< </span>E</span>xit >

Ritornati in shell prima di procedere con la compilazione dell'immagine kernel, possiamo customizzare l'uname modificando il makefile alla voce EXTRAVERSION:

<pre>~# vi Makefile
VERSION = 2
PATCHLEVEL = 6
SUBLEVEL = 24
<strong>EXTRAVERSION = .5-quello_che_vuoi</strong></pre>

Salviamo ed usciamo.

**NB**: La patch Grsecurity applicherà in ogni caso di _suo_ un uname personalizzato che aggiungerà all'EXTRAVERSION:   **-grsec ;** Il risultato finale quindi sarà un uname simile a:

**2.6.24.5-quello\_che\_vuoi-grsec**

Completata la parte che necessitava di qualche spiegazione. Completiamo la compilazione dell'immagine del kernel e dei relativi moduli:

<pre>~# make -j5 bzImage
~# make -j5 modules
~# make modules_install</pre>

Finita la creazione dell'immagine e dei moduli spostiamo il nuovo kernel e i relativi file di config nella directory **/boot:**

<pre>~# cp /usr/src/linux/arch/i386/boot/bzImage /boot/vmlinuz-2.6.24.5-grsec
~# cp /usr/src/linux/.config /boot/config_vmlinuz-2.6.24.5-grsec
~# cp /usr/src/linux/System.map /boot/System.map-2.6.24.5-grsec</pre>

Infine sistemiamo il boot loader (Grub/Lilo) con le stringhe relative al nuovo kernel patchato:

<pre>~# vi /etc/lilo.conf

#  Start Config Image kernel 2.6.24.5-grsec
  image = /boot/vmlinuz-2.6.24.5-grsec
  root = "/dev/hdaX"                            =&gt; device "1-2-3..." corrispondente alla partizione radice /
  label = "2.6.24.5-grsec"                      =&gt; nome che apparirà al boot lilo
  read-only
#  End Config Image kernel 2.6.24.5-grsec</pre>

Quindi, una volta salvate le modifiche, riavviamo lilo in modalità verbose per essere sicuri che non vi siano errori:

<pre>~# lilo -v</pre>

Al riavvio noteremo le stringhe relative al chroot abilitato, ai permessi utente,gid etc...

\# End