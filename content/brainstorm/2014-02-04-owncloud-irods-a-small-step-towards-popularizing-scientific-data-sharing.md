---
author: brainstorm
categories:
- Uncategorized
comments: true
date: '2014-02-03T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889967
excerpt: null
layout: post
modified: '2014-02-03T13:00:00.000000+00:00'
openid_comments:
- a:1:{i:0;i:12988;}
permalink: https://blogs.nopcode.org/brainstorm/2014/02/04/owncloud-irods-a-small-step-towards-popularizing-scientific-data-sharing/
sharing: true
tags:
- owncloud
- science
- uppnex
title: 'OwnCloud + iRODS: a small step towards popularizing scientific data sharing?'
---

<center>
  <a href="https://www.irods.org/"><img src="https://zeeshanali.com/wp-content/uploads/2013/11/iRODS-logo.jpg" width="300" height="120" alt="iRODS logo" class="aligncenter" /></a>
</center>

  
<center>
  +
</center>

  
<center>
  <img src="https://owncloud.org/wp-content/uploads/2012/09/owncloud-square-logo.png" width="120" height="120" alt="OwnCloud logo" class="aligncenter" />
</center>

# Context and iDROP

This is an open, on-demand blog post and drafty software specification.

When I started at <acronym title="International Neuroinformatics Coordination Facility">INCF</acronym>, among other duties I inherited the supervision of an ambitious data sharing project called [DataSpace][1]. After going through [its system architecture][2] and [python helper tools][3], **it seems that the system follows good design principles**.

Still, from the operational and usability sides, **there is still plenty of room for improvement**.

**One of the most commonly mentioned drawbacks** with the system has to do with how the data is presented to the researchers on the web. At the moment of writing this, an [iDROP][4] web interface is the canonical interface to access DataSpace for end users. The web client integrates well with the underlying iRODS infrastructure. One can perform <a href=https://www.youtube.com/watch?v=YhciVQCZuBY>common data operations plus manage its metadata attributes</a>, as any commandline user would do with the underlying [iRODS i-commands][5].

And even share (publish) a file as a (non-shortened, ugly) public link to share data with researchers:

<a href=https://ids-eu.incf.net/idrop-web/home/link?irodsURI=irods%3A%2F%2Fids-eu-1.incf.net%3A1247%2Fincf%2Fhome%2Fbraincode>

https://ids-eu.incf.net/idrop-web/home/link?irodsURI=irods%3A%2F%2Fids-eu-1.incf.net%3A1247%2Fincf%2Fhome%2Fbraincode</a>

# OwnCloud development at INCF

I also took charge of leading an **ongoing contract with [OwnCloud Inc][6]** to completion on the first [prototype][7] of an iRODS-OwnCloud integration. After some weeks of testing, debugging, [realizing about legacy PHP limitations][8] and plenty of help from [Chris Smith][9] and [a profficient OwnCloud core developer][10], a working proof of concept was put together with [PRODS][11], a PHP-based iRODS client.

Today it works on both OwnCloud community and enterprise editions.

Despite it being a proof of concept having some serious performance issues, **at least two scientific facilities have reported they are testing it already**. Even if these are good news, it **does need more work to be a robust solution**. In the next lines, I will be describing what is needed to make it so, I will try to be as specific as possible.

But first, **please have a look at the following GitHub pullrequest** for some context on the current issues that need fixing:

<https://github.com/owncloud/core/pull/6004>

<!--more-->

# OwnCloud+iRODS, what's left to do?

As seen in the previous pullrequest (please, [read it][12]), there is a significant performance penalty by operating as the OwnCloud extension does today, since given research facilities A, B and OwnCloud instance C:

1.  Host A uploads a file to C.
2.  PRODS "streams" that file, while still writing on disk anyway, to host B.
3.  On completion, host C deletes the uploaded file from disk.

A possible performance improvement would be to skip disk altogether or just keep N chunks on disk for reliability, discarding already sent ones. While we talk about chunks, **a set of tests should be written and some benchmarks run** to empirically test which block size fits best for OwnCloud transfers. It is a quite critical core setting, [so it should be tuned accordingly][13].

While fixing this issue would give us some iRODS speedup, it would not take advantage of iRODS's underlying [Reliable Blast UDP protocol][14] since **all transfers are channeled towards HTTP instead of plain UDP**.

<center>
  <a href='https://www.evl.uic.edu/cavern/RBUDP/Reliable%20Blast%20UDP.html'><img src="http://www.evl.uic.edu/cavern/RBUDP/Reliable%20Blast%20UDP_files/rbudpf2.jpg" width="475" height="280" class /></a>
