---
author: brainstorm
date: '2005-10-01T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889479
excerpt: null
layout: post
modified: '2005-10-01T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/10/02/teclat-usb-de-silicona/
tags:
- gadgets
title: Teclat USB de silicona
---

<p>El meu pare m'ha portat un teclat curiós:</p>
<p><a href='https://blogs.nopcode.org/brainstorm/wp-content/images/teclat_silicona1.jpg'><img src='https://blogs.nopcode.org/brainstorm/wp-content/images/thumb-teclat_silicona1.jpg' alt='teclat silicona desplegat' /></a></p>
<p><a href='https://blogs.nopcode.org/brainstorm/wp-content/images/teclat_silicona2.jpg'><img src='https://blogs.nopcode.org/brainstorm/wp-content/images/thumb-teclat_silicona2.jpg' alt='teclat silicona plegat' /></a></p>
<p>Per a que funcioni amb linux, és necessari tenir suport HID al kernel (Device Drivers->USB Support->USB Human Interface Device (full HID) support & HID input layer support):</p>
<pre>
# grep HID .config
CONFIG_USB_HID=m
CONFIG_USB_HIDINPUT=y
</pre>
<p>Fa uns anys havia provat teclats d'aquest tipus, però tots tenien tecles rígides (de goma) i funcionaven amb sensors de <a href="https://www.sensorland.com/HowPage004.html">pressió</a>, en canvi aquest teclat té la mateixa estructura que un teclat de membrana, les tecles ténen recorregut.</p>
<p>El tacte és estrany però agradable. S'ha de dir que treballant-hi una estona acaba sent una mica molest (algunes tecles com el shift, no responen sempre). Així doncs, si algún dia teniu pensat comprar-ne un, tingueu present que és útil si l'useu de forma esporàdica (mai per programar) ... se m'han acudit alguns entorns on podria ser imprescindible:</p>
<ul>
<li>En una fàbrica que genera amb molta pols (fusteria, ferreteria, etc...)</li>
<li>Connectat a un PC-Jukebox d'una festa (a qui no li ha caigut un got d'algun líquid sobre el teclat?)... o tb foam-party</li>
<li>A les instruccions diu que és resistent a àcids també... espero no trobar-me mai en aquests entorns :-S</li>
<li>Si l'usuari és extremadament porc i no vol perdre temps netejant les tecles (amb aigua i sabó es renta ràpidament).</li>
</ul>
<p>S'accepten més idees ;) </p>
