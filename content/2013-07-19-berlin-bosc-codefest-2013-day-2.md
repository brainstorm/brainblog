---
author: brainstorm
categories:
- Uncategorized
date: '2013-07-18T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2013-07-18T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2013/07/19/berlin-bosc-codefest-2013-day-2/
tags:
- bio
- cloud
- python
- software
- uppnex
title: Berlin BOSC Codefest 2013, day 2
---

Here I am in my second day of the BOSC hackathon, polishing work from tomorrow, but also seeing new interesting projects taking off. These are my notes from the second day. See also my [notes from the first day][1].

## Pencils down for the coding

So today we try to [wrap up][2] the support for [SLURM][3] into ipython-cluster-helper. We realized that [generalizing job managers is hard][4]. Even when at the basic level they do the same, namely submit jobs and handle hardware resources, the different flavors exist for a reason.

Extra arguments or "native specifications" that do not fit in the normal job scheduler blueprint must be passed along nicely and that final mile effort takes some time to nail down.

Furthermore, a generalized DRMAA patch towards [ipython parallel][5] on upstream requires more than 2 days to whip up, so we instead move on to optimize what we have in two different fronts:

1.  Getting old SLURM versions to work with ipython-cluster-helper without job arrays in an efficient way.
2.  Automating the deployment of SLURM server(s) with a configuration management tool: [Saltstack][6]

<!--more-->

## Other projects

Per Unneberg manages to setup a proof of concept for a metrics client that reports runtime statistics and system information from different running processes to a web service. This idea stems from [bioplanet's][7] [Genome Comparison Analytics Testing][8]. In that site, several pipelines are compared from the accuracy perspective, but nothing is showed about performance, questions such as:

*   How long did it take to run such a pipeline from beggining to end?
*   Which hardware resources such as CPUs and memory where you using?
*   Which organism(s) and to which depth were you running that pipeline against?

Some interesting talk around [biolite][9], a [data provenance system][10] for bioinformatics arises as a side result of this work. In fact, the [bcbio-nextgen pipeline][11] includes preliminary support for such a system. 

Guillermo unearths a cool project which he wanted to recover for a while, that is, [pytravis][12], a python API to interact with our favourite [continuous integration][13] system at [SciLifeLab][14].

Meanwhile the guys over Cloudbiolinux come up with nice automation and deployment scripts using puppet/chef that should eventually ease the pain for those genomics centers [trying to tame their reference genomes][15].

Those are just a few of the many initiatives going on in this hackathon that is over today, tomorrow <acronym title="Bioinformatics Open Source Conference">BOSC</acronym> starts. If you want to know more, don't miss the [CodeFest 2013 official wiki][16], there's a nice wrap up of the many parallell projects there.

 [1]: https://blogs.nopcode.org/brainstorm/2013/07/18/berlin-bosc-codefest-2013-day-1/
 [2]: https://github.com/roryk/ipython-cluster-helper/pull/6
 [3]: https://computing.llnl.gov/linux/slurm/
 [4]: https://slurm.schedmd.com/rosetta.pdf
 [5]: https://github.com/ipython/ipython/tree/master/IPython/parallel
 [6]: https://saltstack.com/community.html
 [7]: https://www.google.com/url?q=http%3A%2F%2Fwww.bioplanet.com%2Fforum%2Fdiscussion%2F7916%2Fruntimeswallclock-time-alongside-the-accuracy-metrics%23Item_1&sa=D&sntz=1&usg=AFQjCNHtnDZchEAWD71tqXpZCTGNKg3OOw
 [8]: https://www.bioplanet.com/gcat
 [9]: https://www.usenix.org/conference/tapp12/biolite-lightweight-bioinformatics-framework-automated-tracking-diagnostics-and
 [10]: https://en.wikipedia.org/wiki/Provenance#Data_provenance
 [11]: https://github.com/chapmanb/bcbio-nextgen
 [12]: https://mussolblog.wordpress.com/2013/07/19/pushing-forward-pytravis-during-berlin-codefest-2013/
 [13]: https://en.wikipedia.org/wiki/Continuous_integration
 [14]: https://www.scilifelab.se
 [15]: https://github.com/chapmanb/cloudbiolinux/commit/c634f6424a90efd1236d67a98a07d9ea726fdcb5
 [16]: https://www.open-bio.org/wiki/Codefest_2013