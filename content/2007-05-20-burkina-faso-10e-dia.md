---
author: brainstorm
date: '2007-05-19T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2007-05-19T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/05/20/burkina-faso-10e-dia/
tags:
- travel
title: "Burkina Faso: 10\xE8 dia"
---

Avui arribem força d'hora a l'ESI, pero de seguida veiem que algo passa: tots els estudiants són fora de l'edifici en actitud de protesta. Es tracta d'una vaga per reclamar més diners destinats a l'educació. El delegat de classe ens informa de la situació i ens comunica que avui no hi ha classe. Aprofitaré doncs per anar per feina amb el servidor.

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2911747165/" title="Servers on burkina" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3258/2911747165_84e22a01a6_m.jpg" alt="Servers on burkina" class="" /></a>
  </center>
</div>

Abans però, en Xavi em demana que em miri un portàtil suposadament avariat que s'arregla automàgicament :) Després em dedico a informar als administradors sobre els progressos que vaig assolint amb el servidor. Al cap i a la fi, ells es faràn càrrec d'aquesta màquina quan jo no hi sigui i han d'estar preparats. Per tal de fer una petita prova, connectem un dels PC's de la mateixa sala al servidor, formant una petita xarxa local d'un sol pc. Resulta ser suficient per veure que les regles que havia definit al firewall són massa restrictives i les rectifico. També activo l'IP forwarding, després de tot, volem proxy per web, no per a tota connexió.  
<!--more-->

Resulta impactant veure en els logs la quantitat d'atacs que rep el firewall desde internet. Fins ara l'estratègia de firewalling a l'ESI consistia en instal·lar firewalls d'aplicació de windows en tots i cada un dels ordinadors... i resar perque l'usuari no desactivés el firewall :-S Això suposa un risc de seguretat important per l'ESI, i de fet, els virus proliferen dins d'aquesta xarxa. Sense anar més lluny, el pendrive d'en Xavier ja té tres virus provinents d'una de les màquines dels laboratoris. En aquest sentit, els administradors es queixen perque no dónen l'abast pel que fa a reinstal·lar windows sovint. Trobo que donades aquestes circumstàncies és **normal**.

Comprovant algunes de les IP's que figuren als logs, veiem que la gran majoria provenen de sites dedicats a infectar màquines (botnets), spammers i d'altre malware variat.

Amb el permís dels administradors llenço un tcpdump per tenir una idea de quin tràfic circula per aquesta xarxa. A part de les peticions habituals a pagines web, hi ha una quantitat important d'ample de banda que es desperdicia per culpa de **windowsupdate.microsoft.com**. La següent escenari il·lustra el perquè:

*   X ordinadors amb windows. X descàrregues de Y actualitzacions cadascun.
*   Y actualitzacions de Z MB cada un.

*   X*Y descàrregues redundants a través del ja saturat enllaç de l'ESI
*   Un total de X\*Y\*Z MB descarregats (pot arribar a ser del ordre de GB!)

Trobo la solució en un dels llibres que m'he endut al portàtil i va com un guant per aquest problema: [WNDW][1]. La solució passa per definir un registre al servidor DNS local que suplanti **windowsupdate.microsoft.com** per una màquina de la xarxa local (degudament configurada) que actuarà de servidor d'updates dels clients windows. D'aquesta forma només Y*Z MB són descarregats un sol cop i després distribuïts als clients per xarxa local. Li comento a en Bob la solució i la idea li sembla bé, s'encarregarà del tema.

A última hora, decideixo posar en producció el servidor, canviant el PC on fins ara estava connectat per la xarxa de l'ESI:

*   Bob, ets conscient que desconnectaré a tots els usuaris ara mateix ? Estas segur ?
*   Clar que sí home, cap problema ! Fes, fes !
*   M'encanta això, tant de bo pogués fer-ho amb aquesta soltura a la feina X"D

En Bob riu una bona estona, deu pensar que els administradors europeus estem carregats de punyetes pel que fa al tracte amb l'usuari :) Amb tot el respecte li explico el concepte de [<acronym title='Bastard Operator From Hell'>BOFH</acronym>][2] i en tenim per una bona estona amb els riures :P 

Després de tot, l'invent funciona correctament i al cap d'uns minuts ja hi ha varies webs en el proxy-cache i més de 600 dominis en la cache de BIND.

<pre># rdnc dumpdb -cache
# wc -l /var/cache/bind/dumpdb
655
</pre>

Per a celebrar la posada en marxa de la màquina, li pregunto a un dels estudiants què fan per el vespre-nit i quedem per anar a un "Maqui", que és el nom que reben aqui els bars amb terrassa. Entre riures i anécdotes ens anem coneixent tots, sembla que tots tenen molt inculcat que a part d'informàtics han de ser homes de negocis que maneguin molts diners. Entre els càrrecs més repetits dels "jo vull ser..." figura el de <acronym title='DataBase Administrator'>DBA</acronym>. Intercambien experiències, costums, etc. Per exemple, els nois em pregunten què busquen les noies catalanes en els africans... no tinc més remei que respondre-li que si ho sabés li diria encantat i que el desconeixement és general, no només en el cas africà... noies, si voleu comentar e ilustrar-me estaré encantat :P. Ah! Per cert, aqui es dóna la mà a les noies per saludar, tot i això accepten de bon grat els dos petons als que estem acostumats a Barcelona :) Em comenten que només es fa aquesta salutació en ocasions especials.

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2911761329/" title="Hanging out in Burkina" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3249/2911761329_5fe0c8d56d_m.jpg" alt="Hanging out in Burkina" class="" /></a>
  </center>
</div>

De tornada cap a l'hotel, la cuina de l'hotel ja està tancada... per sort no tenia gana igualment, així que decideixo anar a dormir i demà ja esmorçaré fort per compensar :)

 [1]: https://wndw.net/
 [2]: https://en.wikipedia.org/wiki/BOFH