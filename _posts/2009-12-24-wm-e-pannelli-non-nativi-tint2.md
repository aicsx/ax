---
id: 940
title: 'WM e pannelli non nativi: tint2'
date: 2009-12-24T17:11:20+01:00
author: ax
excerpt: "Molti dei più famosi WM (OpenBox, PekWM,ecc..) non prevedono l'avvio di un pannello nativo. Questo aspetto è facilmente risolvibile attraverso l'utilizzo di pannelli non nativi, che rispecchino in base ai gusti e alle esigenze, l'utente. In questo breve post si fa una paronamica generale dei pannelli più diffusi con una particolare attenzione per tint2."
layout: post
guid: http://linuax.wordpress.com/?p=940
permalink: /2009/12/24/wm-e-pannelli-non-nativi-tint2/
image_size:
  - ""
  - ""
categories:
  - Linux
  - Openbox
  - PekWM
  - Programmi
  - Tips
tags:
  - How-to
  - Linux
  - tint2
  - Tips
  - wm
---
Molti dei più famosi **WM** (**OpenBox**, **PekWM**,etc) non prevedono l'avvio di un pannello nativo. Tradotto: se apro 2 applicazioni mi sposto tra le due attraverso il click del tasto centrale del mouse direttamente sul desktop. E le icone della systray ? e l'ora ? volendo vedere le app in uso direttamente in una bar?

Esistono tanti, ma proprio tanti progetti in merito. Una breve lista di quelli che ho testato con felicità include:

**<a href="http://pypanel.sourceforge.net/" target="_blank">Pypanel </a>**

**<a href="http://nsf.110mb.com/bmpanel/" target="_blank">Bmpanel</a>**

**<a href="http://fbpanel.sourceforge.net/" target="_blank">Fbpanel</a>**

**<a href="http://www.gnomefiles.org/app.php/LXPanel" target="_blank">LXpanel</a>**

C'è da dire che fra tutti quelli che ho provato, il migliore per "le mie esigenze" ovviamente è:<a href="http://code.google.com/p/tint2/" target="_blank"><strong> Tint2</strong></a>. Un pannello ampiamente customizzabile che nasce appositamente per wm della famiglia *box. Quindi automaticamente mi limiterò ad accennare di questo.

L'installazione non prevede particolari accorgimenti, oltre alla compilazione classica.

La directory di riferimento è **~/.config/tint2** dove risiede il file **tint2rc** che racchiude tutti i settaggi del pannello. Per evitare di andare troppo per le lunghe vi incollo il mio:

<pre>~$ cp ~/.config/tint2/tint2rc ~/.config/tint2/tinit2rc_backup
~$ vi ~/.config/tint2/tint2rc
</pre>

<pre>#---------------------------------------------
# TINT2  AX CONFIG FILE
#---------------------------------------------

#---------------------------------------------
# BACKGROUND AND BORDER
#---------------------------------------------
rounded = 0
border_width = 0
background_color = #1A1A1A 0
border_color = #979797 100

rounded = 2
border_width = 1
background_color = #ffffff 7
border_color = #ffffff 50

rounded = 0
border_width = 1
background_color = #ffffff 0
border_color = #ffffff 0

#---------------------------------------------
# PANEL
#---------------------------------------------
panel_monitor = all
panel_position = bottom center
panel_size = 100% 22
panel_margin = 0 0
panel_padding = 6 0
font_shadow = 0
panel_background_id = 0

#---------------------------------------------
# TASKBAR
#---------------------------------------------
taskbar_mode = single_desktop
taskbar_padding = 0 0 6
taskbar_background_id = 0

#---------------------------------------------
# TASKS
#---------------------------------------------
task_icon = 1
task_text = 1
task_centered = 1
task_padding = 2 2
task_font = Zekton 8
task_font_color = #ffffff 50
task_active_font_color = #ffffff 100
task_background_id = 2
task_active_background_id = 2

#---------------------------------------------
# SYSTRAYBAR
#---------------------------------------------
systray_padding = 4 2 3
systray_background_id = 2

#---------------------------------------------
# CLOCK
#---------------------------------------------
time1_format = %H:%M
time1_font = sans 8
time2_format = %A %d %B
time2_font = sans 6
clock_font_color = #ffffff 76
clock_padding = 4 4
clock_background_id = 1

#---------------------------------------------
# MOUSE ACTION ON TASK
#---------------------------------------------
mouse_middle = close
mouse_right = none
mouse_scroll_up = toggle
mouse_scroll_down = iconify</pre>

**NB**: adattarlo ai propri gusti e alle proprie esigenze è davvero semplice. Il miglior modo per capire i settaggi è provare a cambiarli. Dal link ufficiale del progetto sono disponibili config di temi e documentazione aggiuntiva.