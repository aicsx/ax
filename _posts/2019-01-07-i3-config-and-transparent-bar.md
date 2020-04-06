---
id: 1857
title: 'i3 - config and transparent bar'
date: 2019-01-07T07:00:26+01:00
author: ax
excerpt: |
  <p>Esempio di configurazione di i3 tiling wm e script bash per forzare la barra in modalità trasparente all'avvio.</p>
layout: post
guid: https://linuax.altervista.org/blog/?p=1857
permalink: /2019/01/07/i3-config-and-transparent-bar/
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'false'
avopt_banners_on_page:
  - 'true'
aa_post_meta:
  - 'a:1:{s:5:"where";s:5:"after";}'
categories:
  - i3
  - Linux
  - Senza categoria
  - Slackware
  - Tips
  - Wm
tags:
  - bar
  - config
  - i3
  - slackware
  - tiling
  - Transparent
  - wm
---
Esempio di configurazione di i3 tiling wm:

<pre>~$ cat .config/i3/config

# i3 config file (v4)
#
# Please see https://i3wm.org/docs/userguide.html for a complete reference!
#
# This config file uses keycodes (bindsym) and was written for the QWERTY
# layout.
#
# To get a config file with the same key positions, but for your current
# layout, use the i3-config-wizard
#
# ax, ax@slackware.eu
set $mod Mod4

new_window pixel 1
new_float normal

hide_edge_borders none

# Font for window titles. Will also be used by the bar unless a different font
# is used in the bar {} block below.
font pango:monospace 9

# This font is widely installed, provides lots of unicode glyphs, right-to-left
# text rendering and scalability on retina/hidpi displays (thanks to pango).
#font pango:DejaVu Sans Mono 8

# Before i3 v4.8, we used to recommend this one as the default:
# font -misc-fixed-medium-r-normal--13-120-75-75-C-70-iso10646-1
# The font above is very space-efficient, that is, it looks good, sharp and
# clear in small sizes. However, its unicode glyph coverage is limited, the old
# X core fonts rendering does not support right-to-left and this being a bitmap
# font, it doesn't scale on retina/hidpi displays.

# use these keys for focus, movement, and resize directions when reaching for
# the arrows is not convenient
set $up l
set $down k
set $left j
set $right h

# use Mouse+Mod1 to drag floating windows to their wanted position
floating_modifier $mod

# start a terminal
#bindsym $mod+Return exec i3-sensible-terminal
bindsym $mod+Return exec xfce4-terminal

# kill focused window
bindsym $mod+Shift+q kill

# start dmenu (a program launcher)
bindsym $mod+d exec dmenu_run -nb black -nf darkgrey  -sf black -sb darkgrey
# There also is the (new) i3-dmenu-desktop which only displays applications
# shipping a .desktop file. It is a wrapper around dmenu, so you need that
# installed.
# bindsym Mod1+d exec --no-startup-id i3-dmenu-desktop
bindsym $mod+F2 exec firefox
bindsym $mod+F3 exec pcmanfm

# change focus
bindsym $mod+$left focus left
bindsym $mod+$down focus down
bindsym $mod+$up focus up
bindsym $mod+$right focus right

# alternatively, you can use the cursor keys:
bindsym $mod+Left focus left
bindsym $mod+Down focus down 
bindsym $mod+Up focus up 
bindsym $mod+Right focusright

# move focused window
bindsym $mod+Shift+$left move left
bindsym $mod+Shift+$down move down
bindsym $mod+Shift+$up move up
bindsym $mod+Shift+$right move right

# alternatively, you can use the cursor keys:
bindsym $mod+Shift+Left move left
bindsym $mod+Shift+Down move down
bindsym $mod+Shift+Up move up
bindsym $mod+Shift+Right move right

# split in horizontal orientation
bindsym $mod+- split h

# split in vertical orientation
bindsym $mod+| split v

