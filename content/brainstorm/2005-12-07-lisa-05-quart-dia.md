---
author: brainstorm
date: '2005-12-06T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-12-06T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/12/07/lisa-05-quart-dia/
tags:
- security
- software
- travel
- unix
title: 'LISA ''05: Quart dia'
---

Abans d'anar a LISA, em vaig presentar voluntari per fer petits resums d'algunes de les conferències, aqui van alguns *quickies*:

La primera a la que vaig assistir al matí, la va donar en [Qi Lu][1], vice president de Yahoo. Va explicar una mica com se les arreglaven amb les noves formes de buscar/guardar continguts personals, com yahoo mail, per exemple. Va explicar les tècniques molt per sobre, lo únic que puc comentar és la idea general: per tal d'escalar bé dades personals (molt més difícils de gestionar que les dades públiques), tracten els conjunts de dades de manera similar a una cèl·lula biològica... quan arriba a crèixer massa, es divideix i es distribueix per tot el sistema conservant redundància, disponibilitat, etc... força curiós tot plegat.

Mentre dinàvem, al [Xavi][2] va comentar d'anar a "Ethereal and the art of debugging networks" [W8][3], i va estar força bé <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" /> 

Canviant de sala de l'hotel (ostres! encara no havia parlat de l'hotel... una mica "pijo" (veure foto)), vaig assistir a una xerrada sobre <acronym title="Grand Unified Logging Project"><a href="http://www.columbia.edu/acis/networks/advanced/gulp">GULP</a></acronym>, molt útil per entorns amb gran quantitat d'usuaris i servidors, permet relacionar ràpid i fàcilment logs de varis serveis (correu, dhcp, webmail, etc...) per resoldre incidències.

<center>
  <img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sd_hotel_hall.jpg' alt='town and country resort' />
</center>

  
<!--more-->

  
Després va aparèixer [Chaos][4] (nom curiós, no ?) i va parlar sobre com mesurar l'"atacabilitat" del codi dels servidors d'IMAP lliures més coneguts avui en dia: Courier-imap, cyrus-imap i UW-Imap. Podeu veure les [transpes][5], són poques però interessants ... **spoiler**: [courier-imap][6] guanya (poca attackability) <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_razz.gif" alt=":-P" class="wp-smiley" /> 

Tancant el "track" de vulnerabilitats i logging, en Ti-Min Wang va presentar [Strider ghostbuster][7]... ja veig els flames als comentaris "buu, ms research, etc...", bé doncs per sorprenent que sembli va ser una de les que més em va agradar d'aquest track, el personatge era força carismàtic i no parava de fer conyes comparant el seu soft amb matrix i el 7o sentido, i rajant d'slashdot... és irreproduïble aquí XD El soft en sí detecta rootkits de windows fent servir una tècnica anomenada cross-view diff (no confondre amb el típic diff de tota la vida), lo que fa és fer un diff de les crides a l'API de windows que es fan dins la màquina infectada i les que es veuen desde fora, força curiós.

El següent track va ser interessant però força aburridot... era sobre gestió de configuracions i soft management, si em permeteu, deixo les url's per qui s'ho vulgui mirar:

1.  Gestió de configuració amb MacOSX amb [Tetre2][8] i [SEPP][9].
2.  [Regcoll][10]: Gestió de registres de windows de varies màquines de forma centralitzada (una pallissa de xerrada, però pot arribar a ser útil per algú).
3.  [Herding cats][11], managing a mobile Unix platform: Interessant la part de gestió de certificats digitals en plan massiu

Per cert, pels curiosos, a flickr teniu el tag [lisa05][12] per si voleu veure fotos de l'event... no us perdeu la d'en [Thomas Limoncelli][13], autor de ["The Practice of System and Network Administration"][14] <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_wink.gif" alt=";-)" class="wp-smiley" />

 [1]: http://360.yahoo.com/profile-dHFl7togcqomOrUGtvI-
 [2]: http://oasi.upc.es/~freakhand/neverBlog/index.php
 [3]: http://www.usenix.org/events/lisa05/training/tutonefile.html#w8
 [4]: http://www.glassonion.org/
 [5]: http://www.glassonion.org/projects/imap-attack/slides.pdf
 [6]: http://www.courier-mta.org/imap/
 [7]: http://research.microsoft.com/rootkit/
 [8]: http://isg.ee.ethz.ch/tools/tetre2/
 [9]: http://www.sepp.ee.ethz.ch/
 [10]: http://coitweb.uncc.edu/~bbkang/isr/paper/RegColl_Kang_USENIX_LISA05.pdf
 [11]: http://rsug.itd.umich.edu/software/radmind/contrib/LISA05/
 [12]: http://www.flickr.com/photos/tags/lisa05/
 [13]: http://www.flickr.com/photos/betsys99/72560798/
 [14]: http://freshmeat.net/articles/view/338/