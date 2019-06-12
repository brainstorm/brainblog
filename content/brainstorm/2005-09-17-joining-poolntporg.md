---
author: brainstorm
date: '2005-09-16T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2005-09-16T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2005/09/17/joining-poolntporg/
tags:
- inet
title: Joining pool.ntp.org
---

As [pool.ntp.org][1] website states:

> The pool.ntp.org project is a big virtual cluster of timeservers striving to provide reliable easy to use NTP service for millions of clients without putting a strain on the big popular timeservers 

I'm using this service for ages, and one day I though that it was fair to contribute a **really** minimal amount of bandwidth to help improving the overall reliability of pool.ntp.org... well at least pool.europe.ntp.org :) 

I've measured the instant and average bitrate in KB with [mesure][2], these are the results:

<pre># time ./mesure -i eth0 -K -p "udp and port 123"
49

real    0m59.546s
# time ./mesure -i eth0 -K -a -p "udp and port 123"
0

real    4m52.314s
</pre>

<!--more-->

  
As you see, it's just ~1KB/s of "lost" bandwidth... as I said, minimal. Of course I've to do further measurements using MRTG for instance, to see how it behaves during days and months, but I'm confident that I'm not going to see a sudden raise on bandwidth usage.

The joining process is completely painless, just an email @ (with confirmation mail) and password have to be supplied, and you've got to change pool.ntp.org server and choose 3-5 static stratum 2 hosts from a [list][3], this policy assures an acceptable quality of service.

In exchange for the bandwidth, they provide a couple of interesting [stats][4] too evaluate my ntp server quality... I hope that I've convinced you to join, see you there ! ;)

 [1]: https://www.pool.ntp.org/
 [2]: ftp://ftp.nopcode.org/prj/mesure/mesure-0.5.tar.gz
 [3]: https://ntp.isc.org/bin/view/Servers/StratumTwoTimeServers
 [4]: https://www.pool.ntp.org/scores?ip=62.57.0.38