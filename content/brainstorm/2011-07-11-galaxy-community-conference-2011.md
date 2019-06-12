---
author: brainstorm
categories:
- Uncategorized
date: '2011-07-10T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2011-07-10T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2011/07/11/galaxy-community-conference-2011/
tags:
- bio
- cloud
- software
- unix
- uppnex
title: Galaxy community conference 2011
---

It has been a while since the [GCC2011][1] took place in [Lunteren][2], in the Netherlands. As a result of my visit, I gained some more valuable insight about what I like to call the [metasploit][3] of [computational biology][4], if such an analogy could be made between computer security and biology.

## A few words about Galaxy

With a [15+ core team][5] and a [very active][6] contributor base, [Galaxy][7] is trying hard to provide a fix for the biomedical Babel in which life scientists work nowadays.

From its modest origin as a **single perl script**, later on morphing into a **python web framework**, Galaxy evolved rapidly. In short, Galaxy can be thought as the glue code that wraps and **uniformizes** a considerable amount of **bioinformatics programs** into a more consistent web interface.

But there's much more under the hood: cluster job management, data conversion, dataset access controls, security, web services, etc... to name a few components and features.

> ["Everything is possible in Galaxy, As long as you can run it on the command line, you can incorporate it into Galaxy."][8]  
> -- Hans-Rudolf Hotz, Friedrich Miescher Institute for Biomedical Research

But not everything shines in the galaxy since NGS tool inclusion hogged its [main site][9] at some point. This fact only proves the point that single sites like Galaxy main, handling 130.000 cluster jobs/month and 1TiB uploads per week, face sustainability issues on the big datasets era we're living in. As a result, other than imposing reasonable cluster quotas, interesting [scaling][10] [strategies][11] are being tested on real research projects. Therefore, federation and cloud computing are the [next steps][12] on this particular quest to the bio-universe.

One interesting realization on the conference is that not only labs are rolling their own Galaxy instances, there was a big sequencing industry player showing some interest on it too:

> ["Galaxy is an attractive workflow engine candidate"][13]  
> &#8211; Kirt Haden, Illumina Inc

<!--more-->

## Common concerns

There are some [FAQ][14] I've been asked by colleagues and that came up on the past conference too. Therefore, I would like to keep them here for future reference, feedback and further questions are very welcome via the comments... and their [support options][15].

### Our compute cluster is not used due to IT policy restrictions, what should I do ?

You might want to run it as a single user, as [Martin Dahlö][16] describes, setting up a SSH tunnel. Obviously, this option has many problems when it's aimed at non-developer scenarios: [it does not scale][17]. Make sure you explain the [big picture][18] and aim to reach consensus with your IT department. Again, some basic [key insights][8] and common sense might help here.

### Are software versions kept in the history when running a workflow ?

During the conference [Kanwei Li][19] came up with some patch to keep track of the software versions by appending a <version> tag on a particular tool's xml. This tag just runs a "&#8211;version" when the tool is used and appends this information in the history. You might want to ask him through [galaxy-dev][20] mailing list if you're interested in this feature.

### How's Galaxy's sample tracking moving forward ?

There are currently two big sample tracking systems present on the galaxy sphere: the one already [present on main][21] and [some nextgen patches by Brad Chapman][22]. Try them out, or better yet, join the upcoming [BOSC2011][23] [Codefest][24] and improve or merge the current systems.

### What is the API status ?

In short: [it is growing][25] as needed.  
Shorter: **@web.expose_api** decorator.  
Better explained: Read up by [slide 26][26].

### My cluster uses a custom job scheduler, will galaxy work with it ?

If your batch system supports DRMAA, there's a better chance to get it rolling. Check out the [recent progress on SLURM][16] system for instance.

### How does galaxy splitting of datasets for embarassingly parallel tools ?

There's [a ticket][27] for that.

## Next stop: [BOSC2011][23] and <a href=https://www.iscb.org/ismbeccb2011"">ISMB</a>

I have just covered a few topics shown in the conference, but you can take the time to [explore it further][1], both by videos and slides. Obviously the galaxy evolution does not end up here, there's still more to come in a few days in Vienna.

 [1]: https://wiki.g2.bx.psu.edu/GCC2011
 [2]: https://en.wikipedia.org/wiki/Lunteren
 [3]: https://www.metasploit.com/
 [4]: https://en.wikipedia.org/wiki/Computational_biology
 [5]: https://wiki.g2.bx.psu.edu/Galaxy%20Team
 [6]: https://dir.gmane.org/gmane.science.biology.galaxy.devel
 [7]: https://galaxy.psu.edu/
 [8]: https://wiki.g2.bx.psu.edu/Events/GCC2011?action=AttachFile&do=get&target=SixKeyInsights.pdf
 [9]: https://main.g2.bx.psu.edu/
 [10]: https://www.biomedcentral.com/1471-2105/11/S12/S4
 [11]: https://bitbucket.org/steder/galaxy-globus
 [12]: https://wiki.g2.bx.psu.edu/Future/Distributed%20Galaxy
 [13]: https://wiki.g2.bx.psu.edu/Events/GCC2011?action=AttachFile&do=get&target=RunningGalaxyDRMAAJobsAsDifferentUsers.pdf
 [14]: https://wiki.g2.bx.psu.edu/Learn/FAQ#Central_Galaxy_server_or_Galaxy_source_distribution
 [15]: https://wiki.g2.bx.psu.edu/Support
 [16]: https://mdahlo.blogspot.com/2011/06/galaxy-on-uppmax.html
 [17]: https://wiki.g2.bx.psu.edu/Admin/Config/Performance/Production%20Server
 [18]: https://wiki.g2.bx.psu.edu/Big%20Picture/Choices
 [19]: https://bitbucket.org/kanwei
 [20]: https://lists.bx.psu.edu/listinfo/galaxy-dev
 [21]: https://wiki.g2.bx.psu.edu/Admin/Sample%20Tracking/Demo
 [22]: https://wiki.g2.bx.psu.edu/Admin/Sample%20Tracking/Next%20Gen
 [23]: https://www.open-bio.org/wiki/BOSC_2011
 [24]: https://www.open-bio.org/wiki/Codefest_2011
 [25]: https://bitbucket.org/galaxy/galaxy-central/src/8b97f197b759/lib/galaxy/web/api/
 [26]: https://wiki.g2.bx.psu.edu/Events/GCC2011?action=AttachFile&do=get&target=GalaxyDeploymentandAPI.pdf
 [27]: https://bitbucket.org/galaxy/galaxy-central/issue/79/split-large-jobs-over-multiple-nodes-for