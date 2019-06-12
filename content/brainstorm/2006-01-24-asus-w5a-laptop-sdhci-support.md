---
author: brainstorm
date: '2006-01-23T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889262
excerpt: null
layout: post
modified: '2006-01-23T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/01/24/asus-w5a-laptop-sdhci-support/
tags:
- hardware
- software
- unix
title: ASUS W5A laptop sdhci support
---

[<img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sd_mmc_cards.jpg' alt='sd and mmc cards' class='alignright' />][1]

Reading [<acronym title="Kernel Traffic">KT</acronym>][2] mailing list I've found an interesting project called [<acronym title="Secure Digital Host Controller Interface">sdhci</acronym>][3] that fills a well-known gap on linux driver support: read/write capabilities for memory card readers as those found integrated on laptop computers like mine:

<pre>#lspci
(...)
01:03.2 Class 0805: Ricoh Co Ltd R5C822 SD/SDIO/MMC/MS/MSPro Host Adapter (rev 17)
</pre>

The code is not yet upstream, but you can easily patch your kernel getting latest [patches][4] from the mailing list. Once compiled as usual (make modules && make modules_install) you only have to load the following modules, mount the device and you're done:

<pre># modprobe mmc_block sdhci
# mount /dev/mmcblk0p1 /mnt/cards
</pre>

I've been able to read a 16MB SD card and a 32MB MMC card without problems whatsoever. Please, report bugs if you find any, the sdhci-dev team is currently dealing with driver timing problems and they need feedback !

 [1]: http://blogs.nopcode.org/brainstorm/wp-content/images/sd_mmc_cards.jpg
 [2]: http://www.kerneltraffic.org/kernel-traffic/kt20051127_335.html#12
 [3]: http://mmc.drzeus.cx/wiki/Linux/Drivers/sdhci
 [4]: http://list.drzeus.cx/pipermail/sdhci-devel/2006-January/000301.html