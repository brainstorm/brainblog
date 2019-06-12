---
author: brainstorm
date: '2007-06-15T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889484
excerpt: null
layout: post
modified: '2007-06-15T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/06/16/nokia-6230i-simple-backup/
tags:
- gadgets
- howto
- software
- unix
title: Nokia 6230i simple backup
---

If you're switching between providers and want to keep your addressbook and sms's, here is a simple way of doing so with [gnokii][1]:

<pre>$ gnokii --getsms AR 1 10 -d >> sms_archive
$ gnokii --getphonebook SM 1 200 > phonebook
</pre>

The "AR" stands for "ARchive" and tells gnokii to search the SMS's there, and not in the INbox ("IN" being another cellphone storage area). Following AR, there is the SMS range you want to save (sms 1st till 10th). Make sure you have a look at gnokii manpage if this is not working for you. For instance, on the following command, the "SM" is referring to the SIM card... perhaps you're not storing your phonebook there, but in the internal mobile phone memory ("ME").

Here I'm assuming that you have configured your [/etc/gnokiirc][2] [properly][3]. Just for the record, I'm using a [bluetooth usb adapter][4].

As a result of this mini-howto, you have a plaintext copy of your phonebook and sms messages on your computer, isn't it easy and convenient ? <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":)" class="wp-smiley" /> Sure, there are clearly better and comprehensive alternatives out there, like [SyncML][5] ([OpenSync][6]), but this solution suits my needs very quickly and it's quite effortless.

Doing a backup of the photos is even simpler if you happen to have [kblueetothd][7], one of the coolest KDE [kioslaves][8]: just drag&drop the pictures to your PC and you're done !

 [1]: http://www.gnokii.org/
 [2]: http://wiki.gnokii.org/index.php/Nokia6230iConfig
 [3]: http://blogs.nopcode.org/brainstorm/2006/06/27/sms-with-gnokii-and-nokia-6230i/
 [4]: http://www.conceptronic.net/site/desktopdefault.aspx?tabindex=0&tabid=200&Cat=10&grp=1020&ar=351&Prod_ID=1227&Prod=CBT200U2
 [5]: http://en.wikipedia.org/wiki/SyncML
 [6]: http://www.opensync.org/
 [7]: http://docs.kde.org/development/en/extragear-pim/kdebluetooth/
 [8]: http://www.kdehispano.org/kioslaves