---
author: brainstorm
date: '2007-07-01T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889111
excerpt: null
layout: post
modified: '2007-07-01T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/07/02/transferring-nopcodeorg-to-a-better-cheaper-place/
tags:
- inet
title: Transferring nopcode.org to a <strike>better</strike> cheaper place
---

[<img id="image90" src="http://blogs.nopcode.org/brainstorm/wp-content/uploads/2007/07/gandi.thumbnail.gif" alt="Gandi logo" align='right' />][1]  
[<img id="image89" src="http://blogs.nopcode.org/brainstorm/wp-content/uploads/2007/07/dreamhost.thumbnail.png" alt="DH logo" align='right' />][2]

I've recently received an email from my (now old) registrar asking me to renew my domain name. I decided to compare with others and I found Dreamhost. This is my first registrar to registrar domain transfer and the transition has been a bit troublesome :/ Why transfer the domain to another registrar if your current one just works ok ? Answer: More "advantages" for less price... keeping my comparison table short (there are more [goodies][3] to take into account):

*   Gandi: 14€, Dreamhost: 7€
*   Gandi: No hosting, Dreamhost: Incredible hosting possibilities
*   Gandi: No referral program, Dreamhost: $97 new clients you enroll to DH
*   Gandi: Poor and slow support, Dreamhost: Awesome and fast support

<!--more-->

**BUT** (OMG, why there's always a big "but" :_()... Dreamhost [L0][4] (domain registry only) option, **does not allow you to transfer nor customize your own subdomains nor DNS <acronym title='Resource Records'><a href="http://www.dns.net/dnsrd/rr.html">RR</a></acronym>** ! That's a huge drawback which (imho) is not well advertised on their [web][5] nor [wiki][6]. On Gandi you get full control of your domain's resource records :/

So now how do I migrate my >20 subdomains without paying extra fee for it ?: [<acronym title='Do It Yourself'>DIY</acronym>][7]. So I set up an external DNS server at home (I already had an internal one for my LAN). **BUT** (oops, yet another one)... make sure you keep it [secure][8] using views and disabling [recursion][9] for the external view among other settings **and** make sure you audit your BIND config [remotely][10].

And finally, make sure you have redundant nameservers, just in case your primary one goes down. How do I find a cheap service on that last subject ? Just [Google for it][11]. One of the best ones I've found (just comparing features for now, not tested) is [FreeDNS][12], so I'll give it a try and post something about it on a next post... because there's yet another hidden **"BUT"** that needs to be solved somehow (hint: FreeDNS's 20 subdomains limit).

 [1]: http://www.gandi.net/
 [2]: http://www.dreamhost.com/
 [3]: http://oriol.joor.net/blog/?item=m-he-comprat-un-hosting-als-eua
 [4]: http://wiki.dreamhost.com/Domain_Registration
 [5]: http://www.dreamhost.com
 [6]: http://wiki.dreamhost.com/
 [7]: http://en.wikipedia.org/wiki/DIY
 [8]: http://www.cymru.com/Documents/secure-bind-template.html
 [9]: http://www.seoconsultants.com/tools/dns/recursion/
 [10]: http://dnsstuff.com/
 [11]: http://www.google.es/search?q=Free+DNS
 [12]: http://freedns.afraid.org/