---
author: brainstorm
date: '2005-09-05T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-09-05T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/09/06/faxos-amb-hylafax/
tags:
- unix
title: Faxos amb Hylafax
---

Sembla que a [diariolinux][1] se m'han avançat mentre tenia aquest post a mitjes, tot i així tinc ganes d'explicar la meva experiència, així que posem-nos mans a la obra :) 

#### Primers passos

Ja sigui per motius de feina, donar-se de baixa d'algun servei de telefonia, tràmit burrocràtic, etc... tots hem passat pel tràngol que suposa enviar un fax, sobretot quan no en tens cap a l'abast i et corre pressa.

És el moment doncs, de rescatar el nostre mòdem, (en el meu cas es tracta d'un USRobotics Sporster 33.6 extern), connectar-lo al server i fer un:

<pre># emerge hylafax</pre>

<!--more-->

  
Un cop instal·lat, executem **faxsetup** tal com ens indica l'ebuild al finalitzar:

<pre>Now run faxsetup and (if necessary) faxaddmodem.</pre>

Jo no vaig necessitar córrer faxaddmodem, ja que faxsetup et genera els fitxers de configuració necessaris, inclosa la configuració del módem (ja postejaré els fitxers si algú li interessen). Això sí, és important afegir la línea següent al inittab per rebre/enviar faxos:

<pre>s0:12345:respawn:/usr/sbin/faxgetty ttyS0</pre>

Podem comprobar que faxgetty es comunica correctament amb el modem fent **faxgetty -D** i veient si al /var/log/messages apareix un text similar al següent:

<pre>Sep  5 13:37:41 nopcode FaxGetty[15276]: MODEM USR USRobotics Sportster Voice 33600 Fax Rev. 2.0/Configuration Profile...
Product type                                        Spain External Options      V32bis,V.FC,V.34+ Fax Options            
Class 1/Class 2.0 Clock Freq             92.0Mhz Eprom                  256k Ram
64k EPROM date                                10/18/96 DSP date            10/18/96 EPROM rev 2.0  DSP rev 2.0
</pre>

Un cop comprovat que funciona **faxgetty**, llençem l'hylafax (i el col·loquem al default runlevel):

<pre>/etc/init.d/hylafax start && rc-update add hylafax default</pre>

Llestos per a enviar i rebre faxos :) 

#### Com rebre/enviar fax-emails ?

Amb **hylafax** és possible enviar un email a una direcció de l'estil "NomCognoms@93666345213.fax" i el nostre sistema de correu combinat amb **hylafax** l'enviarà directament al numero de destí, maco i simple, no ? :) 

I què pot ser més pràctic que rebre faxos a la nostra bústia de correu en format PDF ? Vegem com ho muntem tot això:

Per a rebre'ls creo un fitxer anomenat **FaxDispatch** a /var/spool/fax/etc:

<pre>FILETYPE=pdf; # també estan suportats els formats: ps, tiff
SENDTO=FaxMaster; #Aqui pot anar qualsevol compte de correu del sistema
</pre>

Per a enviar-los uso postfix, per tant creo un *transport* que crida el programa mailfax. Per a fer-ho edito els següents fitxers de configuració de postfix:

main.cf

<pre>fax_destination_recipient_limit = 1
</pre>

master.cf

<pre>hylafax   unix  -       n       n       -       1       pipe
                flags= user=fax argv=/usr/local/bin/mailfax ${user} ${recipient} ${sender}
</pre>

**Nota:** [Aqui][2] podeu trobar les diferents variants del programa "mailfax" per a postfix, sendmail, qmail i smail.

#### Altres sistemes d'enviament

També podem usar [CUPS][3] per enviar un document a una impressora virtual que farà la funció de fax. Es pot usar [Fax4Cups][4] per a fer-ho, tot i així de moment tinc un [problema][5] amb el parseig de .ps's que no he conseguit solventar encara, editaré el post si consegueixo solucionar-ho (he provat de posar en mode raw en cups, tal com es comenta en el missatge, però tampoc funciona i l'autor del missatge no respon).

També és possible fer-ho via samba, clients específics, etc... A la web de [hylafax][6] està tot molt ben documentat.

#### Fax analògic ? go VoIP !

Per últim em queda comentar que tard o d'hora VoIP predominarà i s'ha d'estar preparat per migrar també els faxos a aquest sistema, per això deixo un [link][7] per si algú s'anima a configurar un sistema de faxos amb [asterisk][8].

 [1]: http://www.diariolinux.com/tiki-read_article.php?articleId=21&page=1
 [2]: ftp://ftp.hylafax.org/source/CVS/faxmail/
 [3]: http://www.cups.org/
 [4]: http://vigna.dsi.unimi.it/fax4CUPS/
 [5]: http://www.hylafax.org/archive/2002-09/msg00167.html
 [6]: http://www.hylafax.org/links.html#clients
 [7]: http://www.voip-info.org/wiki-Asterisk+fax
 [8]: http://www.asterisk.org/