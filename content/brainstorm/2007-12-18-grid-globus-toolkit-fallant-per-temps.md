---
author: brainstorm
date: '2007-12-17T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889094
excerpt: null
layout: post
modified: '2007-12-17T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/12/18/grid-globus-toolkit-fallant-per-temps/
tags:
- hardware
- inet
- software
- university
title: Grid (Globus Toolkit) fallant per temps
---

[<img id="image100" src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2007/12/globusalliance-nourl.gif" alt="Globus Toolkit" align='right' />][1]

A la [feina][2] disposem d'un grid d'unes 10 màquines per a fer [web crawling][3] muntades amb [Globus Toolkit][4] per a temes de recerca i estadístiques.

El cas és que últimament el rendiment d'aquest grid en quant a descàrregues de pàgines web estava molt per sota dels tases esperades. Com a administrador d'aquest grid, vaig seguir l'opció més usual per a fer *troubleshooting*: Revisar manuals, forums, etc... fins la sacietat per tal d'entendre el problema:

<pre>error: the server sent an error response: 425 425 Can't open data connection.
data_connect_failed() failed: an authentication operation failed
</pre>

<!--more-->

Després de revisar els scripts que generen aquest error per tal d'aïllar el problema, em vaig trobar amb la comanda que generava el missatge d'error anterior:

<pre>globus-job-run
</pre>

Permet executar comandes a nodes del grid... qualssevol invocació amb aquesta comanda fallava. 

Tenint en compte que Globus Toolkit és un software que funciona amb certificats digitals per tal de gestionar totes les seves accions amb seguretat, vaig pensar que valia la pena revisar-los... al cap i a la fi, el component més complex és el que acostuma a fallar amb més freqüència, no ? :) 

Doncs després d'una bona estona fent probes amb un entés en Globus del [BSC][5] al costat (gràcies Jorge !), ens vem adonar que es tractava d'un problema de sincronització horària !!! Tant els nodes com la consola rebutjavem l'execució de jobs perque els arribaven amb dates incorrectes (futures, per exemple).

Tant els nodes com la consola central, no estaven sincronitzats a l'hora correcta. Va ser qüestió de llençar el dimoni <acronym title='Network Time Protocol Daemon'>NTPD</acronym> per a cada un dels nodes \*i\* a la consola i tot va passar a anar com la seda... momentàniament.

La consola central tenia greus problemes per a mantenir el sincronisme amb servidors NTP externs: el rellotge propi de la màquina [divergia][6] massa ràpid per tal de mantenir l'hora local amb precisió. Finalment, després de fer una mica de [recerca][7], el flag "notsc" aplicat al grub (menu.lst) ha salvat el dia.

 [1]: https://globus.org
 [2]: https://escert.upc.edu/
 [3]: https://en.wikipedia.org/wiki/Web_crawler
 [4]: https://globus.org/
 [5]: https://www.bsc.es/
 [6]: https://en.wikipedia.org/wiki/Clock_drift
 [7]: https://linux.derkeiler.com/Mailing-Lists/Fedora/2005-02/2793.html