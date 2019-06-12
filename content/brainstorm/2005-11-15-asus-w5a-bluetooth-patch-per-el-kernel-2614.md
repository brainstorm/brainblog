---
author: brainstorm
date: '2005-11-14T13:00:00.000000+00:00'
dsq_thread_id:
- 2874888682
excerpt: null
layout: post
modified: '2005-11-14T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/11/15/asus-w5a-bluetooth-patch-per-el-kernel-2614/
tags:
- software
- unix
title: Asus W5A bluetooth patch per el kernel 2.6.14
---

Recentment he conseguit fer anar el suport bluetooth del meu portàtil adaptant un [patch fet per el kernel 2.6.13][1] al 2.6.14. No he hagut de canviar massa coses, però si no voleu perdre una estona arreglant els rejects del patch original, aqui teniu el [meu per el 2.6.14][2] <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_wink.gif" alt=";-)" class="wp-smiley" /> 

Despres d'aplicar-lo, compileu el kernel amb les opcions de BT activades (no cal totes, només que les fareu servir, p.ex, BT_SCO no és necessari si no teniu intenció de treballar amb audio & bluetooth).

<pre># grep BT /usr/src/linux/.config

CONFIG_BT=m
CONFIG_BT_L2CAP=m
CONFIG_BT_SCO=m
CONFIG_BT_RFCOMM=m
CONFIG_BT_RFCOMM_TTY=y
CONFIG_BT_BNEP=m
CONFIG_BT_BNEP_MC_FILTER=y
CONFIG_BT_BNEP_PROTO_FILTER=y
CONFIG_BT_HIDP=m
CONFIG_BT_HCIUSB=m
CONFIG_BT_HCIUSB_SCO=y
</pre>

<pre># cat /proc/acpi/asus/info

Asus Laptop ACPI Extras Driver 0.29
Model reference    : W5A
SFUN value         : 0x8177
DSDT length        : 32795
DSDT checksum      : 184
DSDT revision      : 1
OEM id             : 0AAAA
OEM table id       : 0AAAA000
OEM revision       : 0x0
ASL comp vendor id : INTL
ASL comp revision  : 0x2002026

# echo 1 > /proc/acpi/asus/bled
</pre>

Si tot ha anat bé l'últim echo encendrà la llum del bluetooth. Podeu fer els emerges de les tools que necessitareu i llestos <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" /> 

<pre># emerge net-wireless/bluez-utils net-wireless/bluez-hcidump
</pre>

He escrit un mail als developers, aviam si en la próxima release del kernel no ens hem de barallar amb els canvis :-/

 [1]: http://www.sk-tech.net/support/asus_w5a.html
 [2]: http://blogs.nopcode.org/brainstorm/wp-content/data/asus_acpi_w5a.patch