---
author: brainstorm
categories:
- Uncategorized
date: '2011-11-22T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889436
excerpt: null
layout: post
modified: '2011-11-22T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2011/11/23/module-system-bad-and-ugly/
tags:
- software
- sysadmin
- university
- unix
- uppnex
title: 'The "module system": The good, the bad and the ugly'
---

Dealing with software [package management][1] can be a daunting task, even for experienced sysadmins. From the long forgotten [graft][2], going through the modern and insanely tweakable [portage][3] to the (allegedly) multiplatform [pkgsrc][4] or the very promising [xbps][5], several have tried to build an easy to use, community-driven, simple, with good dependency-handling, optimal, reliable, generic and portable packaging system.

In my experience on both sides of the iron, as a sysadmin and developer, **none** of them work as one would like to.

But first, let's explore what several <acronym title='High Performance Computing'>HPC</acronym> centers have adopted as a solution and why... and most importantly, how to fix it eventually.

<!--more-->

## The good

Widely used in different research facilities, the [module system][6] allows users to choose different versions of several software. The approach is simple, just type **"module load program/1.0&#8243;** and off you go.

On the sysadmin side, it's the **same old familiar spell "tar xvfz && make && make install"**, and a "vim program" to define the module script that will set PATH, LD_LIBRARY or other variables and whatnot.

Consequently, the time required to wrap a software is minimal, conferring sysadmins with speedy quick hack superpowers. After all, [velocity in research does matter][7], and **getting things done** to let research continue its way is mandatory.

Moreover, user-coded modules can be shared easily within the same cluster by simply tweaking MODULEPATH variable. What's the catch ? [Technical debt][8] and most importantly, lack of [automation][9].

## The bad

Software **packaging is a time consuming task** that shouldn't be kept inside institutional cluster firewalls, [but openly published][10]. Indeed, a single program could have been **re-packaged** a number of times on each academic cluster for each university department that has HPC resources. When new versions come up for each package the sysadmin has to take care of bumping it by creating directories and additional recipes. How does one justify this time investment ? **It just doesn't scale**. Skip to "solutions?" section for some relief.

From a technical perspective, using package systems that are not shipped with the operating system introduces an **extra layer of complexity**. More often than not, updates on the base distribution will break compiled programs that rely on old libraries. **Stacking package managers should be considered harmful**.

Ruby, python and perl have their own mature way to install packages for most UNIXes, stacking package managers by rpm-packaging python or ruby modules, has [several][11] [bad][12] consequences. Granted, there are some concerns on uniformity, updates and security, but those again can be solved by the individual package managers.

But getting back to the [module system][13], **how well does it play with cloud computing** ?

It doesn't, thankfully !

One would have to install all the modules, and re-package the software for the virtual instances. In contrast, existing package systems, be it rpm, deb, [pip][14], [gem][15] or [lein][16] solved that by themselves. On top of that, the module system will tweak crucial system variables such as $PATH or $LD\_LIBRARY\_PATH with bad side effects for [python virtual environments][17] or any other user-defined PATHs.

Lastly, from a human resources perspective, writing [modules][13] does not **add value or expertise to your IT toolbox**. On the other hand, search engines have something to say when looking up search terms such as [job experience packaging software rpm deb][18]. Actually, being involved in open source communities via packaging can give you some very valuable insights on how open source projects work.

## The ugly

With aging and relatively unmantained software, some bugs arise. Under some circumstances here's what occurs:

<pre>$ modulecmd bash purge
*** glibc detected *** modulecmd: free(): invalid next size (fast): 0x0000000001b88050 ***
======= Backtrace: =========
(...)

$ export MODULEPATH=AAAAAAAAAAAAAAAAAAAAAAAAA:AAAAAAAAAAAAAAAAAAAAA:/bin/bash && modulecmd bash purge
    *** glibc detected *** modulecmd: corrupted double-linked list: 0x00000000009c4600 ***
</pre>

I would like to light a candle for those who dare running modulecmd with [suid bit][19]. I can only think of [one sysadmin][20] that could do that while being totally self-confident.

## Solutions?

Here's some brainstorming that might help in the long run:

1.  Instead of complicating infrastructure, just state the software versions you are running in your publications. In python, **pip freeze** helps. Want an older version ? DIY.
2.  Use [CDE][21], [rbenv][22] and [virtualenv][23] before and after publishing if concerned about platform updates during your research.
3.  Use [virtual machine images][24] to reproduce experiments.
4.  If you really need the module system, at least [publish the modules somewhere][10] for people to reuse them.
5.  If you are a sysadmin, get started with [FPM][25] as a first approach with the world of package management for your distribution.
6.  Try to get those packages accepted upstream (**best!**) and/or create your own rpm/deb [repo][26].
7.  Learn [puppet][27] and/or [chef][9].

 [1]: https://ianmurdock.com/solaris/how-package-management-changed-everything/
 [2]: https://peters.gormand.com.au/Home/tools/graft/graft-html
 [3]: https://www.gentoo.org/proj/en/devrel/handbook/handbook.xml?part=2&chap=1
 [4]: https://www.netbsd.org/docs/software/packages.html
 [5]: https://code.google.com/p/xbps/
 [6]: https://modules.sf.net/
 [7]: https://blog.jcuff.net/2011/04/velocity-in-research-computing-really.html
 [8]: https://en.wikipedia.org/wiki/Technical_debt
 [9]: https://www.opscode.com/chef/
 [10]: https://github.com/scilifelab/modules.sf.net
 [11]: https://stakeventures.com/articles/2008/12/04/rubygem-is-from-mars-aptget-is-from-venus
 [12]: https://www.b-list.org/weblog/2008/dec/14/packaging/
 [13]: https://modules.sf.net
 [14]: https://pypi.python.org/pypi
 [15]: https://rubygems.org/
 [16]: https://clojars.org/
 [17]: https://blogs.nopcode.org/brainstorm/2011/06/23/how-to-install-python-modules-with-virtualenv-on-uppmax/
 [18]: https://duckduckgo.com/?q=experience+packaging+software+rpm+deb+job
 [19]: https://en.wikipedia.org/wiki/Setuid
 [20]: https://isp.surfnet.fi/aktuelltbilder/schukken.jpg
 [21]: https://stanford.edu/~pgbovine/cde.html
 [22]: https://rbenv.org/
 [23]: https://pypi.python.org/pypi/virtualenv
 [24]: https://cloudbiolinux.org/
 [25]: https://www.semicomplete.com/blog/tags/deb
 [26]: https://www.howtoforge.com/creating_a_local_yum_repository_centos
 [27]: https://puppetlabs.com/