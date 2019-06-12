---
author: brainstorm
date: '2006-06-14T14:00:00.000000+00:00'
dsq_thread_id:
- 2874888746
excerpt: null
layout: post
modified: '2006-06-14T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/06/15/asterisk-perlagi/
tags:
- software
- unix
title: Asterisk Perl::AGI
---

Bé, després d'un bon munt de temps de silenci, per fi he acabat examens finals projectes & pràctiques varies ! :) Volia comentar una petita part del que hem estat fent com a projecte de xarxes i sistemes operatius ([PXCSO][1]) que trobo força interessant. 

Bàsicament he tocat per primer cop la AGI via perl (que ve a ser com els CGI's però per Asterisk). Obviament el ebuild (facilet) per gentoo, ha caigut :-P:

<pre>inherit perl-module

DESCRIPTION="Perl AGI interfacing with the Asterisk open source pbx system"
HOMEPAGE="http://asterisk.gnuinter.net/"
SRC_URI="http://asterisk.gnuinter.net/files/${P}.tar.gz"
LICENSE="Artistic GPL-2"
SLOT="0"

KEYWORDS="~x86 amd64 ~ppc ~sparc ~alpha"

DEPEND="${DEPEND}"
</pre>

El cas és que necessitavem aquest mòdul juntament amb dos dels meus mòduls preferits del CPAN: [WWW::Mechanize][2] & [HTML::TokeParser][3], per tal d'agafar els horaris d'assignatures de la web de la facultat (tècnica anomenada *screen scraping* o *web spidering*) i mitjançant [festival][4] sintetitzar strings de l'estil:

> Són les 8:00, et toca classe de PXCSO a l'aula A5102

<!--more-->

Potser pensareu... com s'ha complicat la vida, amb la d'alternatives que hi ha (demanar dump de la BBDD, demanar que generin horari amb XML, etc...) pero ei, estem a la universitat i és divertit provar coses noves, no ? ;) 

Finalment l'invent va funcionar força bé, tot i que canviar l'idioma del festival no és tasca trivial perquè:

1.  Esta tot molt poc [documentat][5]
2.  Els fitxers de configuració estan en [Scheme][6] :-! 

Però la tonteria que més temps em va tenir batallant va ser el pas de variables entre la Perl::AGI i les extensions d'asterisk... preneu nota si us interessa:

> Al assignar una string a una variable, els espais en blanc al .pl s'han d'escapar amb "\ " !!!

Exemple:

<pre>$AGI->set_variable('isyhorari', 'No\\ tienes\\ classe');
</pre>

Altrament, quan llegiu la variable $isyhorari al extensions.conf del vostre asterisk (usant exten => 2,n,NoOP(${isyhorari}), p.ex) tant sols sortirà "No"... bé, alguns potser ho trobareu obvi, però jo vaig pagar la novatada trencant-me les banyes i llegint un bon piló de documentació :_/.

Coneixeu altres casos on heu escapat l'espai en blanc d'aquesta forma ? És relativament comú trobar-s'ho o és un cas raro ?

 [1]: http://www.fib.upc.es/ca/Estudis/Assignatures/PXCSO.html
 [2]: http://search.cpan.org/~petdance/WWW-Mechanize-1.18/lib/WWW/Mechanize.pm
 [3]: http://search.cpan.org/~gaas/HTML-Parser-3.54/lib/HTML/TokeParser.pm
 [4]: http://www.cstr.ed.ac.uk/projects/festival/download.html
 [5]: http://forums.gentoo.org/viewtopic.php?t=79383&highlight=festival
 [6]: http://en.wikipedia.org/wiki/Scheme_programming_language