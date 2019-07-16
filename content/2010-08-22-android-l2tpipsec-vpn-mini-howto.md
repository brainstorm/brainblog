---
author: brainstorm
categories:
- Uncategorized
date: '2010-08-21T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889687
excerpt: null
layout: post
modified: '2010-08-21T14:00:00.000000+00:00'
openid_comments:
- a:2:{i:0;s:4:"8557";i:1;s:4:"8558";}
permalink: https://blogs.nopcode.org/brainstorm/2010/08/22/android-l2tpipsec-vpn-mini-howto/
tags:
- admin
- android
- gadgets
- howto
- ipsec
- l2tp
- security
- software
- sysadmin
- unix
title: Android L2TP/IPSec VPN mini-howto
---

![][1]

I would have preferred that my Android 1.6 device supported [OpenVPN][2] out of the box. Unfortunately, this is only available for [rooted][3] devices and a bit of suffering. Instead, I went for configuring [IPsec][4] inside [L2TP][5] VPN server. All of it stuffed into an old and low-end [Soekris net4511][6] board running [Voyage Linux][7].

First, I will just redirect you to the well-documented, lengthy but primary resource:

[Using a Linux L2TP/IPsec VPN server][8]

On the client side, this post is quite complete:

[Adding VPN connections to Android 1.6 (Donut)][9]

If you're feeling impatient and brave, perhaps you'll succeed with the configuration files that follow (they worked for me)... since those are highly dependant on your network setup, YMMV, a lot.

Before jumping right into the meat and to avoid confusion, let's see what is the game all these evil daemons are going to play:

1.  A client (my android phone), connects to the server on port 4500.
2.  IPsec server (OpenSWAN) responds and asks for the PSK.
3.  If the previous "gatekeeper" is ok with you, control is handed over L2TP, the other "tunnel keeper" who will ask for another password.
4.  If L2TP is satisfied with your answer, PPPD, the ancient UNIX beast will be waken up and ask for... your user and password !
5.  Congrats ! You're survived the gates, now you're on your home network from your smartphone, ain't it cool ?

<!--more-->

# Prerequisites

I'm using [Openswan][10] for IPsec here since the debian packaging system states its preferences clearly:

<pre>freeswan - IPSEC utilities **transition** package to Openswan
openswan - IPSEC utilities for Openswan
</pre>

On the L2TP part, I went for xl2tpd but is **IMPORTANT** that you use version **1.2.7** at least. Version 1.2.0 present on Debian Lenny did not work for me !

# IPsec

Here is the configuration for "/etc/ipsec.conf", where I use a group shared <acronym title='Pre Shared Key'>PSK</acronym>:

`<br />
nat_traversal=yes<br />
nhelpers=0<br />
conn L2TP-PSK<br />
authby=secret<br />
# Do NOT enable PFS, my android 1.6 did not work with it<br />
pfs=no<br />
rekey=no<br />
keyingtries=3<br />
left=%defaultroute<br />
leftprotoport=17/%any<br />
right=%any<br />
rightprotoport=17/%any<br />
auto=add<br />
#Disable Opportunistic Encryption<br />
include /etc/ipsec.d/examples/no_oe.conf<br />
`

The secret (PSK) for the IPSec tunnel lays in the "/etc/ipsec.secrets" file, in a simple format:

`<br />
192.168.24.100 %any: PSK "your_PSK_here"<br />
10.1.20.1 %any: PSK "your_PSK_here"<br />
`

If you don't put the gateway IP's you expect to use, IPsec logs will complain this way on "/var/log/auth.log" or in "/var/log/secure":

> packet from 10.1.20.29:500: initial Main Mode message received on 10.1.20.1:500 but no connection has been authorized 

# L2TP

First, do make sure that the module "pppol2tp" is loaded and present on "/etc/modules", for it to survive reboots.

Now, the L2TP configuration (xl2tp.conf):

`<br />
[lns default]<br />
ip range = 10.1.20.31-10.1.20.50<br />
; hidden bit = no<br />
local ip = 10.1.20.30<br />
require chap = yes<br />
refuse pap = yes<br />
require authentication = yes<br />
name = yourhost<br />
ppp debug = yes<br />
pppoptfile = /etc/ppp/options<br />
length bit = yes<br />
`

Don't forget to put the L2TP secrets (/etc/xl2tpd/l2tp-secrets):

`<br />
# host (us)     username    password<br />
voyage	    anonymous      another_password<br />
`

I ran into another issue with "/var/run/xl2tpd/l2tp-control", you'll see in the logs clearly, but just make sure you "mkdir -p /var/run/xl2tpd".

I find it quite funny that we've to pass through **two** "gatekeepers" who ask for passwords: IPsec PSK and now, L2TP password. We've another old beast to get through, and it's name is: PPP !

# PPP

I just used the daemon already installed on most distributions by default, the "ppp" package.

Nothing fancy to see here, just the "chap-secrets" file:

`<br />
# Secrets for authentication using CHAP<br />
# client	server	secret			IP addresses<br />
user1		*	"yet_another_pass"		10.1.20.32<br />
user2		*	"and_another!!!"	        	10.1.20.33<br />
`

# Future improvements

First of all, PSK may be convenient for testing purposes, but I would recommed moving to <acronym title='Public Key Infrastructure'>PKI</acronym> just after verifying that this setup works as expected.

The IP addresses where set manually for each client, but I'm sure you can scan through the original document and figure out a nicer DHCP/DNS interaction with your clients.

The default configuration/routing will redirect all your requests towards the tunnel, but perhaps you're more interested on [splitting the tunnel][11].

Good luck !

 [1]: https://www.techbabu.com/wp-content/uploads/2009/10/ipsec.png
 [2]: https://openvpn.net/
 [3]: https://www.androidzoom.com/android_applications/communication/openvpn-installer_epia.html
 [4]: https://en.wikipedia.org/wiki/Ipsec
 [5]: https://en.wikipedia.org/wiki/L2tp
 [6]: https://www.soekris.com/net4511.htm
 [7]: https://linux.voyage.hk/
 [8]: https://www.jacco2.dds.nl/networking/openswan-l2tp.html
 [9]: https://blog.brightpointuk.co.uk/adding-vpn-connections-android-16-donut
 [10]: https://www.openswan.org/
 [11]: https://http://www.jacco2.dds.nl/networking/linux-l2tp.html#Split_tunnelling