# enter fullscreen mode for the focused container
bindsym $mod+f fullscreen toggle

# change container layout (stacked, tabbed, toggle split)
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split

# toggle tiling / floating
bindsym $mod+Shift+space floating toggle

# change focus between tiling / floating windows
bindsym $mod+space focus mode_toggle

# focus the parent container
bindsym $mod+a focus parent

# move the currently focused window to the scratchpad
bindsym $mod+Shift+minus move scratchpad

# Show the next scratchpad window or hide the focused scratchpad window.
# If there are multiple scratchpad windows, this command cycles through them.
bindsym $mod+minus scratchpad show

# Define names for default workspaces for which we configure key bindings later on.
# We use variables to avoid repeating the names in multiple places.
set $ws1 "1"
set $ws2 "2"
set $ws3 "3"
set $ws4 "4"
set $ws5 "5"
set $ws6 "6"
set $ws7 "7"
set $ws8 "8"
set $ws9 "9"
set $ws10 "10"


# switch to workspace
bindsym $mod+1 workspace $ws1
bindsym $mod+2 workspace $ws2
bindsym $mod+3 workspace $ws3
bindsym $mod+4 workspace $ws4
bindsym $mod+5 workspace $ws5
bindsym $mod+6 workspace $ws6
bindsym $mod+7 workspace $ws7
bindsym $mod+8 workspace $ws8
bindsym $mod+9 workspace $ws9
bindsym $mod+0 workspace $ws10

# move focused container to workspace
bindsym $mod+Shift+1 move container to workspace $ws1
bindsym $mod+Shift+2 move container to workspace $ws2
bindsym $mod+Shift+3 move container to workspace $ws3
bindsym $mod+Shift+4 move container to workspace $ws4
bindsym $mod+Shift+5 move container to workspace $ws5
bindsym $mod+Shift+6 move container to workspace $ws6
bindsym $mod+Shift+7 move container to workspace $ws7
bindsym $mod+Shift+8 move container to workspace $ws8
bindsym $mod+Shift+9 move container to workspace $ws9
bindsym $mod+Shift+0 move container to workspace $ws10

# reload the configuration file
bindsym $mod+Shift+c reload
# restart i3 inplace (preserves your layout/session, can be used to upgrade i3)
bindsym $mod+Shift+r restart
# exit i3 (logs you out of your X session)
bindsym $mod+Shift+e exec "i3-nagbar -t warning -m 'You pressed the exit shortcut. Do you really want to exit i3? This will end your X session.' -b 'Yes, exit i3' 'i3-msg exit'"

# resize window (you can also use the mouse for that)
mode "resize" {
        # These bindings trigger as soon as you enter the resize mode

        # Pressing left will shrink the window’s width.
        # Pressing right will grow the window’s width.
        # Pressing up will shrink the window’s height.
        # Pressing down will grow the window’s height.
        bindsym $left       resize shrink width 10 px or 10 ppt
        bindsym $down       resize grow height 10 px or 10 ppt
        bindsym $up         resize shrink height 10 px or 10 ppt
        bindsym $right      resize grow width 10 px or 10 ppt

        # same bindings, but for the arrow keys
        bindsym Left        resize shrink width 10 px or 10 ppt
        bindsym Down        resize grow height 10 px or 10 ppt
        bindsym Up          resize shrink height 10 px or 10 ppt
        bindsym Right       resize grow width 10 px or 10 ppt

        # back to normal: Enter or Escape or Mod1+r
        bindsym Return mode "default"
        bindsym Escape mode "default"
        bindsym Mod1+r mode "default"
}

bindsym $mod+r mode "resize"

# Start i3bar to display a workspace bar (plus the system information i3status
# finds out, if available)
set $trasparent #000000
bar {
        status_command i3status
	i3bar_command i3bar -t
        position top
	   colors {
                background #000000
                statusline #FFFFFF
        	focused_workspace #F9FAF9 #292929 #EEE8D5
		active_workspace #595B5B #353836 #FDF6E3
		inactive_workspace #686868 #000000 #EEE85D
		urgent_workspace #353836 #FDF6E3 #E5201D
	}
}


