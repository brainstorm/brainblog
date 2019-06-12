---
author: brainstorm
categories:
- Uncategorized
date: '2011-08-21T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889757
excerpt: null
layout: post
modified: '2011-08-21T14:00:00.000000+00:00'
openid_comments:
- a:3:{i:0;s:4:"8999";i:1;s:4:"9000";i:2;s:4:"9001";}
permalink: https://blogs.nopcode.org/brainstorm/2011/08/22/galaxy-on-uppmax-simplified/
tags:
- bio
- python
- software
- uppnex
title: Galaxy on UPPMAX, simplified
---

This post is intended to be shortened over time, eventually becoming an automated procedure... a wiki-post from [dahlo's magic][1] until upstream patches settle down. All commands are issued on the cluster, unless otherwise stated.

Please report any issues via comments !

1.  Firsly, follow my earlier [post][2] on how to setup your own python virtual environment on UPPMAX.
2.  Once you have a prompt similar to: *(devel) hostname ~$*, you can continue, else, jump to 1.
3.  pip install drmaa Mercurial PyYAML
4.  Add the following env variables to your .bashrc: 
    <pre>export DRMAA_LIBRARY_PATH=/bubo/sw/apps/build/slurm-drmaa/lib/libdrmaa.so
export DRMAA_PATH=$DRMAA_LIBRARY_PATH
</pre>

5.  Create a file ~/.slurm_drmaa.conf with the contents: 
    <pre>job_categories: {
      default: "-A &lt;your project_account&gt; -p devel"
}
</pre>

6.  hg clone http://bitbucket.org/brainstorm/galaxy-central
7.  Edit universe_wsgi.ini from the provided sample so that it contains: 
    <pre>admin_users = &lt;your_admin_user&gt;@example.com
enable_api = True
start_job_runners = drmaa
default_cluster_job_runner = drmaa://-A &lt;your project_account&gt; -p devel
</pre>

8.  On your local machine: *ssh -f <your_user>@<uppmax> -L 8080:localhost:8080 -N*
9.  On your local machine: Fire up your browser and connect to http://localhost:8080

As a betatester you may expect some issues when running galaxy in that way. Firstly, keep in mind that it'll not perform as fast as a [production-quality setup][3], it's just a developer instance. Furthermore the node you're in might have time limit restrictions, meaning that your instance will be killed in 30 minutes if you don't reserve a slot beforehand as [Martin][4] recommended on the section "Run galaxy on a node".

 [1]: http://mdahlo.blogspot.com/2011/06/galaxy-on-uppmax.html
 [2]: http://blogs.nopcode.org/brainstorm/2011/06/23/how-to-install-python-modules-with-virtualenv-on-uppmax/ "How to install python modules with VirtualEnv… on UPPMAX"
 [3]: http://wiki.g2.bx.psu.edu/Admin/Config/Performance/Production%20Server "Running galaxy on production"
 [4]: http://mdahlo.blogspot.com/2011/06/galaxy-on-uppmax.html "Galaxy on UPPMAX by Martin Dahlo"