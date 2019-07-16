---
author: brainstorm
categories:
- Uncategorized
date: '2006-04-10T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2006-04-10T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/04/11/upgrade-a-wordpress-202/
tags:
- inet
title: Upgrade a WordPress 2.0.2
---

Wow, ha estat molt més fàcil del que em pensava usant webapp-config a gentoo, esborrant la versió antiga:

<pre>x90-nopcode vhosts # webapp-config -C -h blogs.nopcode.org/brainstorm -d wordpress wordpress 1.5.2</pre>

E instal·lat la nova versió de wordpress (prèviament havia fet un emerge wordpress amb el flag +vhosts):

<pre># webapp-config -I -h blogs.nopcode.org/brainstorm -d wordpress wordpress 2.0.2
*   Creating required directories
*   Linking in required files
*     This can take several minutes for larger apps
*   Files and directories installed
* Install completed - success</pre>

Esperem que no s'hagi trencat res (aviseu si és el cas!), tot ha anat extremadament ràpid (1 minut :_?). Després ha estat qüestió d'anar a la part administrativa de wordpress, dir "OK" en un update de la base de dades i tot llest... genial.

Ah ! No us penseu pas que això s'ha mort... tonaré ! Quan no estigui en mode [oriol][1] ;-).

 [1]: https://oriol.joor.net/blog/?item=resumint