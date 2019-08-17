---
author: brainstorm
categories:
- Uncategorized
date: '2011-03-17T13:00:00.000000+00:00'
dsq_thread_id:
- 2874889725
excerpt: null
layout: post
modified: '2011-03-17T13:00:00.000000+00:00'
openid_comments:
- a:1:{i:0;s:4:"8644";}
permalink: https://blogs.nopcode.org/brainstorm/2011/03/18/hadoop-and-friends-on-academic-clusters/
tags:
- admin
- KTH
- software
- sysadmin
- university
- unix
- virtualization
title: Hadoop and friends on academic clusters
---

[<img src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/03/hadoop.png" alt="" title="hadoop" width="200" height="47" class="alignnone size-full wp-image-426" />][1]

I've just [pushed][2] a set of trivial [modules system][3] scripts that will hopefully ease your deployment of [Cloudera Distribution for Hadoop][4] [3 Beta 4][5] on your university cluster... partly, at least. This sad "partly" made me think about the current state of things on IT and <acronym title="High Performance Computing">HPC</acronym>.

Over time I've learnt that there are several unexpected issues when deploying hadoop on custom clusters that you don't own. Those are mainly related to software management policies, [non-root access (being auto-deployment unfriendly)][6], quotas and queueing or "batch" systems.

Ignoring most of these "fixable" issues, it becomes apparent that the most juicy problem for a sysadmin trying to get the most of [hadoop-related][7] tools is the batch system. Be it [SGE][8], [SLURM][9] or other non-[DRMAA][10] compliant [exotic batch system][11] implementations, you'll have to deal with annoying integration quirks at some point, granted.

Making it all work can be [challenging][12] to say the least... but the question is: **does it have to be that hard ?**

<!--more-->

Almost **four** years ago I first approached those problems by building a modest 13-node development [rocks cluster][13] out of recycled machines. Despite its low profile, I could **deploy hadoop** rpm's and **easily scale** by adding more nodes, **quickly reinstalling** the whole cluster with any new software changes when needed.

<center>
  </p> <table>
    <tr>
      <td>
        <a href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/03/cluster.jpg"><img src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/03/cluster-150x150.jpg" alt="13-node poor mans cluster, front view" title="13-node poor mans cluster, front view" width="150" height="150" class="alignleft size-thumbnail wp-image-412" /></a>
      </td>
      
      <td>
        <a href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/03/cluster_rear.jpg"><img src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/03/cluster_rear-150x150.jpg" alt="13-node poor mans cluster, rear messy cables" title="13-node poor mans cluster, rear messy cables" width="150" height="150" class="alignright size-thumbnail wp-image-413" /></a>
      </td>
    </tr>
  </table>
  
  <p>
    </center>
  </p>
  
  <p>
    But still, building your own cluster doesn't feel quite "easy" or cost-effective anyway, right ? If you are willing to pay a few dollar cents, you can get yourself started on your own with <a href="https://aws.amazon.com/elasticmapreduce/">amazon</a>, easy but slow when uploading big datasets, assuming you are legally allowed to do so for <strong>real datasets</strong>. With some more patience and for free you can even use <a href="https://open.eucalyptus.com/CommunityCloud">Eucalyptus Community Cloud</a>.
  </p>
  
  <p>
    Sane critical thinking and skepticism is always the best approach, but this is not about <a href="https://www.forbes.com/2010/07/15/virtualization-automation-resources-technology-cloud-computing.html">cloud hype</a> IT fears anymore, it is about <strong>pragmatism</strong> and getting things done.
  </p>
  
  <p>
    Full root access, reasonable quotas, <strong>automatic deployment</strong>, plain <a href="https://wiki.centos.org/HowTos/SetupRpmBuildEnvironment">community</a> <a href="https://wiki.ubuntu.com/Packaging">oriented</a> <strong>package-based</strong> software management as opposed to $PATH and $LD_LIBRARY_PATH hacks, allows for proper <a href="https://www.stanford.edu/~pgbovine/cde.html">reproducible</a> and <a href="http://cloudbiolinux.com">shareable research</a>.
  </p>
  
  <p>
    Can't wait to see big university private clouds catching up with their commercial counterparts. I just hope it does not take too long to have such convenient tools on user hands :)
  </p>

 [1]: https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/03/hadoop.png
 [2]: https://github.com/brainstorm/pdc/tree/master/hadoop/noarch
 [3]: https://modules.sourceforge.net/
 [4]: https://www.cloudera.com/hadoop/
 [5]: https://archive.cloudera.com/cdh/3/
 [6]: https://issues.apache.org/jira/browse/WHIRR-158
 [7]: https://hadoop.apache.org/
 [8]: https://en.wikipedia.org/wiki/Oracle_Grid_Engine
 [9]: https://computing.llnl.gov/linux/slurm/
 [10]: https://drmaa.org/
 [11]: https://www.pdc.kth.se/resources/software/job-scheduler/easy-main/easy
 [12]: https://blogs.sun.com/ravee/entry/creating_hadoop_pe_under_sge
 [13]: https://www.rocksclusters.org/wordpress/