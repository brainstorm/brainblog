---
author: brainstorm
date: '2005-10-10T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-10-10T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/10/11/no-soc-un-spammer/
tags:
- inet
title: "No s\xF3c un spammer !"
---

Tal com [comentava][1] fa un mes, estic en el procés de de-llistarme de les principals llistes d'spam. Doncs bé, primer vaig demanar per segon cop a auna que em canviessin el registre PTR ja que el primer cop, només van fer el canvi en un dels seus servidors... abans del canvi definitiu els servidors responien així:

<pre>$ host 62.57.0.38 mayor.auna.net
38.0.57.62.in-addr.arpa domain name pointer 38.red-62-57-0.user.auna.net.

$ host 62.57.0.38 ramblas.auna.net
38.0.57.62.in-addr.arpa domain name pointer nopcode.org.
</pre>

Ara ja està corregit, és a dir, els dos resolen a nopcode.org. Després vaig omplir el form de de-listing de [SORBS-DHUL][2] explicant la situació:

> I've recently switched to static IP address within my ISP's IP Space, I pay 1€/month for my IP to be static. I've followed all the requisites you demand to de-list my server, for instance:
> 
> $ host 62.57.0.38  
> 38.0.57.62.in-addr.arpa domain name pointer nopcode.org.
> 
> As you see, my PTR register points back to my domain (I've also requested that change in my ISP). Before, it was pointing to a generic dynamic pool. I've also corrected TTL times as you demand, please, if it's something left to fix, notify me.
> 
> Thank you. 

<!--more-->

I en dos dies em van de-llistar :) :<table style="color:black" width=100% border=0> 

<td width="10%">
  Netblock:
</td>

<td width="20%">
  Record Created:
</td>

<td width="20%">
  Record Updated:
</td>

<td width="20%">
  Additional Information:
</td>

<td style="background-color:#FF8000;text-align:center" colspan="2">
  <b>Listed as an exception and therefore NOT blocked.</b>
</td></table> 

S'han de seguir alguns passos (senzills) per a conseguir que t'eliminin de la llista negra, en concret, aquests són els checks que fa SORBS:

1.  MX records are looked up for the domain entered.
2.  MX records with short TTLs are discarded.
3.  A records are looked up for each MX record with acceptable TTLs (ttl>43200).
4.  A records with short TTLs are discarded.
5.  The IP addresses in each A record are resolved to PTR records.
6.  IP addresses with PTR records that have short TTLs are discarded.
7.  IP addresses with PTR records without matching A records (in previous lookups) are discarded.
Any remaining IP addresses are added to the DUHL exclusions database. </ol> 
Ah, se'm oblidava, requisits previs imprescindibles (en el cas d'auna):

1.  Demanar IP fixa. No té cap sentit demanar que t'arreglin els PTR's si la IP va variant (trucar al 900 855 555). Cobren 1€ al mes per aquest canvi.
2.  Demanar que canviin els registres PTR pq apuntin al teu domini (enviar mail a dominios.tecnico\_nospam\_At_auna.es)

Aviam si hi ha sort amb [NJABLDYNA][3] i amb [FIVETENSRC][4] :-).

**EDIT:** Doncs he tingut sort ! He estat esborrat de les dues llistes que em faltaven al cap de 2 dies d'enviar el mateix mail que vaig enviar a SORBS :)

 [1]: https://blogs.nopcode.org/brainstorm/2005/09/10/registre-ptr-dauna/
 [2]: https://www.nl.sorbs.net/
 [3]: https://dnsbl.njabl.org/dynablock.html
 [4]: https://www.five-ten-sg.com/blackhole.php