---
author: brainstorm
date: '2006-06-26T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889312
excerpt: null
layout: post
modified: '2006-06-26T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/06/27/sms-with-gnokii-and-nokia-6230i/
tags:
- gadgets
- howto
- unix
title: SMS with gnokii and nokia 6230i
---

Sending sms via bluetooth with gentoo is "fairly" simple:

<pre># emerge -pv gnokii
[ebuild   R   ] app-mobilephone/gnokii-0.6.10  +X +bluetooth -irda +mysql -nls -postgres +sms 0 kB
# emerge gnokii
(...)
</pre>

Now tune your /etc/gnokiirc like this:

<pre>[global]
port = &lt;your bluetooth MAC here>
model = 6510
initlength = default
connection = bluetooth
use_locking = yes
serial_baudrate = 19200
smsc_timeout = 10

[gnokiid]
bindir = /usr/sbin/
&lt;/your></pre>

Then launch xgnokii, the gui is intuïtive enough. If you want to send the messages via cmdline, just use the example from gnokii's manpage:

<pre>echo "This is a test message" | gnokii --sendsms +48501123456 -r
</pre>

<!--more-->

Troubleshooting:

*   Q: When I try to run xgnokki or gnokki, it prints *"Couldn't open PHONET device: Connection refused"*
*   A: Try again, it seems that there's a bug related with first-time bluetooth connect attempts
*   Q: I set my cellphone bluetooth settings to autoconnect, but it keeps asking for my PIN on every SMS I send, why ?
*   A: Yep, perhaps a bluetooth security measure ? Bug or feature ?