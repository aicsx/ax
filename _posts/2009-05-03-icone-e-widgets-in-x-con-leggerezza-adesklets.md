---
id: 705
title: 'Icone e widgets in X con leggerezza: aDesklets'
date: 2009-05-03T16:54:10+01:00
author: ax
excerpt: "aDesklets è un applicazione basata su imlib2 che gestisce varie widgets di sistema mediante una serie di script in python. Risulta essere la vera alternativa a gDesklets. Sicuramente non dispone dell'incredibile mole di widgets di quest'ultimo ma a suo favore c'è  l'estrema leggerezza e indipendenza."
layout: post
guid: http://linuax.wordpress.com/?p=705
permalink: /2009/05/03/icone-e-widgets-in-x-con-leggerezza-adesklets/
categories:
  - Fluxbox
  - How-to
  - Linux
  - Openbox
  - PekWM
  - Programmi
  - Wm
  - Xorg
tags:
  - adesklets
  - icons
  - X
---
Ultimamente sempre più spesso, specie negli ultimi tempi, l'ambiente grafico è diventato una scelta preferita a quello testuale che viceversa è amato in ambito remoto "server", soprattutto in ambito casalingo. A tal proposito, può essere utile monitorare l'intero sistema e/o alcune parti di esso attraverso le cosiddette **widgets** "con cui si indicano applicazioni spesso graficamente curate" che implementano una serie di funzioni come lo possono essere: gestione dei processi di sistema, memoria occupata, sforzo CPU, consumo batteria, mount & umount di periferiche e tante altre... Oltre a queste funzioni chiaramente, molto spesso, tali widgets implementano funzioni decisamente meno _invasive_ (nel senso che non necessariamente devono appoggiarsi ad altre risorse attive) e che hanno il semplice scopo di facilitare e rendere gradevole e usabile l'interfaccia che si presenta agli occhi dell'utente. Tutti si ricorderanno ad esempio del progetto <a href="http://idesk.sourceforge.net/" target="_blank"><strong>Idesk</strong></a>. Nel caso specifico Idesk è una risorsa utilizzata spesso in ambiti minimalistici quali ad esempio: fluxbox, openbox che fornisce icone desktop in ambienti  che di per se non sono stati pensati per rispondere a questo tipo di domanda.

Il problema è che spesso l'utente finale "prediamo ad esempio me" per necessità e/o anche solo per scelta decide di personalizzare un ambiente più o meno minimalistico ma allo stesso tempo di non appesantire un sistema che è destinato già di suo ad una serie di carichi. Supponiamo ad esempio di aver scelto **fluxbox** e di voler usare: idesk per le icone desktop, varie applets per gestire cpu e altro, mettiamo nel conto magari una barra stile mac in basso per gestire le applicazioni di tutti i giorni e il risultato finale è che il nostro ambiente di partenza minimalistico finisce per non rispondere più alle nostre richieste in ambito prestazionale. Capite bene che facendo 1+1 dovremmo trovare un giusto compromesso tra usabilità,prestazioni e capacità di sistema nella gestione dei carichi. Ecco perchè spesso è preferibile utilizzare applicativi pensati per rispondere a tali necessità e allo stesso tempo che implementano le generali richieste di _domanda_ (barre,icone,calendario,stats,periferiche, etc..)

Tornando alla base del discorso, cioè alla domanda, esistono già da parecchio tempo progetti validi di abellimento e gestione del desktop quali ad esempio sono <a href="http://netdragon.sourceforge.net/" target="_blank"><strong>SuperKaramba</strong></a> per _KDE_ e <a href="http://www.gdesklets.de/" target="_blank"><strong>gDesklets</strong></a> per _Gnome_; ed anche: **<a href="http://conky.sourceforge.net/" target="_blank">conky</a>, <a href="http://torsmo.sourceforge.net/" target="_blank">torsmo</a>, <a href="http://www.gkrellm.net/" target="_blank">gkrellm</a>**...

Io personalmente predirigo da tempo in ambito desk un'accoppiata secondo me vincente. **Conky + aDesklets**. Entrambi estremamente leggeri.

Conky dovreste già conoscerlo, se non fosse così è presente un mini post scritto qualche tempo fà:

<pre><a href="http://linuax.wordpress.com/2009/01/03/monitor-di-sistema-ultraleggero-conky/" target="_blank"><strong>Monitor di sistema ultraleggero: Conky</strong></a></pre>

