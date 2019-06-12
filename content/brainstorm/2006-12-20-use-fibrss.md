---
author: brainstorm
date: '2006-12-19T13:00:00.000000+00:00'
dsq_thread_id:
- 2874888498
excerpt: null
layout: post
modified: '2006-12-19T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/12/20/use-fibrss/
tags:
- inet
- university
- unix
title: use FIB::RSS;
---

En el post anterior comentava alguns mòduls que ens poden fer la vida més fàcil si volem extreure dades d'una web de forma automàtica. Em vaig oblidar però de mencionar l'extensió del firefox [LiveHttpHeaders][1], sant grial de l'spidering que vaig descobrir gràcies a l'[esteve][2]. Si resulta que la pàgina només carrega amb explorer, podeu provar sort amb [IEHttpHeaders][3] o [Fiddler][4].

També en el post anterior vaig deixar en suspens les raons que em van portar a mirar-me tots aquests móduls. Doncs bé, saludo des d'aqui a [Jordi][5], que em va picar prou com per fer un feed dels PFCs de la facultat usant els mòduls que vaig explicar, ell ja en sap els motius :)  
<!--more-->

  
Nomes cal que apunteu el vostre lector de feeds preferits cap a <http://nopcode.org/fib/rss20.xml> i sabreu quan, on i qui presenta PFC's periòdicament a la [FIB][6].

<strike>El codi el publicaré d'aqui uns dies, pero abans, podeu probar a generar un feed amb les notícies de la FIB usant el següent template i els móduls que he descrit, aviam com us en sortiu ;) </strike>

**UPDATE:** Que tal aquests scripts ? Podeu pastejar codi a comments si voleu... aqui teniu la meva [solució][7].

<!--more-->

<pre>my $news_template = &lt;&lt; NEWS_FIB;
[% FOREACH record %]
[% ... %]
&lt;b class=titol&gt;[% titol %]&lt;br /&gt;[% ... %]&lt;p&gt;[% noticia %]&lt;br /&gt;&lt;br /&gt;[% ... %]&lt;img src=/imatges/q0.gif/&gt; &lt;a href=[% url %]&gt;
[% END %]
NEWS_FIB
</pre>

Ah, m'oblidava de dues coses importants:

*   Per generar el feed he fet servir [XML::RSS][8], molt fàcil de fer anar
*   M'he inspirat en un llibre boníssim d'Oreilly sobre el tema: [Web Spidering Hacks][9]

Podeu enviar també templates de pàgines que trobeu interessants de fer-ne web spidering... com per exemple [telepizza][10], qui no ha volgut mai encarregar una pizza simplement executant un script en perl ? ;P

Ja posats a demanar... si domineu algun altre llenguatge, m'agradaria que comentessiu quins mòduls useu/usarieu per fer anar tot això. 

Happy web spidering !

 [1]: http://livehttpheaders.mozdev.org/
 [2]: http://esteve.tizos.net/archives/mozilla-live-http-headers/
 [3]: http://www.blunck.se/iehttpheaders/iehttpheaders.html
 [4]: http://www.fiddlertool.com/fiddler/
 [5]: http://reloadcity.freehostia.com/
 [6]: http://www.fib.upc.edu/
 [7]: http://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/12/propers_pfc.pl
 [8]: http://search.cpan.org/~abh/XML-RSS-1.22/lib/XML/RSS.pm
 [9]: http://www.oreilly.com/catalog/spiderhks/
 [10]: http://www.telepizza.es/