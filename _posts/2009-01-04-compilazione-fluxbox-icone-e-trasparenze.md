---
id: 137
title: 'Compilazione fluxbox - icone e trasparenze'
date: 2009-01-04T18:49:38+01:00
author: ax
excerpt: |
  Leggero How-to sulla compilazione di un ambiente minimalistico senza tralasciare l'aspetto estetico con l'abilitazione di icone, bar e trasparenze.
  Fluxbox, da tempo risulta essere la soluzione migliore per una linuxbox dalle caratteristiche medio-basse (ram,scheda video).
layout: post
guid: http://linuax.wordpress.com/?p=137
permalink: /2009/01/04/compilazione-fluxbox-icone-e-trasparenze/
categories:
  - Fluxbox
  - How-to
  - Linux
  - Wm
tags:
  - fluxbox
  - icons
  - Linux
  - slackware
  - wm
---
Fluxbox, da tempo risulta essere ancora prima di "openbox" e prima ancora con "kahakai o waimea" (nel 2000 d.c) e morti prima ancora del nascere, praticamente abbandonati a se stessi, la soluzione migliore per una linuxbox dalle caratteristiche medio-basse (ram,scheda video). Insieme ad altri applicativi specificamente destinati diventa senza ombra di dubbio fantastico, sia per uso minimalistico e sia anche per quei linuxiani che amano la desktopmania. Mi limiterò ad illustrare gli attrezzi che dispone questo wm per X al fine di rendere la vita più comoda a tutti quegli utenti che spesso richiedono un uso minore e più snello della box ma al tempo stesso una buona dose si richieste estetiche.

#

Per prima cosa è consigliabile utilizzare la versione svn , per intenderci quella di sviluppo che solitamente contiene alcuni fix che le stable release non supportano. E' bene ricordare che al momento attuale la release stable di fluxbox è la versione 1.1.0 che è reperibile dal sito ufficiale <a href="http://www.fluxbox.org" target="_blank">http://www.fluxbox.org</a>

Siccome però come i miei cordialmente sempre amici mi ricordano io sono un incline all'antiquato e sperimentale allo stesso tempo vada per la svn che detto fra di noi ho sempre usato e su cui non ho mai riscontrato particolari problemi. Scarichiamo quindi la release da user:

<pre>~$ svn checkout svn://svn.berlios.de/fluxbox/trunk fluxbox</pre>

Ci collegheremo al server svn che provvederà a scaricare nella directory "fluxbox" da noi specificata i file per esteso già estratti. A questo punto non ci resta che compilare sempre da user ovviamente:

<pre>~$ cd fluxbox
~$ ./autogen.sh
~$ ./configure</pre>

E' bene ricordare per i non generalmente addetti alla compilazione manuale che la compilazione prevede due livelli: il default utilizzato solitamente se non richiediamo particolari cose e il "configure -help" che ci da la possibilità di aggiungere features alla compilazione di default per intenderci con questo comando vedremo le cose che è possibile modificare e quelle che vengono compilate di default. Vediamo in particolare dando ./configure -help quali sono le variabili che ci interessano per ottenere il massimo dal nostro wm:

<pre> --enable-FEATURE[=ARG]  include FEATURE [ARG=yes]
  --disable-dependency-tracking  speeds up one-time build
  --enable-dependency-tracking   do not reject slow dependency extractors
  --enable-shape          enable support of the XShape extension (default=yes)
  --enable-slit           include code for the Slit (default=yes)
  --enable-remember       include code for Remembering attributes (default=yes)
  --enable-toolbar        include code for Toolbar (default=yes)
  --enable-regexp         regular expression support (default=yes)
  --enable-newwmspec      include code for the new WM Spec (default=yes)
  --enable-ordered-pseudo include code for ordered pseudocolor (8bpp)
                          dithering (default=no)
  --enable-debug          include verbose debugging code (default=no)
  --enable-nls            include native language support (default=no)
  --enable-timed-cache    use new timed pixmap cache (default=yes)
  --enable-kde            KDE slit support (default=yes)
  --enable-gnome          GNOME support (default=yes)
  --enable-xft            Xft (antialias) support (default=yes)
  --enable-xrender        Xrender (transparent) support (default=yes)
  --enable-xpm            Xpm (pixmap themes) support (default=yes)
  --enable-imlib2         Imlib2 (pixmap themes) support (default=yes)
  --enable-xmb            Xmb (multibyte font, utf-8) support (default=yes)
  --enable-randr          RANDR (The X Resize and Rotate Extension) support (default=yes)
  --enable-xinerama       enable xinerama extension (default=yes)</pre>

Come è facile intuire quelle con (default=yes)  sulla parte destra sono le feature's abilitate di default dando un semplice ./configure

Siccome però noi siamo scrupolosi e ci accorgiamo che alcune non sono abilitate e che potrebbero interessarci decidiamo di effettuare la compilazione dando i parametri  che ci interessano compresi quelli già attivi di default e quindi al comando sopra elencato sostituiremo:

