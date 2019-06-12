---
author: brainstorm
date: '2007-05-12T14:00:00.000000+00:00'
dsq_thread_id:
- 2874890078
excerpt: null
layout: post
modified: '2007-05-12T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/05/13/burkina-faso-5e-dia/
tags:
- travel
title: "Burkina Faso: 5\xE8 dia"
---

Em desperta simultàniament el telèfon de l'habitació i l'alarma programada al mobil :-S Comprovo rutinàriament signes de picades... cap ni una... de fet, no hi ha cap mosquit a l'habitació. Sembla que l'ús massiu d'espirals antimosquits fa efecte més tard del que em pensava :-S

Després del senzill però molt saludable esmorçar a base de suc de taronja & papaya al que ja ens tènen acostumats, anem cap a l'ESI.

Les dues hores de classe d'avui les dedico a mostrar la part més pràctica dels conceptes bàsics de wireless que he mostrat a les dues anteriors. Porto un dels routers linksys a classe, el connecto i mostro l'interfície administrativa i explico com funciona i què significa cada una de les parts (gràcies Jose per el consell ;)). Abans d'acabar la classe, pregunto què és <acronym title='Wired Equivalent Privacy'>WEP</acronym> a una alumna. No sembla que tingui massa clar el concepte així que deixo 10 minuts en total perque usant apunts dels seus companys & el llibre, elabori una exposició. La resposta és brutal, pràcticament tota la classe repassa apunts i discuteixen entre sí. 

<div class='flickr_photo'>
  <center>
    <a href="http://www.flickr.com/photos/rvalls/2912221614/" title="Students reviewing notes Burkina Faso" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3172/2912221614_8e0cd606e4_m.jpg" alt="Students reviewing notes Burkina Faso" class="" /></a>
  </center>
</div>

<!--more-->

  
Mentre preparen l'exposició, aprofito per veure què fa la última fila de la classe on alguns dels alumnes tènen portàtils... estan revisant pàgines del CERT sobre seguretat WiFi ! Boníssim ! Faig sortir a tres persones (dues noies i un noi) a fer l'exposició i es defensen prou bé <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" /> 

Descobreixo més coses sobre l'antic radioenllaç Nasso-Bobo gràcies a les explicacions de Mesmin. A simple vista es veu un cable de baixa pèrdua LMR400 connectat a un pigtail N a SMA que al seu torn està connectat a un sistema embedded que haig de determinar què fa exactament.

Junt amb Mesmin, dediquem part del matí i tota la tarda a muntar el punt d'accés i col·locar l'antena. Finalment usem la mateixa torre que subjecta l'antiga antena.

<div class='flickr_photo'>
  <center>
    <a href="http://www.flickr.com/photos/rvalls/2911461279/" title="Installing wifi antenna on Burkina Faso" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3122/2911461279_719a5472d4_m.jpg" alt="Installing wifi antenna on Burkina Faso" class="" /></a>
  </center>
</div>

Com es pot veure a les fotos, la forma d'accedir a l'antena no és la més segura del món. Conec a més d'un tècnic en prevenció de riscos laborals que s'estiraria dels cabells veient la nostra forma de treballar.

<div class='flickr_photo'>
  <center>
    <a href="http://www.flickr.com/photos/rvalls/2912288960/" title="Burkina Faso risk prevention" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3047/2912288960_94f28758f2_m.jpg" alt="Burkina Faso risk prevention" class="" /></a>
  </center>
</div>

Després d'alguna confusió amb les connexions del <acronym title='Power Over Ethernet'>POE</acronym>, conseguim fer anar el punt d'accés i funciona a la perfecció. No és necessari posar-ne un segon AP com a repetidor, la senyal és molt bona. Tal com varem acordar a Barcelona, un o dos punts d'accés es cediran als alumnes per a que puguin practicar (en concret a l'associació de Linux de la facultat :-)).

Tot i així, la connexió a internet és molt lenta, és precisament el punt en el que procuraré treballar la setmana vinent (configurant un cache dns (named o similar) i web (squid)). Demà però, toca descansar (dissabte) i fer una mica de turisme.