---
author: brainstorm
date: '2007-05-19T14:00:00.000000+00:00'
dsq_thread_id:
- 2874890097
excerpt: null
layout: post
modified: '2007-05-19T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/05/20/burkina-faso-13e-dia/
tags:
- travel
title: "Burkina Faso: 12\xE8 dia"
---

Avui és l'últim dia que estarè per Nasso i concretament a l'ESI. Així que toca acabar de lligar al màxim els temes pendents que puc acabar amb el poc temps que em queda. Xavi accepta de bon grat substituïrme en dues hores de classe que em correspondrien pel matí per tal que pugui enllestir els temes tècnics (gràcies!). Bob em va donar la clau de l'edifici on hi ha els equips, així que a primera hora vaig per feina: entro, connecto tots els aparells i engego el servidor. Al llarg del matí em dedico a instal·lar [webmin][1] per que els administradors tinguin una forma fàcil d'administrar el servidor (no els fa massa gràcia encara la línea de comandes :-P). Deixo una de les consoles del sistema llesta perque llençi un:

<pre>multitail -f /var/log/messages -f /var/log/squid/access.log
</pre>

D'aquesta forma tènen a primera vista què esta passant al servidor. A mesura que passa el matí, cada cop més usuaris fan ús dels punts d'accés instal·lats al campus i de la pròpia xarxa. Sembla que tot funciona com hauria :) 

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2912661400/" title="Shorewall logs screen on Burkina Faso" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3019/2912661400_b3b41a75f5_m.jpg" alt="Shorewall logs screen on Burkina Faso" class="" /></a>
  </center>
</div>

Decideixo doncs cridar als administradors per ensenyar-los com està muntat el tinglado i em traiciona l'efecte demo: Deixa d'atendre peticions. Una inspecció ràpida revela un cable ethernet en mal estat que es desconnecta a la mínima. Procurem substituïr-lo per un de nou et voilà, tot correcte. Aprofitant que estem amb tema cables, aprofita per portar-me el cable de consola del cisco 2600:

<pre># apt-get install minicom
# minicom /dev/ttyS0
ESI> enable
ESI# show running-config
</pre>

<!--more-->

Les coses no podien anar millor, tinc accés als settings del router, fet que m'ajuda a descobrir que disposen de 5 IP's públiques  
contractades entre d'altres paràmetres rellevants. Tot i ja començar a estar curat d'espants en aquest indret del món, trobar-me amb un router principal sense password definit em posa els pels de punta igualment :-S

Tot just acabo d'explicar-li a l'Abdul com fer anar webmin per controlar tots els serveis i apareix en Bob. Per no tornar a repetir tota l'explicació li demano que li transmeti tot lo que he dit, així m'asseguro que m'he explicat bé i queda tot clar. Al acabar el tutorial improvisat, els passo MB's de material d'administració de sistemes, xarxes, seguretat, una ubuntu server i una [backtrack 2][2] (distribució de penetration testing en liveCD). Ara tènen material de sobres per aprendre el que es proposin :) 

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2911823525/" title="Webmin powered admin in Burkina Faso" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3262/2911823525_d6d7f4ae06_m.jpg" alt="Webmin powered admin in Burkina Faso" class="" /></a>
  </center>
</div>

Es mostren satisfets per la feina feta i em comenten que s'ha notat un speedup a la xarxa: m'envaeix una sensació d'orgull, satisfacció i descans :) De camí cap als menjadors, Xavi aprofita per mostrar als alumnes que internet funciona usant un dels portàtils que els hem cedit i l'access point que vàrem instal·lar al sostre de l'edifici:

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2912674322/" title="Xavier Messeguer showing connectivity on ESI campus" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3198/2912674322_05f90b4bd1_m.jpg" alt="Xavier Messeguer showing connectivity on ESI campus" class="" /></a>
  </center>
</div>

Per fer la proba, busco "burkina" a google, i trobem una notícia important per ells: La vaga del dijous ha sorgit efecte ! El govern destinarà més diners per als estudiants :) 

Després de dinar, preparo les últimes tres hores de classe que em queden. Procuro donar nocions bàsiques sobre sistemes en producció: Hardware, emmagatzemament, redundància i serveis de xarxa entre d'altres. Sent francs, simplement es tracta d'un petit recuit de la classe d'ASO que vaig donar fa un temps, pero trobo que els pot resultar interessant igualment. Com ja és habitual, mostren força interés i fan preguntes sobre els temes que vaig explicant. Al acabar la classe es presenten uns quants amb els seus respectius pendrives USB per a que els copii les transparències que he donat :-! També sento comentar a algun alumne que avui la connexió va més ràpida... yay ! :) 

Només queda una última cosa que puc fer amb el temps que em queda: flashejar un dels routers que tenien a l'ESI amb [OpenWRT][3] i deixar una nota perque dos d'aquests equips passin a formar part de l'associació de linux, per a que tinguin l'oportunitat d'aprendre i millorar ells mateixos la infraestructura de xarxa actual, que sigui dit de pas, tot i que funciona és molt millorable encara :) 

En el viatge d'anada al LCBOB, en Xavi comenta que mentre feia classe ha intentat fer fotos dels alumnes en va: un contrallum amb gent negra resulta ser molt més complicat que amb gent blanca, i els mateixos alumnes ho reconeixen X"D Tot i això ens en sortim en les fotos d'exteriors:

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2911843905/" title="ESI alumni Burkina Faso" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3093/2911843905_3d0529be36_m.jpg" alt="ESI alumni Burkina Faso" class="" /></a>
  </center>
</div>

Un cop hem acabat al LCBOB, passem per una gasolinera a repostar i ens estranya (encara!) sentir uns bens... pero on son ? Els han lligat al sostre de l'autobus del costat X"D

<div class='flickr_photo'>
  <center>
    <a href="https://www.flickr.com/photos/rvalls/2912700950/" title="Animals on bus rooftop" target="_blank" class="flickr-image aligncenter"><img src="http://farm4.static.flickr.com/3111/2912700950_60f1b6372e_m.jpg" alt="Animals on bus rooftop" class="" /></a>
  </center>
</div>

Després de fer la foto, se'ns acosten reclamant que els paguem algo (drets d'imatge? :P). Tot seguit en Xavi procura convençe'ls tot seriós que la foto anava dirigida als animals i que per tant ells no són mereixedors de la propina. Sens dubte les meves riallades desde el seient de darrere no ajuden massa a que s'empassin l'argument :) Athanase arrenca el cotxe i marxem abans que es posin més exigents.

Per acabar el dia, mentre sopem, descobreixo que en Xavi va fer la seva tesi doctoral usant la Thinking Machine que tantes vegades he vist pel departament i que ara forma part del museu: us imagineu com deu ser treballar amb una màquina que té **1024 processadors d'un sol bit** ? Doncs es veu que en la seva època era molt potent però l'empresa que fabricava aquestes màquines tant curioses va entrar en bancarrota... quines voltes dóna la indústria.

 [1]: https://www.webmin.com/
 [2]: https://www.remote-exploit.org/backtrack_download.html
 [3]: https://wiki.openwrt.org/