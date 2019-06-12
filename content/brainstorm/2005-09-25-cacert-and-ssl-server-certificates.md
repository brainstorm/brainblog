---
author: brainstorm
date: '2005-09-24T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-09-24T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/09/25/cacert-and-ssl-server-certificates/
tags:
- inet
- security
- unix
title: CAcert and SSL Server certificates
---

I've recently changed my ssl certificates to fix some problems regarding subdomain handling. As firefox and other browsers complained:

> You have attempted to establish a connection with "blogs.nopcode.org". However the security certificate presented belongs to "nopcode.org" (...) 

My SSL certificates are not self-signed, I use a <acronym title="Certificate Authority">CA</acronym> known as [CAcert][1] instead. This is one of their a logos:

<img src='http://blogs.nopcode.org/brainstorm/wp-content/images/cacert3.png' alt='cacert logo' align='middle' />

I've joined that CA six months ago and I'm really happy with it. I've learnt lots of issues about SSL, certificate generation, CA's, security devices, trust policies, server configurations, code signing, etc...

If you want to know more about certificates, check cacert's [wiki][2], there's a lot of interesting stuff there, including how to join CAcert's <acronym title="Web of Trust">WoT</acronym> <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_wink.gif" alt=";-)" class="wp-smiley" />  
<!--more-->

Next I'm gonna explain how easy it was to generate a certificate for my main domain and subdomains: I've simply used a helper perl [script][3] to generate a [<acronym title="Certificate Signing Request">CSR</acronym>][4] and a private key. Then, I've sent that CSR to CAcert, and they sent back to me my server certificate. Then, with a certificate signed by CAcert, and my private key file, I'm now able to secure my services (imap, smtp, www and jabber).

The script execution went like this:

<pre># ./subjectAltname.pl
Generate SSL Cert stuff for SAPI
FQDN/Keyname for Cert (ie www.example.com)              :nopcode.org
Alt Names (ie www1.example.com or &lt;return> for none)                    :blogs.nopcode.org
Alt Names (ie www1.example.com or &lt;/return>&lt;return> for none)                    :www.nopcode.org
Alt Names (ie www1.example.com or &lt;/return>&lt;return> for none)                    :ftp.nopcode.org
(... more sub-domains ...)
Alt Names (ie www1.example.com or &lt;/return>&lt;return> for none)                    :voip.nopcode.org
Alt Names (ie www1.example.com or &lt;/return>&lt;return> for none)                    :
Host short name (ie imap big_srv etc)                           :nopcode

Attempting openssl...
Generating a 2048 bit RSA private key
..+++
...................+++
writing new private key to '/somewhere/privatekey.pem'
-----

writing csr to /somewhere/csr.pem...

Take the contents of /somewhere/csr.pem and go submit them to receive an SSL ID.  When you receive your public 
key back, you 'should' name it something like 'something.pem'.
&lt;/return></pre>

If you just want to issue a certificate for one single <acronym title="Top Level Domain">TLD</acronym> (instead of using subjectAltName SSL attribute for subdomains), you only have to write:

<pre># openssl req -nodes -new -keyout private.key -out server.csr
</pre>

**EDIT**: There is a **strong** [reason][5] against using self-signed certificates.

 [1]: http://cacert.org/
 [2]: http://wiki.cacert.org/
 [3]: http://wiki.cacert.org/wiki/VhostTaskForce#head-15f2cf5e27a280c7c16e4d82910a16871a4fb345
 [4]: http://wiki.cacert.org/wiki/CSR
 [5]: http://www.informationweek.com/story/showArticle.jhtml?articleID=171200010&cid=RSSfeed_All