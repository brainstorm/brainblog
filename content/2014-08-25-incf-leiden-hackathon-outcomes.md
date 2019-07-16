---
author: brainstorm
comments: true
date: '2014-08-24T14:00:00.000000+00:00'
excerpt: Organizing a hackathon for neuroinformatics.
layout: post
modified: '2014-08-24T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2014/08/27/incf-leiden-hackathon-outcomes/
tags:
- hackathon
- incf
- code
title: INCF Leiden Hackathon
---

I am at the [INCF][1] [neuroinformatics congress][2] in Leiden and I am co-organizing
an [in-congress hackathon][3]. An [Etherpad][4] is used to **announce and coordinate
tasks**.

# Participants

The following participants walked into the room during the 3 days that the room was
open, presented themselves:

	Shreejoy Tripathy, University of British Columbia
	Richard Gerkin, Arizona State University
	Zameer Razack, Bournemouth University
	Tristan Glatard, McGill University, CBRAIN
	Joris Slob, Leiden University
	Barry Wark, Physion LLC
	Stephen Larson, OpenWorm
	Jeff Muller, EPFL-Blue Brain Project
	Roman Valls Guimera, INCF

At first we started with very few participants, but there was a stream of people
over the days, coming and going. Some exchanging knowledge, others just curious
about the concept of a hackathon. Also, local researchers such as [Joris Slob][33], who
works with Ontologies and NeuroImaging were highly welcome, growing the neuroinformatics
hackathon clique is always a good idea.

A special mention goes to [Barry Wark][5], Physion CEO kindly sponsored this
hackathon and briefly introduced us to [Ovation][6], a great scientific
management system that talks with many commercial cloud backends. [Thanks Barry!][7]


# Hands on work

A hackathon basic principle is that of **learning by doing**. An early example of that
started happening during the first minutes of the event via the collaborative
integration efforts of Barry Wark and Christian from [G-node][8].
After a brief discussion, they both started coding Java bindings for [NIX][9], a HDF5-based
neuroscience exchange format. They used [SWIG][10] for those bindings so potentially, other
programming languages could talk with NIX.

This is also a very clear, mutually benefiting, **hands-on example of collaboration between
industry (Ovation) and research institutions (G-node)**.

Another interesting initiative that surfaced during the event was a possible
integration between [CBrain][11] workflow systems and [INCF NIDASH neuroinformatics data model][12]
and [reference implementation][13]. Tristan Glatard and I went through the
abundant ipython notebooks and documentation of NIDASH and after understanding
the framework, proposed a quickstart for developers, [which is coming soon][14].

The specific outcome from this would be to have an **export provenance information** fuctionality
when **publishing or sharing neuroinformatics research**.

Meanwhile, Stephen Larson from [OpenWorm][16] got [2 INCF GSoC slots on 2014 edition][17]. His particular mission
was to improve packaging for [PyOpenWorm alpha0.5 version][15] that was produced by
INCF OpenWorm GSoC student project and getting it ready for merge into master.

Stephen got to hear about the [sorry state of packaging in Python][32] but **he took advantage of the hackathon
time to fix and publish GSoC outcomes for easier public consumption**.

Those in neurophysiology will love to hear that [NeuroElectro API documentation][23] was improved by
Rick Gerkin together with Shreejoy Tripathy. It is interesting to see how much electrophysiology data
can be [extracted from literature][24], all the better if it can be queried via an API!

On my side, I simplified [nipype][18] (bummer here since it was fixed already) and [pymvpa2][19]
installation processes and revived interest in [OpenTracker][20], a bittorrent tracker
that could potentially be used as a more scalable [data sharing backend][21] for [*-stars scientific Q&A sites][22].

If bittorrent had so much success in filesharing, why should not happen the same while **sharing
scientific datasets?** Let's lower the barrier for this to happen!

# DIY hackathon

As Stephen Larsson from OpenWorm put very well anticipating the upcoming [CRCNS hackathon][29]:

