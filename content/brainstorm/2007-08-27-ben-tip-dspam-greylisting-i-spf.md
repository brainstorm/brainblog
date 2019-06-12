---
author: brainstorm
date: '2007-08-26T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2007-08-26T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/08/27/ben-tip-dspam-greylisting-i-spf/
tags:
- howto
- inet
- software
- unix
title: 'Ben tip d''SPAM: Greylisting i SPF'
---

<img id="image92" src="http://blogs.nopcode.org/brainstorm/wp-content/uploads/2007/07/spam_1.thumbnail.jpg" alt="SPAM" align='right' />

La batalla continua indefinidament: es milloren els motors antispam ([spamassassin][1], [dspam][2]...) i els spammers s'adapten, s'usen llistes negres remotes ([RBL][3]&#8216;s) i ells s'adapten... que un gif com attachment pot tenir pinta d'spam ? Doncs ells usen [PDF's][4]... el compte de mai acabar :/

Doncs bé, fart de rebre una mitja de 10 spams diaris a l'Inbox vaig decidir implantar mesures dràstiques. Primer vaig probar amb [greylisting][5] (usant [postgrey][6]). El resultat va ser decepcionant: apenes filtrava un 10% més dels spams respecte a no tenir aquesta mesura. 

La capacitat d'adaptació i perseverança dels spammers em recorda a uns personatges d'[una sèrie][7] de ciència ficció :-/... El cas és que finalment em vaig decantar per [<acronym title='Sender Policy Framework'>SPF</acronym>][8]... conclusió després de setmanes d'ús i revisió de logs exhaustiva: tant de bo ho hagués fet abans !

<!--more-->

Veiem per una banda, l'acció del dimoni SPF que uso junt amb postfix, [policyd-weight][9]:

<pre>Aug 27 16:21:33 nopcode postfix/policyd-weight[16924]: weighted check:  IN_DYN_PBL_SPAMHAUS=3.25 
NOT_IN_SBL_XBL_SPAMHAUS=-1.5 NOT_IN_SPAMCOP=-1.5 NOT_IN_BL_NJABL=-1.5 CL_IP_NE_HELO=4.75
 RESOLVED_IP_IS_NOT_HELO=1.5 (check from: .ba88. - helo: .[122.254.168.34]. - helo-domain: .34].)
FROM_NOT_FAILED_HELO(DOMAIN)=6.25 &l;tclient=122.254.168.34&gt; &lt;helo=[122.254.168.34]&gt;
&lt;from=vcardwell@ba88.com&gt; &lt;to=hunnopcodeveg at nopcode org&gt;, rate: 11.25
</pre>

<pre>Aug 27 16:21:33 nopcode postfix/policyd-weight[16924]: decided action=550 Mail appeared to be SPAM 
or forged. Ask your Mail/DNS-Administrator to correct HELO and DNS MX settings or to get removed from
 DNSBLs; MTA helo: [122.254.168.34], MTA hostname: unknown[122.254.168.34] 
(helo/hostname mismatch); delay: 2s
</pre>

I per l'altra l'acció de postfix actuant en conseqüència (rebutjant el mail):

<pre>Aug 27 16:21:33 nopcode postfix/smtpd[3393]: NOQUEUE: reject: RCPT from 
unknown[122.254.168.34]: 550 5.7.1 &lt;hunnopcodeveg@nopcode.org&gt;: Recipient address rejected:
 Mail appeared to be SPAM or forged. Ask your Mail/DNS-Administrator to correct HELO and DNS MX 
settings or to get removed from DNSBLs; MTA helo: [122.254.168.34], MTA hostname: 
unknown[122.254.168.34] (helo/hostname mismatch); from=&lt;vcardwell@ba88.com&gt; 
to=&lt;hunnopcodeveg at nopcode org&gt; proto=ESMTP helo=&lt; [122.254.168.34]>
</pre>

L'avantatge d'aquest sistema es veu en aquest log: l'spammer no ha passat del [RCPT TO][10] ! Això vol dir dues coses:

1.  Estalvi d'ample de banda: spammer, queda't el cos del missatge
2.  Estalvi en temps de CPU: Els filtres bayesians descansen

Pero no tot són meravelles a SPF, s'han de tenir ben presents els [greus problemes que comporta][11] implantar aquest sistema. Tot i així, i sent conscient dels seus problemes, SPF ha conseguit retornar la calma al meu INBOX, benvingut ! :) 

Ah ! He mostrat la banda "interna" d'SPF, aqui teniu l'externa (la que publico via registre TXT de DNS):

<pre>$ dig -t txt +short nopcode.org
"v=spf1 a mx a:lists.nopcode.org mx:mail.nopcode.org -all"
</pre>

 [1]: http://spamassassin.apache.org/
 [2]: http://www.nuclearelephant.com/
 [3]: http://en.wikipedia.org/wiki/Real-time_Blackhole_List
 [4]: http://efectomariposa.wordpress.com/2007/07/31/pdf-spam/
 [5]: http://en.wikipedia.org/wiki/Greylisting
 [6]: http://postgrey.schweikert.ch/
 [7]: http://en.wikipedia.org/wiki/Borg_%28Star_Trek%29
 [8]: http://en.wikipedia.org/wiki/Sender_Policy_Framework
 [9]: http://www.policyd-weight.org/
 [10]: http://en.wikipedia.org/wiki/SMTP
 [11]: http://david.woodhou.se/why-not-spf.html