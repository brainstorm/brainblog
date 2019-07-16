---
author: brainstorm
date: '2006-08-02T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889822
excerpt: null
layout: post
modified: '2006-08-02T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/08/03/usrbingdb/
tags:
- bio
- software
- university
title: 'usr-bin-gdb'
---

No s'ha de posar pas un sh-bang per automatitzar una sessió de debugging repetitiva amb gdb, però el títol m'ha semblat encertat :P Tot i això gdb s'ho empassa, si ho voleu posar, és totalment inofensiu ;) 

Simplement cal fer ús del paràmetre "-x" i passar-li un fitxer de text amb les comandes que vols executar, una per línea, així de fàcil:

<pre>~> cat uni/tests/vect_tests/exec_gdb
file shifts
b main
r
display /t *a
display /t c
n
n
n
</pre>

<!--more-->

Tal com si treballessim interactivament amb el nostre gdb de sempre :) A continuació teniu el resultat d'executar part de l'script que acabo de mostrar (certament no ve massa a cuento, però em fa gràcia posar-ho):

<pre>> gdb -x exec_gdb
(...)
16              c = _mm_slli_si128(*a,1);
2: /t c = {0, 0}
1: /t *a = {1111111111111111111111111111111111111111111111111111111111111111,
  1111111111111111111111111111111111111111111111111111111111111111}
18              return 0;
2: /t c = {1111111111111111111111111111111111111111111111111111111100000000,
  1111111111111111111111111111111111111111111111111111111111111111}
1: /t *a = {1111111111111111111111111111111111111111111111111111111111111111,
</pre>

Aixx... -x, si t'hagués conegut abans... que bé va llegir la documentació (man gdb) de tant en tant, no trobeu ? :P