* Define the time and goals of the hackathon in advance that you have in mind and write them up.
* Target your participants in advance and ask them to provide at least one-liners on who they are and what they do, and help you collaboratively make an 'ideas list'.  Good platforms for this are wikis (maybe a GitHub wiki), a Google Doc that is world editable, or as Roman used recently, Etherpad.
* Lately I've been seeing folks use real-time chat more.  Consider opening up an IRC chat room, or I've seen people liking HipChat, Slack, or Google Hangout (chat) for this
* When the time comes, lead by example during the session by being there the whole time and driving the process.  Maybe open up an on air google hangout if there is an in-person component and have people watch you hacking while interacting via chat / other collaborative media
* During, try to collect a list of things that "got done".  This will be the meat you will use to demonstrate the effectiveness of the session to others who ask you to justify its existence :)

On that line, I'm looking at you, [INCF Belgian node][31], who [will hold such an event very soon][30].

Another important hint for such events is, in my opinion, to minimize the amount and time allocated to "speakers".
**Doing** should be prioritized over delivering a 30 minutes **presentation**, effectively moving away
from traditional scientific congress formats.

# Conclusions

Hackathons (or codefests) are meant to **get things done, polished or spark new ideas** and interesting collaborations.

This all happened with a relatively small amount of participants, but outcomes usually grow (to a certain point) the more
engaged participants show up and work. See [BrainHack][25] as an excellent example of this.

INCF realized that hackathons should be **considered as dedicated events**, free from other (highly interesting)
congress distractions and will continue to support them via [Hackathon Series][28] program.

**A chained stream of science hackathons**, such as the [#mozsprint][26], celebrated a few months ago, helps in
pushing tools, refinements and integrations forward. That is, **standardized and interoperable neuroinformatics research**,
in line with INCF's core mission.

More neuro-task proposals can be found over [NeuroStars][27]. Pick your's for the next hackathon ;)

 [1]: https://incf.org
 [2]: https://neuroinformatics2014.org
 [3]: https://wiki.incf.org/mediawiki/index.php/Hackathons/Leiden-2014
 [4]: https://etherpad.mozilla.org/hackathon-leiden2014
 [5]: https://www.linkedin.com/pub/barry-wark/3/941/68b
 [6]: https://ovation.io/
 [7]: https://twitter.com/barryjwark
 [8]: https://github.com/G-Node
 [9]: https://github.com/G-Node/nix
 [10]: https://www.swig.org/
 [11]: https://journal.frontiersin.org/Journal/10.3389/fninf.2014.00054/abstract
 [12]: https://nidm.nidash.org/
 [13]: https://github.com/incf-nidash
 [14]: https://github.com/incf-nidash/nidm/issues/162
 [15]: https://github.com/openworm/PyOpenWorm/tree/alpha0.5
 [16]: https://www.openworm.org/
 [17]: https://www.incf.org/gsoc/2014/proposals/
 [18]: https://github.com/nipy/nipype/pull/898
 [19]: https://pypi.python.org/pypi/pymvpa2/2.3.1
 [20]: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=685575
 [21]: https://www.biostars.org/p/106585/
 [22]: https://github.com/ialbert/biostar-central/
 [23]: https://www.neuroelectro.org/api/docs/
 [24]: https://www.neuroelectro.org/neuron/85/
 [25]: https://brainhack.org/
 [26]: https://mozillascience.org/the-mozsprint-heard-round-the-world/
 [27]: https://neurostars.org/p/1/
 [28]: https://incf.org/activities/hackathons
 [29]: https://crcns.org/NWB
 [30]: https://www.frontiersin.org/Community/EventDetails.aspx?eid=2528
 [31]: https://www.neuroinformatics.be/
 [32]: https://python-notes.curiousefficiency.org/en/latest/pep_ideas/core_packaging_api.html
 [33]: https://www.liacs.nl/~jslob/