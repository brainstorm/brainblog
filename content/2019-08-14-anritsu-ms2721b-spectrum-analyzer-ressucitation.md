---
title: Anritsu MS2721B spectrum analyzer
author: brainstorm
date: '2019-08-14'
slug: anritsu-ms2721b-spectrum-analyzer
categories: []
tags:
  - electronics
  - reversing
  - repair
  - python
---

# The signal path and the EEVBlog

[The Signal Path][TSP] is one of the most fascinating corners of the internet if you are curious about the complex interplay between high frequency radio engineering, electronics and hardware repair.

On two of Shariar's videos, he does a teardown and repair of an [Anritsu MS2721 spectrum analyzer][anritsu_ms2721b]. Both [A][TSP#49] and [B][TSP#137] model series are meticulously opened and carefully explained from a RF engineering perspective. Since I do not have any test equipment of that sort and I'm interested in learning more about RF, I went and bought a non-working unit on ebay.

After some time poking around with it and documenting my thinking process as described below, [I recently found an EEVBlog users post where I can hopefully cross-check notes with other folks in similar situations][eevblog_anritsu]. That's what this blogpost is about: summarising my findings so far.

Welcome to my hobby repair and reverse engineering [(for fair use)][reveng_fairuse_au] journey of a [SuperH4](https://en.wikipedia.org/wiki/SuperH#SH-4) based [spectrum analyzer](https://en.wikipedia.org/wiki/Spectrum_analyzer).

# Application running! (null)

As the users in the EEVBlog forum comment, the unit comes with no internal compact flash. Luckily there's a firmware upgrade process available through the ["Master Software Tools"][anritsu_mst]. With one of the programs from the suite, USB Loader, one can copy the files to a USB drive that will then be used to prime the internal CF card.

[The USB upgrade mechanism does not work from the instrument][no_usb_loading], so copying the file structure directly into the CF card seems to go somewhere. Unfortunately, here is a screenshot of the device as it boots right after inserting a CF card:

<img src="/brainstorm/images/2019/08/anritsu_application_null.jpg" alt="anritsu application null" width="600" height="400">

Before the optimistic "Application running!" message, loading `cst_gui.out` and `cst_base.out` fly by as the progress bar goes on and on.

That being said, there's also an "Emergency recovery" 3-key-pressing mechanism which will come in handy in the future:

<img src="/brainstorm/images/2019/08/3-key-magic.png" alt="anritsu application null" width="550" height="400">

# When hardware is not really the main issue

The nonstop progress bar symptom is fairly similar to the signal path video. On Shariar's video, SRAM modules are burning hot to the touch due to a possible internal short to ground:

{{< youtube "UPSLgY_-4HE?t=1247" >}}

In my case there were some faint water damage marks on the IC so for <$20 I decided to go ahead replaced all the ICs too, just in case. After that, it would be a piece of cake repair, right? Order chips, desolder those and substitute? Wrong!

**After hot air desoldering the SRAM chips, and replacing them with fresh ones**, everything stays the same. My intuition tells me that this is a software issue with possible main (non-static, different chips on board) RAM (system) memory corruption. **The installed memory is 64MB and this thing runs an old version of VxWorks:

{{< highlight shell >}}
VxWorks operating system version "5.5.1" , compiled: "Sep 25 2009, 08:56:03"`
{{</ highlight >}}

So before replacing those (cheap) RAM chips too:

<img src="/brainstorm/images/2019/08/anritsu_ram_solder.jpg" alt="anritsu application null" width="600" height="400">

I take some RAM data snapshots thanks to the open [WDB Agent (builtin debugger)][wdb_ram_dump] to see if and how the actual OS code is being loaded (and uncompressed) right on RAM.

# Memory dump, colorized

The Metasploit team at rapid7 poked with the WDB debugger back in 2010 and [the auxiliary script that dumps the whole memory contents comes in handy in this case][metasploit_wdb]. That way, hopefully, we don't need to poke around with the compressed `VxWorks.bin` base OS binary [as other VxWorks reversing folks have done][vxworks_ttyS0].

Pre RAM desoldering I wasn't sure if the loader was having some issues allocating memory. The colorized representation of bytes is taken with the excellent [binvis.org online tool][binvis.io].

## Pre RAM desoldering

<img src="/brainstorm/images/2019/08/anritsu-memdump.png" alt="Before replacing RAM ICs" width="600" height="400">


## Post RAM soldering

<img src="/brainstorm/images/2019/08/anritsu_post_chip_stuff.png" alt="After replacing RAM ICs" width="600" height="400">

Later on I found out a handy command to run a memtest with VxWorks:

{{< highlight shell >}}
-> DoRAMTest

Address Error (Load)
EXPEVT Register: 0x000000e0
Program Counter: 0x0c01d730
Status Register: 0x40000001
Access Address: 0x0000000d

c374c28 _vxTaskEntry +8 : _shell ()
c3f89ec _shell +ec : c3f8a60 ()
c3f8bb8 _shell +2b8: _execute ()
c3f8d54 _execute +94 : _yyparse ()
c40b78a _yyparse +5ca: c409a80 ()
c409b88 _yystart +848: _DoRAMTest ()
shell restarted.
{{< / highlight >}}

One of the reasons I was suspecting that the RAM was faulty was related to the active VxWorks process (task) table:

{{< highlight shell "hl_lines=12">}}
$ telnet 10.1.1.116
Trying 10.1.1.116...
Connected to 10.1.1.116.
Escape character is '^]'.

-> i

  NAME        ENTRY       TID    PRI   STATUS      PC       SP     ERRNO  DELAY
---------- ------------ -------- --- ---------- -------- -------- ------- -----
tExcTask   _excTask      ff7a8f8   0 PEND        c448de0  ff7a850   3006b     0
tLogTask   _logTask      ff77f58   0 PEND        c448de0  ff77eac       0     0
(null)     �ߐ          f90df90   0 SUSPEND           4  f90df68       0     0
tShell     _shell        fb87684   1 READY       c43e720  fb8732c  1c0001     0
tPcmciad   _pcmciad      ff653e0   2 PEND        c448de0  ff65348       0     0
tRlogind   _rlogind      fdd09f8   2 PEND        c373d96  fdd06b8       0     0
tWdbTask   _wdbTask      fb888b8   3 PEND        c373d96  fb886c0  3d0002     0
tUfiClnt   c081c60       fb8e574   5 PEND        c373d96  fb8e508       0     0
tKbd       c015fe0       fb7a5bc  25 PEND        c373d96  fb7a560       0     0
tSplash    _tAnimateSpl  fb2b950  30 READY       c43e14c  fb2b8e8       0     0
tNetTask   _netTask      fedf468  50 PEND        c373d96  fedf3fc       0     0
tPortmapd  _portmapd     fdd3d18  54 PEND        c373d96  fdd3b50  3d0002     0
tTelnetd   _telnetd      fdce710  55 PEND        c373d96  fdce640       0     0
tFtpdTask  c00d620       fb285d4  55 PEND        c373d96  fb284f8       0     0
tTelnetOut__telnetOutTa  f91015c  55 READY       c373baa  f90fec0       0     0
tTelnetIn_f_telnetInTas  f8f58f8  55 PEND        c373d96  f8f55ac       0     0
tDhcpcStatec3a9700       fdd6a68  56 PEND        c373d96  fdd69ec  3d0004     0
tDhcpcReadT_dhcpcRead    fdd54bc  56 PEND        c373d96  fdd52f8  3d0002     0
tUglInput  _uglInputTas  fb76cbc  60 PEND+T      c373d96  fb769b0  3d0002     3
t1         c413020       fbcbf20 100 PEND        c448de0  fbcbe48       0     0
tUsbIsp1161_usbIsp1161H  fbc1b48 100 READY       c448de0  fbc1aa0  3d0004     0
usbOhciIsr c071560       fbbf924 100 PEND        c373d96  fbbf8dc       0     0
BusM A     c42a6c0       fbbd6d0 100 READY       c43e14c  fbbd678       0     0
usbMouseLibc413020       fbbaf00 100 PEND        c448de0  fbbad28       0     0
usbMouseLibc413020       fbb67c0 100 PEND        c448de0  fbb6738       0     0
usbKeyboardc413020       fbaddcc 100 PEND        c448de0  fbadbf4       0     0
usbKeyboardc413020       fba968c 100 PEND        c448de0  fba9604       0     0
BULK_CLASS c413020       fba4f1c 100 PEND        c448de0  fba4d44       0     0
BULK_CLASS_c413020       fba07dc 100 PEND        c448de0  fba0754       0     0
tBulkClnt  c080a00       fb9c528 100 PEND        c373d96  fb9c4c0       0     0
CBI_UFI_CLAc413020       fb96f98 100 PEND        c448de0  fb96dc0       0     0
CBI_UFI_CLAc413020       fb92858 100 PEND        c448de0  fb927d0       0     0
usbAcmLib  c413020       fb837f8 100 PEND        c448de0  fb83620       0     0
usbAcmLib_Ic413020       fb7f0b8 100 PEND        c448de0  fb7f030       0     0
tBigBrother_tBigBrother  f91c884 100 SUSPEND           4  f91c81c       0     0
tCheckUsb  _tCheckUsb    f912a20 100 READY       c43e14c  f912608       0     0
tRootTask  _usrRoot      ff7fe54 103 READY       c43e14c  ff7fb90  1c0001     0
tDcacheUpd _dcacheUpd    ff4260c 250 READY       c43e14c  ff425a0       0     0
tUsbKbd    c413020       fbb250c 255 READY       c43e14c  fbb249c       0     0
value = 0 = 0x0
{{< / highlight >}}

But I digress a bit here... since the instrument seems to still work, let's not muck around with hardware and switch to software analysis instead?

# Telnet

A passwordless prompt is all it needs to get inside the instrument:

{{< highlight shell >}}
$ telnet 10.1.1.116
Trying 10.1.1.116...
Connected to 10.1.1.116.
Escape character is '^]'.

->
{{< / highlight >}}

Getting in the telnet console early enough I can see the following boot message:

{{< highlight shell >}}
(...)
got OS version V3.21
OS match failed: V2.06 != V3.21
Application running!


*** Base Platform Compiled: Sep 23 2016, 10:27:14
{{< / highlight >}}

## Dumping symbols via telnet

There is a "lookup address" VxWorks command we can leverage instead to enumerate some symbols:

{{< highlight shell >}}
-> lkAddr 0
0x0c002000 _usrEntry                 text
0x0c002040 _sysInit                  text
0x0c002064 _intPrioTable             text
0x0c00216c _intPrioTableSize         text
0x0c002180 _ataDrv                   text
0x0c002e40 _ataDevCreate             text
0x0c0030c0 _ataRawio                 text
0x0c0050e0 _testAtaRw                text
0x0c005140 _mysysInWordString        text
0x0c0051c0 _mysysInLongString        text
0x0c005240 _batteryInit              text
0x0c0052a0 _giBatteryCmd             text
{{< / highlight >}}

So performing a `lkAddr 0x0c0052a0`, for the last `_giBatteryCmd` row, gives us the next consecutive batch of symbols... writing up a dirty telnet python script to do the rest for us is fairly straightforward:

{{< gist brainstorm  0eca3489bf44c41f6168e7448b81e419 >}}

After the resulting list of address/symbol pairs `.csv` has been generated, we can have some fun with some of the so-called VxWorks tasks, interacting with the instrument:

{{< highlight shell >}}
-> _testKbd
KBD: Setting interrupt level to 14

0x23
0x2b
0x11
0x19
0x10
0x18
Row tally == 0
0x15
0x1d
0x13
0x1b
Collumn tally == 0
(...)
{{< / highlight >}}

The voltage rails seem as "broken" as in Shariar's video (via the recovery GUI he finds out):

{{< highlight shell >}}
-> sysSelfTest
SELFTEST STEP 4
Failed Temperature selftest (HOT)
SELFTEST: +5.8 voltage test failed (0 mv)
SELFTEST: +24.0 voltage test failed (0 mv)
SELFTEST: -5.8 voltage test failed (-21 mv)
SELFTEST: one or more supply voltages incorrect
Self Test returning 0x00000810
value = 2064 = 0x810
{{< / highlight >}}

**But those failed tests are most probably a software-level artifact since [the onboard Linear Technologies DC-DC converters](https://www.analog.com/media/en/technical-documentation/data-sheets/1765fd.pdf) have simply not activated those voltage rails at this stage in the boot process.** This process of enabling power rails in sequence is conveniently named [power sequencing, usually controlled by PMICs](https://en.wikipedia.org/wiki/Power_management_integrated_circuit).


# Binwalk... meh

As warned by experts in the field, binwalk and other semi-automatic tools still don't cut it  with VxWorks blobs (and probably never will) although new tools like [binaryanalysis-ng might hold some promise?][binaryanalysis-ng-vxworks]:

{{< highlight shell >}}
$ docker run -v /tmp:/samples/ -v /tmp/extracted:/samples/extracted cincan/binwalk -C /samples/extracted -e /samples/anritsu.mem

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
4698492       0x47B17C        Boot section Start 0x42424242 End 0x7E4242
7954393       0x795FD9        Boot section Start 0x31000000 End 0x10081400
7954853       0x7961A5        Boot section Start 0x45000000 End 0x10081400
7955545       0x796459        Boot section Start 0x14006300 End 0xE1008
(...)
8032708       0x7A91C4        Boot section Start 0x1400C422 End 0xE1008
8036439       0x7AA057        Boot section Start 0x752340 End 0xE100814
8071937       0x7B2B01        Boot section Start 0x42424242 End 0x3C3C
8702277       0x84C945        Boot section Start 0x-4D6AF9C0 End 0x10102400
9251448       0x8D2A78        Copyright string: "Copyright (C) 1998, Thomas G. Lane"
9291980       0x8DC8CC        YAFFS filesystem, little endian
9611985       0x92AAD1        Microsoft executable, MS-DOS
10281976      0x9CE3F8        VxWorks operating system version "5.5.1" , compiled: "Sep 25 2009, 08:56:03"
10326056      0x9D9028        Copyright string: "Copyright 1984-2002 Wind River Systems, Inc."
10339244      0x9DC3AC        Copyright string: "Copyright Wind River Systems, Inc., 1984-2003"
10410684      0x9EDABC        VxWorks WIND kernel version "2.6"
10413595      0x9EE61B        Copyright string: "Copyright  1999-2001 Wind River Systems."
10504249      0xA04839        Copyright string: "copyright_wind_river"
11511236      0xAFA5C4        VxWorks symbol table, big endian, first entry: [type: function, code address: 0x202F0C, symbol address: 0x402A40C]
60465640      0x39AA1E8       Base64 standard index table
66396536      0x3F52178       XML document, version: ""1."
66405752      0x3F54578       JPEG image data, JFIF standard 1.01
66405782      0x3F54596       TIFF image data, little-endian offset of first image directory: 8
{{< / highlight >}}


Good news is that there does not seem to be that much entropy for this in-memory dump, meaning no signs of encrypted sections for this ~10 year old discontinued instrument:


{{< highlight shell >}}
$ docker run -v /tmp:/samples/ -v /tmp/extracted:/samples/extracted cincan/binwalk -C /samples/extracted -N -E /samples/anritsu.mem

DECIMAL       HEXADECIMAL     ENTROPY
--------------------------------------------------------------------------------
0             0x0             Falling entropy edge (0.687660)
3670016       0x380000        Falling entropy edge (0.838699)
3735552       0x390000        Falling entropy edge (0.848914)
4030464       0x3D8000        Falling entropy edge (0.810111)
9437184       0x900000        Falling entropy edge (0.436021)
9535488       0x918000        Falling entropy edge (0.722818)
60129280      0x3958000       Falling entropy edge (0.823834)
{{< / highlight >}}

# Next up: Gearing up with Radare2 and SuperH4 decompiler

[Radare2][radare2] and its great community comes in very handy in this work since other free software alternatives like [GHIDRA][ghidra] lacks [Super-H4 support at the time of writing this][ghidra-superh4]. Even if it did, I would have to pre-segment the firmare sections beforehand since 66MB of memory dump might not sit well with its project loader.

On the other hand, [it just took just a few hours from the original r2-decompiler author to add r2 super-h decompiler support to the already existing radare2 SuperH support][superh_r2dec] and radare2 can [selectively analyze slices of a binary **fast**][r2-anal-default].

After this initial exploration blogpost I'll be definitely analyzing deeper on the the VxWorks software stack side with radare2 on followup posts, stay tuned!


[superh_r2dec]: https://github.com/wargio/r2dec-js/issues/178
[reveng_fairuse_au]: https://www.alrc.gov.au/publications/17-computer-programs-and-back-ups/exceptions-computer-programs
[anritsu_ms2721b]: https://www.anritsu.com/en-US/test-measurement/products/ms2721b
[anritsu_spa_ebay]: https://www.ebay.com/sch/i.html?_nkw=anritsu+ms2721b
[anritsu_mst]: https://www.anritsu.com/en-US/test-measurement/products/mst
[wdb_ram_dump]: https://blog.rapid7.com/2010/08/02/shiny-old-vxworks-vulnerabilities/
[TSP]: https://www.youtube.com/channel/UCKxRARSpahF1Mt-2vbPug-g
[TSP#49]: https://www.youtube.com/watch?v=UPSLgY_-4HE
[TSP#137]: https://www.youtube.com/watch?v=1_M-Wccn2w8
[eevblog_anritsu]: https://www.eevblog.com/forum/testgear/anritsu-ms2721b-internal-cf-card-missing/msg2584446/#msg2584446
[vxworks_ttys0]: http://www.devttys0.com/2011/07/reverse-engineering-vxworks-firmware-wrt54gv8/
[lt1765]: https://www.analog.com/media/en/technical-documentation/data-sheets/1765fd.pdf
[metasploit_wdb]: https://github.com/rapid7/metasploit-framework/blob/76954957c740525cff2db5a60bcf936b4ee06c42/lib/msf/core/exploit/wdbrpc.rb
[binvis.io]: http://binvis.io/#/view/local?curve=scan
[radare2]: https://github.com/radare/radare2
[ghidra]: https://github.com/NationalSecurityAgency/ghidra
[no_usb_loading]: https://dl.cdn-anritsu.com/en-us/test-measurement/files/Software/Drivers-Software-Downloads/CodeLoadingIssue.pdf
[binaryanalysis-ng-vxworks]: https://github.com/armijnhemel/binaryanalysis-ng/issues/31
[ghidra-superh4]: https://github.com/NationalSecurityAgency/ghidra/issues/37
[r2-anal-default]: http://radare.today/posts/analysis-by-default/
