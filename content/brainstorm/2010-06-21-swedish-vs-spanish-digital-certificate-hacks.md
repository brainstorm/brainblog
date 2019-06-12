---
author: brainstorm
categories:
- Uncategorized
date: '2010-06-20T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2010-06-20T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2010/06/21/swedish-vs-spanish-digital-certificate-hacks/
tags:
- admin
- security
title: Swedish vs Spanish digital certificate hacks
---

In one single day I had to deal with two nasty tricks to get the following [e-administration][1] to work properly:

1.  My swedish [e-legitimation][2] [BankID][3] software token.
2.  My spanish [digital certificate][4] renewal request.

The first one failed to authenticate (silently!) because the ([propietary][5]) software, BankID, refused to work properly on [64-bit Ubuntu][6]. Adding a [ wrapper][7] solved the issue:

`<br />
sudo apt-get install nspluginwrapper<br />
sudo nspluginwrapper -i /usr/local/lib/personal/libplugins.so<br />
`

On the other hand, the spanish counterpart, complained like this:

<pre>"Su certificado no ha permitido generar una firma válida"
</pre>

Pasting the error on google sufficed to find the [solution][8] as well.

Now I wonder how our mums can cope with these big user annoyances :-S

Hopefully not everything is a lost cause here... openness and common sense in security seem to start making their way on Spain regarding [DNIe][9]: [PKCS11 sources][10] have been recently released !.

 [1]: http://en.wikipedia.org/wiki/E-Administration
 [2]: http://www.e-legitimation.se/
 [3]: http://www.bankid.com/
 [4]: http://www.cert.fnmt.es/
 [5]: http://en.wikipedia.org/wiki/Proprietary_software
 [6]: https://bugs.launchpad.net/ubuntu-website/+bug/585940
 [7]: http://www.linuxwiki.se/index.php/BankID#64-bit
 [8]: http://www.aeat.es/wps/portal/DetalleContenido?url=Ayuda/Certificado+electr%C3%B3nico/Incidencias+m%C3%A1s+frecuentes+en+el+uso+de+certificado+electr%C3%B3nico/Al+firmar+con+el+certificado+en+Mozilla+Firefox+me+aparece+un+error%3A+%22Su+certificado+no+ha+permitido+generar+una+firma+v%C3%A1lida%22&content=aef807c322186110VgnVCM1000004ef01e0aRCRD&channel=254c2a7ebe686110VgnVCM1000004ef01e0a____&ver=L&site=56d8237c0bc1ff00VgnVCM100000d7005a80____&idioma=es_ES&menu=0&img=3
 [9]: http://www.dnielectronico.es/
 [10]: http://www.kriptopolis.org/disponibles-fuentes-pkcs11