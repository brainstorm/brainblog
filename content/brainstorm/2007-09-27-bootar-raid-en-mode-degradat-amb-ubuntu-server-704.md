---
author: brainstorm
date: '2007-09-26T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889560
excerpt: null
layout: post
modified: '2007-09-26T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/09/27/bootar-raid-en-mode-degradat-amb-ubuntu-server-704/
tags:
- hardware
- software
- unix
title: Bootar RAID en mode degradat amb Ubuntu Server 7.04
---

Fa uns dies que havia muntat el típic setup "[fake RAID][1]+LVM" usant tres disc durs amb una combinació de RAID5 i RAID1. Fins aqui, normal. Un cop tot muntat i amb el sistema bootant (grub instal·lat al [MBR][2] de cada un dels discs) he decidit fer les proves típiques per comprovar la fiabilitat del RAID: desconnectar algun dels discs per veure si continua bootant/funcionant.

Doncs res, sorpresa: Ubuntu server es nega a bootar si falta algun dels discs O_o ! RAID no era interessant entre altres coses per temes d'**alta disponibilitat** !!?? 

El cas és que salta una consola del ramdisk inicial i no arrenca res de l'array :/ Després de passar-me una bona estona pels foros, trobar i aplicar la solució, [reporto el bug al launchpad d'Ubuntu][3].

Bé, només volia estalviar aquest trencaclosques que lliga initramfs+mdadm per al que vol que el seu RAID funcioni com hauria, *enjoy it* :)

 [1]: http://en.wikipedia.org/wiki/Fakeraid#Software_RAID
 [2]: http://en.wikipedia.org/wiki/Master_boot_record
 [3]: https://bugs.launchpad.net/bugs/108971