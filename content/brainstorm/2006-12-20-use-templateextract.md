---
author: brainstorm
date: '2006-12-19T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889372
excerpt: null
layout: post
modified: '2006-12-19T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/12/20/use-templateextract/
tags:
- inet
- software
- unix
title: use Template::Extract;
---

Fa un temps que sóc aficionat a fer scripts que fan [screen scraping][1] (web spidering). Els meus primers tantejos van ser amb el fantàstic mòdul [LWP][2]. Amb una mica de paciència, fent GETs i POSTs on tocava, [url-encodejant][3] i en alguns casos usant un grapat d'expressions regulars podia extreure les dades d'una pàgina web i mostrar-les en el format que m'interessava, emmagatzemar-les en una BD, etc...

Un bon dia vaig descobrir una altra **perl**a interessant a <acronym title='Comprehensive Perl Archive Network'>CPAN</acronym>: [WWW::Mechanize][4], que es comporta com un navegador (sense javascript). Alguns mètodes de l'API són:

<pre>$mech->follow( $link );
$mech->click( $button );
$mech->set_fields( %field_values );
$mech->back();
</pre>

Com podeu veure no és necessària una llarga explicació (imho), el seu comportament és el que us podrieu trobar en qualsevol navegador. Aquest mòdul ens simplifica força les accions que podem fer en el navegador, però el parseig l'hem de fer a mà igualment... fins que un bon dia, apareix a la meva vida en [Template::Extract][5] :)  
<!--more-->

  
Tothom que hagi fet algun CGI o programat amb templates sap generar codi html, doncs aquest mòdul fa l'operació inversa. Donat un document HTML ja format i un template que construïm nosaltres, ens extreu la informació que hem indicat. La [sinopsi][6] del mòdul mostra l'ús posant com exemple slashdot & microsoft.

Per acabar d'enroscar tot això, el mateix autor d'extract ([Autrijus Tang][7]) també va programar [Template::Generate][8]. Aquest mòdul ens genera el template ! Només cal indicar-li la informació d'exemple que voldriem extreure i ens retorna un possible template que fa match, ja no caldria doncs ni mirar-nos el codi font de la pàgina... maquíssim ! :) 

Alguns preguntareu a que ve exactament tot això... més en el pròxim post ! :)

 [1]: http://en.wikipedia.org/wiki/Screen_scraping
 [2]: http://search.cpan.org/dist/libwww-perl/lib/LWP.pm
 [3]: http://en.wikipedia.org/wiki/URL_Encoding
 [4]: http://search.cpan.org/dist/WWW-Mechanize/lib/WWW/Mechanize.pm
 [5]: http://search.cpan.org/dist/Template-Extract/lib/Template/Extract.pm
 [6]: http://search.cpan.org/dist/Template-Extract/lib/Template/Extract.pm#SYNOPSIS
 [7]: http://autrijus.org/
 [8]: http://search.cpan.org/~autrijus/Template-Generate-0.04/lib/Template/Generate.pm