</center>

An improvement that would definitely boost the performance would be to just **wrap the shell i-commands into well scaffolded PHP functions**. If i-commands binaries were installed in the system, **OwnCloud would detect it and use them** instead of a **PRODS-based "compatibility mode"**. From a user perspective, the "compat mode" should be announced as a warning in the web interface to hint the user about i-commands and how the sysadmins could install the corresponding [RPM][15] or [DEB][16] packages for them.

Since **metadata is a big upside of iRODS**, it should definitely be exposed in OwnCloud as well. A possible way to meet ends, would be to **extend an existing OwnCloud [metadata plugin][17]**.

<img src="https://upload.wikimedia.org/wikipedia/commons/3/38/Screenshot_of_browser_based_file_manager_in_ownCloud2.png" width="500" height="301" class />

Last but not least, no software engineering project seems to be **100% safe from UTF-8 issues**, and PRODS does not seem to be clear from it either:

<verbatim>  
{"app":"PHP","message":"SimpleXMLElement::_\_construct(): Entity: line 18: parser error : Input is not proper UTF-8, indicate encoding !nBytes: 0&#215;92 0&#215;73 0x2D 0&#215;61 at /var/www/owncloud/apps/files\_external/3rdparty/irodsphp/prods/src/RODSMessage.class.php#140&#8243;,"level":2,"time":"2013-12-05T14:46:25+00:00&#8243;}  
{"app":"PHP","message":"SimpleXMLElement::_\_construct(): at /var/www/owncloud/apps/files\_external/3rdparty/irodsphp/prods/src/RODSMessage.class.php#140&#8243;,"level":2,"time":"2013-12-05T14:46:25+00:00&#8243;}  
{"app":"PHP","message":"SimpleXMLElement::_\_construct(): ^ at /var/www/owncloud/apps/files\_external/3rdparty/irodsphp/prods/src/RODSMessage.class.php#140&#8243;,"level":2,"time":"2013-12-05T14:46:25+00:00&#8243;}  
(...)  
</verbatim>

So a fix for that is in order too. The ultimate goal is to have a well-rounded pull request that OwnCloud developers could merge without concerns, making those features available to serveral **scientific communities in need for user-friendly and high performance data management**.

I hope I outlined what needs to be done from a software engineering perspective. **Other issues would surely pop up** while developing and extending OwnCloud, [and those deviations should be accounted for when planning the roadmap][18].

# Funding? Developers?

OwnCloud seems to be a good candidate to engulf a **heavily used data management framework in academia, iRODS**. Recently, an OwnCloud REST API is provided and bindings for [modern Cloud storage systems][19] are being worked on. This is a good chance to bring iRODS and other scientific data management systems together with OwnCloud, but we need: **developers, developers, developers!**

In the next few days, [INCF will be applying for EUDAT funding][20]. We are looking for talented OwnCloud/PHP developers, please feel free to leave a comment below if you are interested in this initiative or reach me on twitter: [@braincode][21] or just followup on the [aforementioned pullrequest][13]!.

 [1]: https://incf.org/dataspace "DataSpace"
 [2]: https://dev.incf.org/trac/infrastructure/raw-attachment/wiki/SystemArchitecture/INCF%20Dataspace%20Architecture%20v1.2.pdf
 [3]: https://github.com/INCF/ids-tools
 [4]: https://code.renci.org/gf/project/irodsidrop/
 [5]: https://www.irods.org/index.php/icommands
 [6]: https://owncloud.com/
 [7]: https://github.com/owncloud/core/blob/master/apps/files_external/lib/irods.php
 [8]: https://bugs.php.net/bug.php?id=44522
 [9]: https://github.com/cansmith
 [10]: https://github.com/DeepDiver1975
 [11]: https://www.irods.org/prods_doc/
 [12]: https://github.com/owncloud/core/pull/6004
 [13]: https://github.com/owncloud/core/pull/6004#discussion-diff-7856938
 [14]: https://www.evl.uic.edu/cavern/RBUDP/Reliable%20Blast%20UDP.html
 [15]: https://build.opensuse.org/package/show/home:braincode/irods
 [16]: https://launchpad.net/~cansmith/+archive/irods/
 [17]: https://apps.owncloud.com/content/show.php?content=151520
 [18]: https://www.quora.com/Engineering-Management/Why-are-software-development-task-estimations-regularly-off-by-a-factor-of-2-3
 [19]: https://blog.adityapatawari.com/2014/01/using-openstack-swift-as-owncloud.html
 [20]: https://www.eudat.eu/news/eudat-calls-collaboration-projects
 [21]: https://twitter.com/braincode