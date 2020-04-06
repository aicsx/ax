---
id: 199
title: Tips Fluxbox
date: 2009-01-05T19:49:11+01:00
author: ax
excerpt: Dopo i post degli how-to precedenti che riguardavano la compilazione e la configurazione generale del nostro ambiente minimalistico vi pasto alcuni tips e note che non sono stati trattati nei precedenti how-to riguardanti fluxbox.
layout: post
guid: http://linuax.wordpress.com/?p=199
permalink: /2009/01/05/tips-fluxbox/
categories:
  - Fluxbox
  - How-to
  - Linux
  - Tips
  - Wm
tags:
  - fluxbox
  - How-to
  - Linux
  - Tips
  - wm
---
Vi sarà sicuramente capitato leggendo gli how-to o semplicemente ricercando screen in rete di notare i menù trasparenti o le dockapps ed altre piccole perle che contribuiscono a rendere fluxbox decisamente il top per chi ama il minimalismo ma anche l'estetica volendo.

#

Fluxbox come visto precedentemente supporta fin da compilazione e in maniera nativa le trasparenze, ma in alcune distro datate xorg non aggiornato potrebbe richiede l'installazione di due applicativi:  <span class="norm"><span class="norm">xcompmgr e transset. Per evitare future rogne quindi accertatevi che siano presenti nel sistema e in caso contrario scaricate e compilat.</span></span>

<span class="norm"><span class="norm"><strong>NB: </strong>Non ci sono grosse cose da spiegare per l'installazione di xcompmgr e transset, l'unica cosa da modificare è proprio xorg.conf al quale vanno apportate le seguenti modifiche nella sezione "extensions".Se non c'è createla:</span></span>

<pre><span class="norm">


<pre>Section "Extensions"
        Option "Composite" "true"
        Option "RENDER" "true"
EndSection</pre>


<p>
  </span><br />
  Al riavvio di X saranno abilitate di default sul server grafico. Vediamo ora in specifico alcuni dettagli non trattati nei precedenti post:
</p>


<p>
  Una volta avviato Fluxbox noteremo una barra in basso, chiamata "toolbar", dove andranno a finire le finestre iconizzate dei programmi e dove potremo leggere ora di sistema e numero di desktop virtuale attivo; volendo potremo cambiare desktop virtuale posizionandoci con il mouse nella toolbar sopra il numero di desktop virtuale e agendo sulla rotellina del mouse stesso.
</p>


<ul>
  <li>
    Cliccando invece con il tasto destro del mouse sul desktop ci apparirà il "menu" di fluxbox da cui possiamo lanciare le varie applicazioni(notare che è possibile agire sulla trasparenza del menu, per le versioni 0.9.x, mediante la voce apposita "menu alfa" nel sottomenu "fluxbox menu"->"configure").
  </li>
  
</ul>


<ul>
  <li>
    Si puo "switchare" tra una finestra aperta ed un altra con la combinazione alt+tab.
  </li>
  
</ul>


<ul>
  <li>
    Si ricorda che sono supportati a pieno gli stili di blackbox e le "dock apps" mediante la slitlist (per ulteriori informazioni leggere la documentazione a riguardo sul sito).
  </li>
  
</ul>


<p>
  <strong>-MENU CONFIGURAZIONE</strong>
</p>


<p>
  Opzioni utili nel sottomenu <strong>fluxboxmenu->configure</strong> accessibile tramite il menu di fluxbox. In "configure" appunto troviamo utili le seguenti voci:<br />
  <strong>- Opaque window moving</strong> = se selezionato quando muoviamo una finestra sul desktop la vediamo sempre disegnata completamente durante il suo "trascinamento";<br />
  <strong>- Full maximization =</strong> se selezionato quando andremo ad espandere completamente una finestra sul desktop questa andra a coprire anche la toolbar;<br />
  <strong>- Desktop wheeling =</strong> attiva la funzione della rotellina per lo "switch" dei desktop virtuali;<br />
  <strong>- Antialias =</strong> attiva l'antialiasing dei fonts (se attivando l'antialising noti che il font si ingrandisce troppo puoi rimediare inserendo la seguente riga in fondo al tuo file di tema in
</p>


<pre>/home/utente/.fluxbox/styles: *.font: Verdana-7 -&gt;scegliendo un fontsize a piacere);</pre>


<p>
  <strong>- Menu alpha =</strong> incrementa o decrementa il valore della trasparenza del menu sullo sfondo del desktop;<strong></strong>
</p>


<p>
  <strong>NB: </strong>255 vuol dire nessuna trasparenza mentre 0 è la max trasparenza;<br />
  <strong></strong>
</p>


<p>
  <strong>- Slit =</strong> sottomenu di configurazione della slitlist con diverse voci;<br />
  <strong>- Toolbar =</strong> sottomenu di configurazione della tollbar con diverse voci.
</p>


<p>
  <strong>TIPS:</strong>
</p>


<p>
  <strong>-SLITLIST-</strong> la slitlist è una parte "virtuale" (che non si vede) del desktop dove vanno a posizionarsi le "dock applications"; queste ultime sono quelle applet che si trovano spesso anche in window manager come window maker.
</p>


<p>
  Si possono scaricare le dock apps da <a href="http://www.dockapps.org/">www.dockapps.org</a> e poi si possono lanciare ad ogni avvio di fluxbox mettendole una ad una "in lancio" in .xinitrc (nella $HOME) con alla fine la solita "&" per l'esecuzione delle applet in background. A questo punto dalla prox volta che si lancia fluxbox si noterà un nuovo file in .fluxbox che si chiama slitlist; questo file tiene traccia della posizione e di tutto quello che riguarda le dockapps sul desktop in modo da mantenere sempre le stesse impostazioni ad ogni sessione.
</p>


<p>
  Da menu di fluxbox se si va su:
</p>


<pre>fluxbox menu--&gt;configure---&gt;slit</pre>


<p>
  si trovano le varie opzioni per la slitlist come ad es. in che posizione si vogliono le applets sul desktop,l'orientamento ecc.
</p>


<p>
  <strong>-COME LANCIARE LE APPLICAZIONI ALL'AVVIO DI FLUXBOX-</strong> Questa operazione è semplicissima. Basta scrivere:
</p>


<pre>[startup] {urxvt}</pre>


<p>
  Va inserito in /home/utente/.fluxbox/apps. Chiaramente va ripetuta l'operazione per ogni prog che si vuole far autoavviare.
</p>


<p>
  <strong>LINK</strong>
</p>


<p>
  Sul sito <a href="http://www.box-look.org" target="_blank">http://www.box-look.org</a> potete trovare temi, sfondi e icone.
</p>


<p>
  Se andate alla ricerca di temi GTK o simili e/o quello che avete trovato non vi è bastato vi rimando ai link  per la personalizzazione dei rispettivi wm:
</p>


<p>
  <a href="http://www.gnome-look.org" target="_blank">http://www.gnome-look.org</a>
</p>


<p>
  <a href="http://www.kde-look.org" target="_blank">http://www.kde-look.org</a>
</p>


<p>
  # END
</p>