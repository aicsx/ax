---
id: 813
title: 'Arch Linux bootsplash: fbsplash'
date: 2009-09-15T12:53:43+01:00
author: ax
excerpt: 'Installazione, abilitazione e configurazione splash grafico su Arch Linux senza patch e ricompilazione del kernel: Fbsplash.'
layout: post
guid: http://linuax.wordpress.com/?p=813
permalink: /2009/09/15/arch-linux-bootsplash-fbsplash/
image_size:
  - ""
  - ""
categories:
  - Arch
  - Boot
  - bootsplash
  - How-to
  - Linux
tags:
  - Arch
  - bootsplash
  - fbsplash
  - Linux
---
Così come descritto in altri post precedenti riguardo Slackware e bootsplash, vediamo sinteticamente come abilitare lo **splash** grafico con tanto di barra ed effetti fadein,fadeout all'avvio di **Arch**.

**NB**: **fbsplash** è un utility che implementa lo splash grafico. E' adatta soprattutto per la modalità di avvio grafico silent. Comunemente è associata al progetto gensplash (patch fbcondecor) che non sarà trattato in questo post.Il motivo sta nel fatto che  gensplash richiede patch e compilazione kernel. Sarà quindi trattata in post successivi.

Per prima cosa scarichiamo da AUR e/o tramite yaourt il pacchetto fbsplash:

<pre>~$ yaourt -S fbsplash</pre>

**NB**: L'installazione di fbsplash installerà solo le utility, non prevede aggiunta di temi associati di default.

Il passo successivo sarà scaricare initscripts-extras-fbsplash.

<pre>~$ yaourt -S initscripts-extras-fbsplash</pre>

**NB:** Questo pacchetto è necessario, in quanto, contiene gli script hooking per fbsplash. Senza di esso la creazione dell'immagine kernel con mkinitcpio non sarà generata correttamente.

Per capirci in maniera breve: installato fbsplash, dovremo creare un immagine init necessaria al caricamento del nostro splash. Il concetto è lo stesso, così come avveniva con patch bootsplash. La differenza sta nel metodo. Su bootsplash si utilizzava: _**/sbin/splash -s -f /percorso_tema/config.cfg >> /boot/initrd.tema**_ . Su Arch la differenza è che la creazione di un immagine init con tutte le dipendenze e le ragioni per cui possa essere creata, sia generata tramite mkinitcpio.

Installiamo il tema prescelto:

<pre>~$ yaourt -S fbsplash-theme-archax</pre>

**NB:** è un esempio, altri temi sono disponibili su AUR. La directory di riferimento per i temi installati e scaricati è  **/etc/splash/** .

Prima di creare l'immagine kernel con mkinitcpio, modifichiamo con un editor il file config per dire al sistema di considerare l'ordine di avvio e aggiungiamo il parametro **fbsplash**. La stringa di riferimento è **HOOKS=""**.

<pre>~$ sudo vi /etc/mkinitcpio.conf
HOOKS="<strong>fbsplash</strong> base udev autodetect pata scsi sata filesystems"</pre>

**NB**: Può sembrare scontato dirlo, ma l'ordine di carimento è importante. Non tanto adesso che si parla solo di **fbsplash**, ma lo sarà sucessivamente in altri post, nel caso di patch **fbcondecor** utilizzata per lo più in modalità grafica verbose: un esempio a pennello è lo splash utilizzato di default in gentoo.

Come ultimo passo diciamo al kernel che tema utilizzare prima di creare l'immagine modificando il path in **/etc/conf.d/fbsplash** alla voce SPLASH_THEMES come da esempio:

<pre>~$ sudo vi /etc/conf.d/fbsplash
SPLASH_THEMES="archax"
</pre>

Salvato il path in _**/etc/conf.d/fbsplash**_ e modificati gli hooks  non resta che generare l'immagine con mkinitcpio:

<pre>~$



<div class="codecolorer-container text dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="text codecolorer">
    sudo mkinitcpio -p kernel26
  </div>
</div>

</pre>

Creata l'immagine init _kerenel26.img_ modificheremo **/etc/rc.conf** sezione **Daemons=()** aggiungendo il parametro fbsplash che richiama _**/etc/rc.d/fbsplash**_ come da esempio:

<pre>~$ vi /etc/rc.conf
DAEMONS=(syslog-ng hal .....<strong> fbsplash</strong>)</pre>

Non resta che apportare le dovute modifiche al boot manager (GRUB di default in ARCH); Modifichiamo grub dandogli i parametri dell'immagine kernel, init, grafica, modalità di avvio. Esempio:

<pre>~$ vi /boot/grub/menu.lst
# (4) Arch Linux 2.6.30.6 + fbsplesh
title  Arch Linux Splash
root   (hd1,0)
kernel /vmlinuz26 root=/dev/sdb2 ro <strong>video=vesafb vga=792 logo.nologo console=tty1 splash=silent,theme:archax quiet</strong>
initrd /kernel26.img</pre>

**NB**:  E' sempre buona norma, evitare di cancellare link ad altre voci di menù del boot loader fino ad avvenuta verifica.

Per verificare:

<pre>~$ sudo reboot</pre>

\# End