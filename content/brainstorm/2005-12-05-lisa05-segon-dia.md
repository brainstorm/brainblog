---
author: brainstorm
date: '2005-12-04T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-12-04T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/12/05/lisa05-segon-dia/
tags:
- security
- software
- travel
- unix
title: 'LISA''05: Segon dia'
---

Avui tocava ["Security without firewalls"][1]... sonava interessant... **sonava**. Ha acabat sent un cúmul de " basic good admin practices", tot i això, una tool que ha mencionat m'ha cridat l'atenció: [cfengine][2]. Permet centralitzar la configuracions heterogenees de diversos hosts, evitant haver de mantenir cada màquina individualment e incorpora mecanismes de recuperació per fer fallback a un estat conegut i funcional (per exemple quan es produeix un update que trenca configuracions). Per als qui els agraden els [replacements][3], també existeix [Puppet][4] que es basa en el mateix principi i està escrit en ruby.

Cap a la tarda he conseguit apuntar-me sense pagar (sóc un estudiant esponsoritzat per google segons posa a la tarja d'accés :-P) a ["Google web driven development"][5] by [Deryck Hodge][6], molt bon orador. Aquesta ja m'ha agradat bastant més, ha explicat una mica per sobre google, gmail i l'API de [google maps][7], tot posant [petits exemples][8] amb <acronym title="Asynchronous JavaScript and XML"><a href="http://en.wikipedia.org/wiki/AJAX">AJAX</a></acronym>. Ens ha ensenyat un debugger de javascript molt complet: [Venkman Javascript debugger][9] per mozilla. També ha recomanat alguns frameworks que poden anar bé per desenvolupar amb AJAX: [dojo][10], [prototype][11] i [qooxdoo][12]. Ah ! Que no m'oblidi de la petita però potent eina de [CLI][13]: [libgcli][14] feta per el mateix autor de la xerrada.  
<!--more-->

  
Finalment per culminar el dia, hem anat a un <acronym title="Birds of Feather">BoF</acronym> que feien pel vespre: [An Evening with MAKE Magazine][15]... que ha estat MOOLT guapo. Només entrar, en la tela del projector un [MAME corrent PACMAN][16] en una atari 2600 (amb la circuïteria interior reemplaçada per una placa mini-itx). Ens han explicat uns quants hacks relativament senzills de hardware, entre els quals es troben:

*   Mando "universal" de parkings: Amb un comptador intern, envia senyals usant totes les possibles configuracions del [switch DIP][17] (típic en els mandos de pàrking)
*   Anonymous megaphone: Mòbil connectat a un amplificador i a un altaveu per espantar la gent remotament, segons comentava l'autor de la xerrada XD.

No us perdeu les [slides d'aquesta xerrada][18] ! En una frase, [Make:][19] magazine és per aquells que els agraden pàgines a l'estil [hackaday][20] i disfruten fent els seus hardware hacks.

Després ha aparegut en [bunnie][21], famós per el seu llibre ["Hacking the Xbox"][22] entre altres [hacks][23], i ens ha explicat algunes de les tècniques que usa per fer [enginyeria inversa directament en silici][24] (observant directament els components i pistes dels xips)... impressionant.

Al tornar cap al hostel ens hem trobat que tots els restaurants estaven tancats !! :( A les 10pm ! Per sort hem trobat un mexicà que estava obert fins tard... tot i que el menjar deixava bastant que desitjar (qui té estomac per menjar un "burrito" de monjetes xafades amb formatge a les 11 de la nit ?... ~7 del matí a BCN ?).

 [1]: http://www.usenix.org/events/lisa05/training/tutonefile.html#m7
 [2]: http://www.cfengine.org/
 [3]: http://news.nopcode.org/pancake/
 [4]: http://reductivelabs.com/projects/puppet
 [5]: http://www.usenix.org/events/lisa05/training/tutonefile.html#m11
 [6]: http://devurandom.org/
 [7]: http://www.google.com/apis/maps/
 [8]: http://romeo.lib.auburn.edu/gkit/tests/
 [9]: http://www.mozilla.org/projects/venkman/
 [10]: http://dojotoolkit.org/
 [11]: http://prototype.conio.net/
 [12]: http://qooxdoo.oss.schlund.de/
 [13]: http://en.wikipedia.org/wiki/Command_line_interface
 [14]: http://www.devurandom.org/blog/googledevel#gcli_0.2_released
 [15]: http://www.usenix.org/events/lisa05/make.html
 [16]: http://blogs.nopcode.org/brainstorm/wp-content/images/play_pacman.jpg
 [17]: http://en.wikipedia.org/wiki/Dip_switch
 [18]: http://www.usenix.org/events/lisa05/make/grand.pdf
 [19]: http://www.makezine.com/
 [20]: http://www.hackaday.com/
 [21]: http://blogs.nopcode.org/brainstorm/wp-content/images/bunnie_circuits.jpg
 [22]: http://hackingthexbox.com/
 [23]: http://www.bunniestudios.com/?page_id=6
 [24]: http://www.bunniestudios.com/?page_id=14