**aDesklets** invece è un applicazione basata su imlib2 che gestisce varie widgets di sistema mediante una serie di script in python. Risulta essere la vera alternativa a gDesklets. Sicuramente non dispone dell'incredibile mole di widgets di quest'ultimo ma a suo favore c'è  l'estrema leggerezza e _indipendenza_ (ovvero è indipendente dal windows manager scelto: KDE, Gnome, fluxbox, openbox, etc. Praticamente si interfaccia con tutti gli ambienti.

Una volta installato da sorgente e/o da pacchetto precompilato per la distribuzione in uso (spesso disponibile), passiamo alla configurazione vera e propria:

<pre>~$ adesklets_installer</pre>

**NB**: si aprirà una finestra che permette di scegliere ed eventualmente installare le desklets scelte. Personalmente io preferisco fare a mano come segue.

**NB**: Il file generale di configurazione è **~/**_**.adesklets**_ . La directory di riferimento per le desklets installate graficamente mediante comando e/o a mano è **~/**_**.desklets**_ . Ambedue presenti nella home utente.

Supponiamo di volere una barra stile mac mediante adesklets:

<pre>~$ wget <a href="http://prdownloads.sourceforge.net/adesklets/yab-0.0.2.tar.bz2?download" target="_blank">yab-0.0.2</a>
~$ tar jxvf yab-0.0.2.tar.bz2
~$ cp -r yab-0.0.2/ ~/.desklets/
~$ cd ~/.desklets/yab-0.0.2/</pre>

**NB**: Solitamente ogni **_desklet_** è fornita di relativo file _config.txt_ e _readme_. Nel caso specifico per **yab** nella directory _**~/.desklets/yab-0.0.2/icons/**_ si andranno a mettere le icone che preferiamo. Successivamente configureremo con il nostro editor preferito il relativo config.txt come da esempio seguente:

<pre>~$ vi ~/.desklets/yab-0.0.2/config.txt
id0 = {'bar_background_1': 'AAAAAA',
 'bar_background_2': None,
 'bar_foreground': 000000,
 'bar_gradient_angle': 255,
 'bar_height': 64,
 'bar_opacity_1': 0,
 'bar_opacity_2': None,
 'caption_above': True,
 'caption_color': '7F748A',
 'caption_delay': 0,
 'caption_fade_in': True,
 'caption_fade_in_duration': 0.5,
 'caption_fade_in_steps': 10,
 'caption_font': None,
 'caption_size': None,
 'click_effect': 'tint(alpha=100,red=255,green=255,blue=255);',
 'click_effect_duration': 0.10000000000000001,
 'icon_max_height': 52,
 'icon_max_width': 52,
 'icon_maximize_threshold': 0,
 'icon_min_height': 52,
 'icon_min_width': 52,
 'icon_spacing': 10,
 'icons': [('terminal.png', 'urxvt', 'urxvt'),
 ('thunar.png', 'Thunar', 'thunar'),
 ('f3me3.png', 'Firefox', 'firefox'),
 ('thunderbird.png', 'Thunderbird', 'thunderbird'),
 ('amule.png', 'Amule', 'amule'),
 ('msn1.png', 'Msn', 'amsn')]}</pre>

**NB**:  E' solo un esempio, ogni config è commentato per poter effettuare modifiche a proprio piacimento.

Per vedere i relativi effetti e/o modifiche sarà sufficiente da shell:

<pre>~$ python ~/.desklets/yab-0.0.2/yab.py</pre>

Seguirà il messaggio:

<pre><blockquote>
  <span style="font-family:courier new,courier;">Do you want to (r)egister this desklet or to (t)est it?</span>
</blockquote>
</pre>

Premendo "t", verrà avviata la desklet sessione singola, potete provarla, vedere come funziona, e quando volete chiuderla; premendo "r", la desklet verrà registrata nel file **<span style="font-family:courier new,courier;">~/.adesklets</span>** per un utilizzo futuro.

Lanciando quindi _<span style="font-family:courier new,courier;">adesklets</span>_, si avvierà la sessione con le desklets registrate, nel nostro caso Yab.

__ Scegliete _«Move»_ per muoverla, la posizione che sceglierete verrà salvata in **<span style="font-family:courier new,courier;">~/.adesklets</span>**.

**- RICHIAMARE LE DESKLETS ALL'AVVIO DI FLUXBOX**

Per richiamare le desklets scelte all'avvio di Fluxbox basta dirgli all'avvio quale delle desklets presenti avviare modificando il file _**~/.fluxbox/apps**_ nella relativa home user come segue:

<pre>[startup]                                    {ADESKLETS_ID=0 python /home/a/.desklets/yab-0.0.2/yab.py}
[startup]                                    {ADESKLETS_ID=0 python /home/a/.desklets/weatherforecast-0.2.1/weatherforecast.py}</pre>

**NB**:  4 desklets  = 4 [startup] ... se ci fossero dubbi consultare gli articoli riguardo fluxbox.

> Sito ufficiale progetto**: <a href="http://adesklets.sourceforge.net/" target="_blank">aDesklets</a>**
> 
> Download dal sito ufficiale**: <a href="http://adesklets.sourceforge.net/desklets.html" target="_blank">desklets</a>**
> 
> Altre desklets sono presenti su: **<a href="http://www.gnome-look.org" target="_blank">gnome-look</a>**

\# End