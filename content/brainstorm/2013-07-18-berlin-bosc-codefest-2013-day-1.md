---
author: brainstorm
categories:
- Uncategorized
date: '2013-07-17T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2013-07-17T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2013/07/18/berlin-bosc-codefest-2013-day-1/
tags:
- bio
- cloud
- software
- unix
title: Berlin BOSC Codefest 2013, day 1
---

I'm at the 4th [Bioinformatics Open Source Conference][1] in a warm and sunny Berlin.

After everyone found the venue, a preliminar brainstorming helped everyone organize the tasks across several workgroups:  
[Bioruby][2] and [Illumina Basespace][3], [Visualization][4], Cloudbiolinux/Cloudman, [Ontologies and metadata][5], Data Handling, Biopython, etc...

<!--more-->

## Our contribution

[Valentine][6], [Guillermo][7] and I sat in front of [Rory Kirchner][8] and [Brad Chapman][9] to whip up SLURM support into their ipython-cluster-helper module. That would help SciLifeLab to move from the [old bcbio-nextgen pipeline][10] to the new [ipython-backed version][11] with all the [neat parallelization tricks][12] needed to run up to [1500 Human WGS][13].

The motivation behind our specific task is to:

1.  Implement basic SLURM support by understanding the already existing classes, which already support SGE, LSF, Torque and Condor schedulers in ipython-cluster-helper.
2.  Learning from that, introduce the use of the DRMAA connector, generalizing all the specific classes for the different job schedulers.
3.  Ultimately port such generalization into ipython so that python scientific computations can be executed efficiently across different clusters around the world.

That was the idea, what really happened, as with any software jorney is that planning the trip differs somehow from actually walking it:

1.  We realized that array jobs are not supported on SLURM <2.6.x and then implemented a [workaround using srun][14].
<blockquote class="twitter-tweet" width="550" lang="en">
  <p>
    <a href="https://twitter.com/UPPMAX">@UPPMAX</a>, can you please update to <a href="https://twitter.com/search?q=%23SLURM&src=hash">#SLURM</a> >= 2.6.x so that we can run job arrays and <a href="https://twitter.com/RoryKirchner">@RoryKirchner</a>'s ipython-cluster-helper? <a href="https://twitter.com/search?q=%23kthxbye&src=hash">#kthxbye</a>
  </p>
  
  <p>
    &mdash; Roman Valls (@braincode) <a href="https://twitter.com/braincode/statuses/357504694198874112">July 17, 2013</a>
  </p>
</blockquote>



2.  We realized that Since DRMAA does not generate job templates to send via cmdline, it might be wiser to put that support directly into ipython.
3.  [Guillermo got his hands dirty with installing SLURM][15] in a couple of vagrant machines so that we don't have to wait long queues on our compute cluster.

## Other stuff happening outside our coding bubble

During the day, the original draft ideas outlined in the board changed as participants got to talk to each other. If anything, that would be the highlight and the common *modus operandi* of most hackathons I've been involved in: how self-organized groups turn vague questions such as "what are you up to?" to useful working code and collaborations.

During a very quick walk around the room, I discovered [a variant analysis pipeline][16] based on [Ruffus][17] used by the [Victorian Life Sciences Computation Initiative][18], University of Melbourne. This is meant to play well with Enis Afgan's integration, [or CloudBiolinux flavor][19], for the [australian national cloud infrastructure][20].

From [provenance standardization][21] to [workflow systems][22] and a prototype to collect runtime metrics by [Per Unneberg][23] gives a grasp on the exciting ways left to walk for genomics and computational biology.

Definitely looking forward to some more action tomorrow morning :)

 [1]: http://www.open-bio.org/wiki/BOSC_2013 "Bioinformatics Open Source Conference 2013"
 [2]: http://www.bioruby.org/
 [3]: http://github.com/joejimbo/basespace-ruby-sdk
 [4]: http://www.biodalliance.org
 [5]: https://docs.google.com/document/d/19VpzwxZdlz1K4P1q1a-WYZUtiSXwUp2nafM716dzW8I
 [6]: http://nxn.se
 [7]: http://mussolblog.wordpress.com/
 [8]: https://github.com/roryk
 [9]: https://github.com/chapmanb
 [10]: https://github.com/chapmanb/bcbb/tree/master/nextgen
 [11]: https://github.com/chapmanb/bcbio-nextgen
 [12]: http://bcbio.wordpress.com/2013/05/22/scaling-variant-detection-pipelines-for-whole-genome-sequencing-analysis/
 [13]: https://github.com/chapmanb/bcbb/blob/master/talks/scipy2013_bcbio_nextgen/scipy2013_bcbio_nextgen.pdf?raw=true
 [14]: https://github.com/vals/ipython-cluster-helper/commit/02a5d9f9695ed441bb2a25ef0a17ceae193092ad
 [15]: http://mussolblog.wordpress.com/2013/07/17/setting-up-a-testing-slurm-cluster/
 [16]: https://github.com/claresloggett/variant_calling_pipeline
 [17]: https://code.google.com/p/ruffus/
 [18]: http://www.vlsci.org.au/
 [19]: https://github.com/afgane/gvl_flavor
 [20]: https://genome.edu.au/wiki/GVL
 [21]: www.w3.org/TR/prov-overview/
 [22]: http://mobyle.pasteur.fr/workflow
 [23]: https://twitter.com/unnebe