# Theme colors                                                                                                                                                           
# class                 border  backgr. text    indic.  child_border
client.focused          #808280 #808280 #80FFF9 #FDF6E3
client.focused_inactive #434745 #434745 #16A085 #454948
client.unfocused        #434745 #434745 #16A085 #454948
client.urgent           #CB4B16 #FDF6E3 #16A085 #268BD2
client.placeholder      #000000 #0c0c0c #ffffff #000000 #0c0c0c

client.background       #2B2C2B


#######################################################################
# automatically start i3-config-wizard to offer the user to create a
# keysym-based config which used their favorite modifier (alt or windows)
#
# i3-config-wizard will not launch if there already is a config file
# in ~/.i3/config.
#
# Please remove the following exec line:
#######################################################################
#exec i3-config-wizard
exec --no-startup-id numlockx on
exec --no-startup-id nitrogen --restore; sleep 1; compton -b
exec --no-startup-id clipit
exec --no-startup-id xrandr --output VGA-0 --mode 1366x768
exec --no-startup-id /home/ax/.config/i3/i3bar_trasp.sh
exec --no-startup-id volumeicon

# gaps 
gaps inner 5
gaps outer 0

smart_gaps on

smart_borders on

~$ cd .config/i3
~$ wget <a href="https://linuax.altervista.org/blog/wp-content/uploads/2019/01/config"><strong>config</strong></a></pre>

**Nota: **ovvimente il path è ralitivo. Su Slackware e Freebsd è praticamente identico, ma su altre distribuzioni probabilmente ometterà la directory config. Quindi: spostatevi nel path corretto, scaricate ed eventualmente modificate il file config.

**Nota Bene: **come da esempio il config richiama lo script bash che si occupa di assegnare la trasparenza alla barra di stato di i3. Inutile dire che prima va opportunemente compilata. Se avete seguito il post precedente che prevede la compilazione automatica mediante sbopkg non avrete alcun tipo di problema. 

<pre>~$ cat .config/i3/i3bar_trasp.sh



<div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="bash codecolorer">
    <span class="co0">#!/bin/sh</span><br />
    <span class="co0"># Search id for i3bar and transparent set</span><br />
    <span class="co0"># Required: xwininfo, transset</span><br />
    <span class="co0">#</span><br />
    <span class="co0"># ax - ax@slackware.eu</span><br />
    <br />
    <span class="kw1">for</span> <span class="kw2">id</span> <span class="kw1">in</span> <span class="sy0">`</span>xwininfo <span class="re5">-all</span> <span class="re5">-root</span> <span class="sy0">|</span> <span class="kw2">grep</span> <span class="st_h">'i3bar for output'</span> <span class="sy0">|</span> <span class="kw2">awk</span> <span class="st_h">'{print $1}'</span><span class="sy0">`</span>; <br />
         &nbsp; &nbsp; <span class="kw1">do</span> <br />
              &nbsp; transset <span class="nu0">0.8</span> <span class="re5">-i</span> <span class="re1">$id</span> <span class="sy0">&gt;</span> <span class="sy0">/</span>dev<span class="sy0">/</span>null; <span class="kw1">done</span>
  </div>
</div>

~$ chmod +x .config/i3/i3bar_trasp.sh</pre>

Riavviate la lettura del file config di i3; approfittiamo per usare uno dei bindkeys settati precedentemente:

Mod4+Shift+r

**Nota Bene:** Mod4 corrisponde al tasto Windows sulla tastiera del mio laptop, si può modificare ovviamente con altri tasti. Visto che quel tasto in altri casi che omettono l'uso di i3 non lo utilizzo praticamente MAI, l'ho scelto per la mia configurazione default.

\# End