<pre>~$ ./configure --enable-shape --enable-slit --enable-remember --enable-toolbar --enable-regexp --enable-newwmspec  --enable-nls --enable-timed-cache --enable-kde --enable-gnome --enable-xft --enable-xrender --enable-xpm --enable-imlib2 --enable-xmb --enable-randr --enable-xinerama</pre>

Alcuni di voi potrebbero dire: ok ma a che serve dargli tutte le flag se sono già in default yes? Semplice, nelle precedenti versioni non erano abilitate e quindi (tanto per) è sempre meglio abilitare per evitare successive rogne.

N.B. Potrebbe capitare un messaggio simile a questo che suggerisce all'user di scaricare la versione git:

<pre>configure: error:
We have switched to git for fluxbox development. Svn is no longer being
used. To get the latest development sources, use the following commands:
git clone git://git.fluxbox.org/fluxbox.git
cd fluxbox
./autogen.sh && ./configure && ./make install</pre>

Detto fatto! il simpaticone ci sta ricordando che alcune feature's non sono presenti nemmeno nella versione svn e ci rimanda ad un altra versione la .git che noi senza perderci d'animo andremo a scaricare e compilare.

Prima però cancelliamo la vecchia cartella fluxbox creata, e quindi:

<pre>~$ cd ..
~$ rm -rf fluxbox/
~$ git clone git://git.fluxbox.org/fluxbox.git
~$ cd fluxbox/
~$ ./autogen.sh
~$ ./configure --enable-shape --enable-slit --enable-remember --enable-toolbar --enable-regexp --enable-newwmspec  --enable-nls --enable-timed-cache --enable-kde --enable-gnome --enable-xft --enable-xrender --enable-xpm --enable-imlib2 --enable-xmb --enable-randr --enable-xinerama
~$ make
~$ make install</pre>

NOTA. Chiaramente è bene ricordare che nel caso ci dovessero essere errori molto probabilmente sono riferiti a librerie mancanti, nel caso specifico gli errori comuni sono riguardanti: imlib2 ; xrender ; xinerama. Quindi prima di tutto accertatevi di avere tutto ciò che vi occorre ancora prima di effettuare la compilazione.

Installato il wm non ci resta che avviarlo scegliendolo di default allo "startx". Quindi da shell nel caso usiate come il sottoscritto slackware daremo un:

<pre>~$ xwmconfig</pre>

Nel caso la vostra distro sia diversa basterà modificare .xinitrc presente nella $HOME e sotto  "# Start the window manager" modificare:

<pre>exec /usr/bin/startfluxbox</pre>

O anche:

<pre>$ <span class="code-input">echo "exec startfluxbox" &gt; ~/.xinitrc</span></pre>

Scelto Fluxbox: starteremo il server X con:

<pre>~$ startx</pre>

NOTA. Potrebbe capitare in slackware che il nostro caro amico si sia dimenticato di creare automaticamente l'init di avvio per fluxbox in

/etc/X11/xinit/

e quindi xwmconfig non ci mostri nel menù grafico il link a fluxbox. In questo caso "senza disperarci" cercheremo i file che ci interessano per capire il percorso di default che non abbiamo provveduto a specificare prima in fase di compilazione e creeremo lo .xinitrc relativo a fluxbox nella cartella precedentemente detta che si occupa di gestire tutti gli init di avvio dei wm presenti nella box (kde,gnome,xfce,etc). Quindi questa volta da root:

Le virgolette " "  segnano il percorso che ci interessa e che terremo a mente.

<pre>~# whereis startfluxbox
startfluxbox: "/usr/bin/startfluxbox" /usr/X11R6/bin/startfluxbox /usr/bin/X11/startfluxbox
/usr/X11/bin/startfluxbox /usr/local/bin/startfluxbox
~# cd /etc/X11/xinit/
~# vi xinitrc.fluxbox</pre>

Dentro xinitrc.fluxbox scriveremo le stringhe necessarie a startare X per questo wm:

<pre>#!/bin/sh

userresources=$HOME/.Xresources
usermodmap=$HOME/.Xmodmap
sysresources=/usr/lib/X11/xinit/.Xresources
sysmodmap=/usr/lib/X11/xinit/.Xmodmap

# merge in defaults and keymaps

if [ -f $sysresources ]; then
    xrdb -merge $sysresources
fi

if [ -f $sysmodmap ]; then
    xmodmap $sysmodmap
fi

if [ -f $userresources ]; then
    xrdb -merge $userresources
fi

if [ -f $usermodmap ]; then
    xmodmap $usermodmap
fi

# Start the window manager:
# NOTA: exec $percorso segnato in precedenza con whereis
exec /usr/bin/startfluxbox</pre>

E quindi salvate le modifiche:

<pre>~# chmod +x xinitrc.fluxbox
~# exit
~$ xwmconfig            ### dove sceglieremo Fluxbox
~$ startx</pre>

<pre><img class="alignnone size-medium wp-image-185" title="fluxbox_screen_default" src="http://linuax.files.wordpress.com/2009/01/fluxbox_screen1.jpg?w=300" alt="fluxbox_screen_default" width="300" height="225" /> # END</pre>