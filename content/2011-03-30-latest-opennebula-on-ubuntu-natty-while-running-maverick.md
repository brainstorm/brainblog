---
author: brainstorm
categories:
- Uncategorized
date: '2011-03-29T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2011-03-29T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2011/03/30/latest-opennebula-on-ubuntu-natty-while-running-maverick/
tags:
- howto
- software
title: Latest OpenNebula on Ubuntu Natty (while running Maverick)
---

So you cannot wait for a few days when the new ubuntu release comes out with updated [opennebula][1] packages, but you need them right **now**. What are the options ?

1.  As the seasoned UNIX guy you are, you go with the good old tar xvfz & compile it yourself, wasting time both by managing its intrinsic dependencies, build dependencies and whatnot: <font color="red">#FAIL</font>
2.  You get more intrepid and try to perform some black magic with apt-pinning, risking your system's integrity if you don't do it properly (thanks [@alexmuntada][2]!): <font color="red">#FAIL</font>
3.  You realize that you want manageable distribution packages as opposed to [unmantainable][3] and [hard to share hacks][4] and go for the [recommended alternative to apt-pinning][5] (using deb-src): <font color="green">#WIN</font>

Now, the <font color="green">#BIGWIN</font> would have been giving back to the community by building my own package and contributing it via [PPA][6] or whatever packaging system you use, but that good practice will come eventually :)

 [1]: https://opennebula.org/
 [2]: https://twitter.com/alexmuntada
 [3]: https://peters.gormand.com.au/Home/tools/graft/graft-html
 [4]: https://modules.sourceforge.net/
 [5]: https://help.ubuntu.com/community/PinningHowto#Recommended alternative to pinning
 [6]: https://help.launchpad.net/Packaging/PPA