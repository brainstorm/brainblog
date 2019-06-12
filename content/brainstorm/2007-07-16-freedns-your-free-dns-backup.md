---
author: brainstorm
date: '2007-07-15T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889544
excerpt: null
layout: post
modified: '2007-07-15T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2007/07/16/freedns-your-free-dns-backup/
tags:
- inet
- software
- unix
title: 'FreeDNS: Your free DNS backup'
---

As I exposed on a [previous post][1], I was planning to replicate my DNS server using [FreeDNS][2] as a secondary nameserver for redundancy. My main concern was how to overcome the 20 subdomain limit advertised by FreeDNS. 

I thought about creating a new DNS view to allow FreeDNS to transfer a subset of my subdomains (the twenty most "critical" by my criteria). But apparently, there's no need to do so if you just use the DNS backup feature

Thanks FreeDNS ! :) 

<!--more-->

Quoting the instructions for this awesome **free** service:

> Secondary hosting lets you use afraid.org's secondary nameserver as a slave nameserver to your primary nameserver for your domain.
> 
> If you already have a domain's DNS hosted somewhere, and you are only looking for backup-DNS hosting, then this service is for you.
> 
> If you already have a domain in afraid.org in the &#8216;domains' area, then your domain is already using this feature.
> 
> To use this, you must enter your domain, and your primary nameserver's hostname. In order for this to work, your domain must allow AXFR transfers from ns2.afraid.org, and be delegated to ns2.afraid.org at your parent DNS servers.
> 
> Also note, you will NOT be able to edit hosts for your domain if you put it in here, this is ONLY for offsite backup-dns for your domain, and the changes you make on your primary nameserver will automatically be transferred to ns2.afraid.org.
> 
> DNS "notifies" are accepted. Make sure when you update your zone to also update your serial on your own DNS server, (if notifies are enabled on your DNS server) it will send ns2.afraid.org a notify (be sure to list it in the parent SOA records) which will cause ns2.afraid.org to immediately download your latest zone file.

 [1]: http://blogs.nopcode.org/brainstorm/2007/07/02/transferring-nopcodeorg-to-a-better-cheaper-place/
 [2]: http://freedns.afraid.org/