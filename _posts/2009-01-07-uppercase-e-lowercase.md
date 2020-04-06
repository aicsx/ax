---
id: 217
title: Uppercase e Lowercase
date: 2009-01-07T00:46:21+01:00
author: ax
excerpt: |
  <p>Uppercase e Lowercase: su linux i caratteri in minuscolo e Maiuscolo hanno sostanza differente. Piccolo tip per il reverse di tali caratteri da x=X e viceversa.</p>
layout: post
guid: http://linuax.wordpress.com/?p=217
permalink: /2009/01/07/uppercase-e-lowercase/
categories:
  - How-to
  - Linux
  - Scripts
  - Tips
tags:
  - How-to
  - Linux
  - Tips
---
Un alternativa per la conversione di nomi di file contenuti in una determinata directory: nell'esempio seguente **/test** da maiucolo a miniscolo e viceversa attraverso l'uso di un unica istruzione; **tr.**

L'utilità è quella di normalizzare tali nomi di file, probabilmente provenienti da altri sistemi operativi (esempio: windows), che non distinguono questa differenza.

**NB:** La directory di riferimento è un esempio visto che la stringa non fà riferimenti a directory. Basta entrare nella directory scelta dove applicare la conversione file e startare il comando in base alle proprie esigenze. Si tenga conto che le stringhe non fanno differenze di estensione \*.txt(esempio) o file\*(binario).

Da "minuscolo a Maiuscolo":

<div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="bash codecolorer">
    <span class="co4">~$ </span><span class="kw1">for</span> i <span class="kw1">in</span> <span class="sy0">*</span> ; <span class="kw1">do</span> <span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$i</span> <span class="br0">&#93;</span> <span class="sy0">&</span>amp;<span class="sy0">&</span>amp; <span class="kw2">mv</span> <span class="re5">-i</span> <span class="re1">$i</span> <span class="sy0">`</span><span class="kw3">echo</span> <span class="re1">$i</span> <span class="sy0">|</span> <span class="kw2">tr</span> <span class="st_h">'[a-z]'</span> <span class="st_h">'[A-Z]'</span><span class="sy0">`</span>; <span class="kw1">done</span>
  </div>
</div>

Da "Maiuscolo a minuscolo":

<div class="codecolorer-container bash dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="bash codecolorer">
    <span class="co4">~$ </span><span class="kw1">for</span> i <span class="kw1">in</span> <span class="sy0">*</span> ; <span class="kw1">do</span> <span class="br0">&#91;</span> <span class="re5">-f</span> <span class="re1">$i</span> <span class="br0">&#93;</span> <span class="sy0">&</span>amp;<span class="sy0">&</span>amp; <span class="kw2">mv</span> <span class="re5">-i</span> <span class="re1">$i</span> <span class="sy0">`</span><span class="kw3">echo</span> <span class="re1">$i</span> <span class="sy0">|</span> <span class="kw2">tr</span> <span class="st_h">'[A-Z]'</span> <span class="st_h">'[a-z]'</span><span class="sy0">`</span>; <span class="kw1">done</span>
  </div>
</div>

#

**  
**