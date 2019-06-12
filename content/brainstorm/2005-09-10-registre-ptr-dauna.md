---
author: brainstorm
date: '2005-09-09T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-09-09T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/09/10/registre-ptr-dauna/
tags:
- inet
title: Registre PTR d'Auna
---

Després de més d'un mes de mails i faxos enviats a "Gestión de dominios de Auna TLC", per fí he aconseguit que el meu actual ISP, canvii el registre [PTR][1] dels seus servidors DNS.

Per a que volia aquest canvi ? Doncs bàsicament per esborrar el meu servidor de les llistes negres d'spam (per el simple fet de ser un usuari d'un ISP amb una pool d'IP's dinàmiques).

En el moment d'escriure aquestes línees, [dnsstuff][2] indica que el meu servidor està llistat a tres bases de dades d'spam. La que més em preocupa és SORBS-DUHL, ja que alguns servidors de correu coneguts usen aquesta llista per refusar mails entrants (per exemple vodafone.es).  
<!--more-->

  
Un dels requisits mínims que demanen les llistes negres és que la nostra ip, resolgui el nostre domini i viceversa, aquí entra el perquè canviar el <acronym title="PoinTer Record">PTR</acronym>:

> 62.57.0.38 has reverse dns of 38.red-62-57-0.user.auna.net. If your domain name does not appear as the last components in any of those reverse dns names, that needs to be fixed first. Also, none of those names have a dns A record pointing to the original 62.57.0.38. That needs to be fixed. 

Un cop canviat el registre per part d'auna i passades les 24-48 hores que triguen en propagar-se els canvis, he passat de veure això:

<pre>$ host 62.57.0.38
38.0.57.62.in-addr.arpa domain name pointer 38.red-62-57-0.user.auna.net.
</pre>

A veure això :

<pre>$ host 62.57.0.38
38.0.57.62.in-addr.arpa domain name pointer nopcode.org.
</pre>

Si finalment retiren la meva ip de la llista negra, escriuré un altre post explicant com fer-ho pas a pas ;)

 [1]: http://en.wikipedia.org/wiki/PTR_record
 [2]: http://www.dnsstuff.com/tools/ip4r.ch?ip=62.57.0.38