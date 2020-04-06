---
id: 756
title: Samba + Cups = client-error-document-format-not-supported
date: 2009-06-26T11:30:54+01:00
author: ax
excerpt: "Rete Linux-Windows e condivisione della stampante sfruttando Samba + Cups; Risoluzione dell'errore: client-error-document-format-not-supported"
layout: post
guid: http://linuax.wordpress.com/?p=756
permalink: /2009/06/26/samba-cups-client-error-document-format-not-supported/
image_size:
  - ""
  - ""
categories:
  - How-to
  - Linux
  - Reti LAN
  - Samba
tags:
  - How-to
  - Linux
  - samba linux
  - Tips
---
Sempre più spesso ci si trova con errori frequenti di non proprio facilissima risoluzione. La gestione di reti LAN e la condivisione di file, cartelle, stampa etc, è affidata come già detto in altri post a **Samba**; ma spesso questo tool in coppia con **Cups** (applicativo di gestione stampa) ci restituisce un bel pò di errori più o meno gravi. Nel caso specifico, se si sta provando a stampare tramite cups dal pc Windows verso il server Linux potremmo ritrovarci con un output di log **samba** simile a questo:

<pre>[2009/06/24 22:20:09, 0] printing/print_cups.c:cups_job_submit(656)
Unable to print file to brother - client-error-document-format-not-supported</pre>

**NB**: Questo errore impedisce alla stampante di avviare il processo di stampa. Per risolvere la cosa dobbiamo effettuare alcune modifiche direttamente su cups come da seguito.

**Risoluzione:**

<pre>~# vi /etc/cups/mime.convs</pre>

Aperto il file _**mime.convs**_ va decommentata (**togliere il #**) la stringa:

<pre># application/octet-stream   application/vnd.cups-raw   0   -</pre>

Che diventerà chiaramente decommentandola:

<pre><strong><span style="color:#99cc00;">application/octet-stream   application/vnd.cups-raw   0   -</span></strong></pre>

Stessa modifica va affettuata sul file **mime.types:**

<pre>~# vi /etc/cups/mime.types</pre>

Decommentiamo, se non lo fosse già, la stringa:

<pre># application/octet-stream</pre>

Che diventerà:

<pre><strong><span style="color:#99cc00;">application/octet-stream</span></strong></pre>

Infine modificheremo il file generale di configurazione di cups aggiungengo le seguenti stringhe:

<pre>~# vi /etc/cups/cupsd.conf
&lt;Location /printers&gt;
 AuthType None
 Order Deny,Allow
 Deny From None
 Allow From All
&lt;/Location&gt;</pre>

Che modificato risulterà simile al seguente:

<pre>LogLevel info
SystemGroup sys root
# Allow remote access
Port 631
Listen /var/run/cups/cups.sock
# Enable printer sharing and shared printers.
Browsing On
BrowseOrder allow,deny
BrowseAllow all
BrowseAddress @LOCAL
DefaultAuthType Basic
&lt;Location /&gt;
 Allow all
 Allow all
 Allow all
 # Allow shared printing...
 Order allow,deny
 Allow all
&lt;/Location&gt;
&lt;Location /admin&gt;
 Encryption Required
 Allow all
 # Restrict access to the admin pages...
 Order allow,deny
&lt;/Location&gt;
&lt;Location /admin/conf&gt;
 AuthType Default
 Require user @SYSTEM
 Allow all
 # Restrict access to the configuration files...
 Order allow,deny
&lt;/Location&gt;
<strong><span style="color:#99cc00;">&lt;Location /printers&gt;
 AuthType None
 Order Deny,Allow
 Deny From None
 Allow From All
&lt;/Location&gt;</span></strong>
&lt;Policy default&gt;
 &lt;Limit Send-Document Send-URI Hold-Job Release-Job Restart-Job Purge-Jobs Set-Job-Attributes Create-Job-Subscription Renew-Subscription Cancel-Subscription Get-Notifications Reprocess-Job Cancel-Current-Job Suspend-Current-Job Resume-Job CUPS-Move-Job&gt;
 Require user @OWNER @SYSTEM
 Order deny,allow
 &lt;/Limit&gt;
 &lt;Limit CUPS-Add-Modify-Printer CUPS-Delete-Printer CUPS-Add-Modify-Class CUPS-Delete-Class CUPS-Set-Default&gt;
 AuthType Default
 Require user @SYSTEM
 Order deny,allow
 &lt;/Limit&gt;
 &lt;Limit Pause-Printer Resume-Printer Enable-Printer Disable-Printer Pause-Printer-After-Current-Job Hold-New-Jobs Release-Held-New-Jobs Deactivate-Printer Activate-Printer Restart-Printer Shutdown-Printer Startup-Printer Promote-Job Schedule-Job-After CUPS-Accept-Jobs CUPS-Reject-Jobs&gt;
 AuthType Default
 Require user @SYSTEM
 Order deny,allow
 &lt;/Limit&gt;
 &lt;Limit Cancel-Job CUPS-Authenticate-Job&gt;
 Require user @OWNER @SYSTEM
 Order deny,allow
 &lt;/Limit&gt;
 &lt;Limit All&gt;
 Order deny,allow
 &lt;/Limit&gt;
&lt;/Policy&gt;</pre>

Effettuate tutte le modifiche non resta che riavviare il servizio **cups**:

<pre>~# /etc/rc.d/rc.cups restart</pre>

\# End