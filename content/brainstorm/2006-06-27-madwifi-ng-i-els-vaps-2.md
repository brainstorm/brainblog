---
author: brainstorm
date: '2006-06-26T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2006-06-26T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/06/27/madwifi-ng-i-els-vaps-2/
tags:
- security
- software
- unix
- wifi
title: MadWiFi-ng i els VAPs
---

Els que estigueu posats en temes wifi sota unix sabreu que [madwifi-ng][1] és la nova branca dels drivers d'atheros. No tindria massa sentit parlar-ne si no fos perquè incorpora una sèrie de millores molt interessants. Una d'elles es el suport per <acronym title="Virtual Access Point">VAP's</acronym> que ens permetrà usar **varies interfícies amb una mateixa ràdio 802.11b/g**. Anem als exemples concrets:

<pre># wlanconfig ath create wlandev wifi0 wlanmode ap 
ath0
# iwconfig ath0 essid test0 && ifconfig ath0 up && iwconfig ath0
ath0    IEEE 802.11g  ESSID:"test0"
          Mode:Master  Frequency:2.417GHz  Access Point: 00:80:BE:BA:CA:FE
          Bit Rate:0kb/s   Tx-Power:16 dBm   Sensitivity=0/3
          Retry:off   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality=0/94  Signal level=-95 dBm  Noise level=-95 dBm
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:0   Missed beacon:0
</pre>

Fins aquí tot normal, tenim una interfície anomenada *ath0* amb una MAC determinada (manipulada :-P) i en mode **Master** o access point. A continuació ve la gràcia dels VAPs en forma de shellscript:

<!--more-->

<pre># cat > aps.sh
#!/bin/sh
for i in `seq 1 3`
        do
                wlanconfig ath create wlandev wifi0 wlanmode ap
                iwconfig ath$i essid test$i && ifconfig ath$i up
        done
^D
# ./aps.sh
ath1
ath2
ath3
</pre>

Què acaba de passar ? Hem creat **tres** access points virtuals més sobre la mateixa tarja wifi <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" /> Veiem-ho amb l'iwconfig (sortida retallada):

<pre>ath0    IEEE 802.11g  ESSID:"test"
          Mode:Master  Frequency:2.417GHz  Access Point: 00:80:BE:BA:CA:FE
ath1    IEEE 802.11g  ESSID:"test1"
          Mode:Master  Frequency:2.417GHz  Access Point: 06:80:BE:BA:CA:FE
ath2    IEEE 802.11g  ESSID:"test2"
          Mode:Master  Frequency:2.417GHz  Access Point: 0A:80:BE:BA:CA:FE
ath3    IEEE 802.11g  ESSID:"test3"
          Mode:Master  Frequency:2.417GHz  Access Point: 0E:80:BE:BA:CA:FE
</pre>

Aqui els tenim ! Quatre access points virtuals al mateix canal però amb **MACs i ESSID's diferents** i als quals se'ls pot configurar com volguem: ens podria interessar tenir la xarxa WiFi d'ús personal amb WPA i una d'oberta molt més restringida per convidats i una altre AP addicional amb WEP, per a fer experiments, per exemple... tot amb una sola tarja/radio wireless. Cal afegir que tots els clients que he probat han detectat correctament tots els APs, no es tracta doncs de *black magic* no estàndar, funciona i de meravella !

També es poden crear interfícies en mode client (sta) i d'altres. Per a més info, mireu-vos el [wiki][2] de madwifi-ng, no té desperdici.

I què passa si creem aquests access points posant com a ESSID un AP legítim ? Un possible usuari despistat (o una radio nostra més potent) podria associar-se a nosaltres al veure tants AP's repetits a la llista d'scaneig. Cal anar doncs cada cop més amb compte amb els APs repetits abans de connectar-se a una xarxa sense fils <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_wink.gif" alt=";-)" class="wp-smiley" />

 [1]: http://madwifi.org/
 [2]: http://madwifi.org/wiki/UserDocs