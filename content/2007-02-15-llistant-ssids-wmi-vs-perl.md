---
author: brainstorm
date: '2007-02-14T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2007-02-14T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/02/15/llistant-ssids-wmi-vs-perl/
tags:
- software
- unix
- wifi
title: 'Llistant SSID''s: WMI vs Perl'
---

Part de la feina que havia de fer al 3GSM consistia en escanejar les xarxes inalàmbriques que hi havia en rang (podien arribar fins les 300 sense moure's massa). De totes aquestes xarxes calia veure quines eren legítimes i quines s'havien muntat sense permís exprés de l'organització.

Com que els portàtils dels que disposàvem corrien windows, em van venir al cap unes tools que vaig descobrir a [LISA'05][1]: [Wireless Weapons of Mass Destruction for Windows][2], concretament la utilitat [ssidscan.vbs][3] que suposadament retorna un llistat d'SSID's per command line per tal de poder parsejar els resultats a posteriori... he dit suposadament, aquest és el resultat:

<center>
  <a class="imagelink" href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2007/02/ssidwin.png" title="crippled WMI ssid listing"><img id="image64" src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2007/02/ssidwin.thumbnail.png" alt="crippled WMI ssid listing" /></a>
</center>

Com veieu, per un bug documentat (i pel que sembla, [no arreglat encara][4] (!!!) no mostra correctament el nom dels SSIDs real: lletres addicionals, altres que falten, caràcters estranys en alguns casos...

<!--more-->

Vist l'èxit de la interfície [WMI][5] de windows i el fet que tinc una nokia 770 que ve amb perl de sèrie... [KISS][6]:

<pre>#!/usr/bin/perl -w

use strict;

my $iface='wlan0';
# Llistat d'ap's autoritzats en els canals permesos
my %auth_aps = (ssid1 => [1,2,3],
ssid2 => [10,13]
#...
);
my @iwlist = `iwlist $iface scan`;
my @ssids = grep(s/\s+ESSID:"(.*)"\n/$1/,@iwlist);
my @channels = grep(s/\s+Channel:(\d)\n/$1/,@iwlist);

foreach my $ssid (@ssids) {
    print "Unauthorized access point detected: $ssid\n" if not defined $auth_aps{$ssid};
}
</pre>

Pipejant l'script amb sort & uniq ja tenim lo que buscava, sense complicacions. Per una versió més elaborada, podeu preguntar a en [MiKi][7] ;)

 [1]: https://blogs.nopcode.org/brainstorm/2005/12/05/san-diego-lanada-1er-dia/
 [2]: https://neg9.org/toorcon/toorcon2004_cd/Schmoo%20Group
 [3]: https://neg9.org/toorcon/toorcon2004_cd/Schmoo%20Group/scripts/SsidScan.vbs
 [4]: https://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=291514&SiteID=1
 [5]: https://www.microsoft.com/whdc/system/pnppwr/wmi/default.mspx
 [6]: https://en.wikipedia.org/wiki/KISS_principle
 [7]: https://mikihq.com/