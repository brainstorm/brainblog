---
author: brainstorm
date: '2014-11-19T13:00:00.000000+00:00'
excerpt: The entanglement of electrophysiology data formats... is there a fix for
  Babel?
layout: post
modified: '2014-11-19T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2014/11/20/neuroinformatics_without_borders
tags:
- e-phys
- neuroscience
title: Neuroinformatics Without Borders, day 1
---

## Intro

I am attending a neuro meeting at the fantastic [Janelia Farm](https://janelia.org/) facilities to see how experts in the fields of [electrophysiology](https://en.wikipedia.org/wiki/Electrophysiology) and computer science among others, decide a common format to express recordings of neuronal activity and the surrounding experimental metadata.

The mandate and outline of [NWB](https://crcns.org/NWB) has a clear mission, timeline and particular steps:

1. August 2014: Project Start.
2. Phase 1: Identify use cases and evaluation criteria.
3. Phase 2: Select/assemble most promising approaches and develop data format and test it.
4. Phase 3: Test and fine-tune it.
5. July 2015: Project ends.

This post would not have been possible without the [collaborative editing](https://etherpad.mozilla.org/Uqc4nScYLF) and [many](https://twitter.com/hashtag/nwbhack?src=hash) [tweets](https://twitter.com/hashtag/NWBHackathon?src=hash) that happened during the event.

## E-phys formats

Now, brace for impact. Here's a small list of common e-phys file formats that were created by different labs:

[KWIK](https://github.com/klusta-team/kwiklib/wiki/Kwik-format),
[NIX](https://github.com/G-Node/nix),
[MEF](https://www.ieeg.org/sites/default/files/MEF_Format.pdf),
[Svoboda lab](https://crcns.org/files/data/alm-1/Svoboda_lab_data_format_general.pdf),
[LBNL BRAIN](https://bitbucket.org/oruebel/brainformat),
[StorageBIT](https://www.lx.it.pt/~afred/papers/StorageBIT.pdf),
[ARF](https://github.com/margoliashlab/arf/blob/master/specification.org),
[NSDF](https://github.com/subhacom/nsdf),
[WFDB](https://www.physionet.org),
[epHDF](https://crcns.org/files/papers/ephdf.pdf),
[MTSF](https://www.ep.liu.se/ecp/076/050/ecp12076050.pdf),
[NeXus](https://nexusformat.org),
[NDF](https://www.frontiersin.org/10.3389/conf.fnins.2010.13.00118/event_abstract),
[Brainliner](https://www.cns.atr.jp/dni/en/brainliner/brainliner_web_qs/),
[NeuroHDF](https://neurohdf.readthedocs.org/en/latest/),
[NEO](https://neuralensemble.org/neo/),
[Neuroshare](https://neuroshare.sourceforge.net/index.shtml),
[Ovation](https://ovation.io),
[Neuralynx](https://neuralynx.com/software/NeuralynxDataFileFormats.pdf),
[EEGBase](https://eegdatabase.kiv.zcu.cz)

For a more nuanced view of some of the main data formats, please have a look at the [considerations for developing a standard for storing e-phys data in HDF5](https://cdn.f1000.com/posters/docs/256316059) and the [NWB data and file formats summary](https://crcns.org/files/data/nwb/nwb_hackathon1.pdf).

Wouldn't it be a massive win to choose a single data format and not fall in the traditional academic mantra that states: "different formats are good for different things"? Or even worse, create [yet another competing standard](https://xkcd.com/927/)?

How many of those formats are actually used in research publications? Which is the one seeing the most adoption in academic literature so far?

<blockquote class="twitter-tweet" lang="sv"><p><a href="https://twitter.com/braincode">@braincode</a> well... searching for the klustakwik clustering algorithm gives 152 hits in pubmed central: <a href="https://t.co/O3UeO01spk">https://t.co/O3UeO01spk</a></p>&mdash; Shreejoy Tripathy (@neuronJoy) <a href="https://twitter.com/neuronJoy/status/535637647927287808">21 november 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Why shouldn't we just choose the top N that share the most mindshare for the greater good (reproducibility, data sharing, interoperability)?

Is HDF5 really the best format to adopt given the [difficulties encountered](https://www.hdfgroup.org/pubs/papers/Big_HDF_FAQs.pdf) with modern parallel processing frameworks such as [Spark](https://spark.apache.org/) or the neuro-oriented [Thunder](https://github.com/freeman-lab/thunder)?

Let's see if we can find a fix for this e-phys Babel.

## The talkathon

Several labs describe and present their custom ephys formats. Most of them have a **fairly large overlap on attributes, structure and features**. With varying specifications, the labs seem to revolve around [HDF5](https://en.wikipedia.org/wiki/Hierarchical_Data_Format), a hierarchical file format that stores all the attributes of the experiments, from images to timeseries, in varying degrees of complexity.

It is interesting to see how, being an event-processing problem at its core, there are very few mentions of industry and opensource event processing frameworks:

<blockquote class="twitter-tweet" lang="sv"><p>Infrastructure for Data Streams with <a href="https://twitter.com/hashtag/kafka?src=hash">#kafka</a>: <a href="https://t.co/wAfiWAYXyJ">https://t.co/wAfiWAYXyJ</a></p>&mdash; Roman Valls (@braincode) <a href="https://twitter.com/braincode/status/532188238753308672">11 november 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

With the exception of Jeremy Freeman who demoes a very interesting combination of Spark, Thunder and [Lightning](https://github.com/mathisonian/lightning), but warns us that the community buy-in into HDF5 [complicates things](https://stackoverflow.com/questions/22125778/how-is-hdf5-different-from-a-folder-with-files):

<blockquote class="twitter-tweet" lang="sv"><p><a href="https://twitter.com/braincode">@braincode</a> <a href="https://twitter.com/neuromusic">@neuromusic</a> key quote: if you&#39;ve figured out how to do it, &quot;you should consider applying for a job at The HDF Group&quot;</p>&mdash; The Freeman Lab (@thefreemanlab) <a href="https://twitter.com/thefreemanlab/status/535562916783992832">20 november 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Software developers in the room recommend exposing a strongly typed API that deals with the raw data attributes via an **intermediate representation** instead of having to change the HDF5 container at every specification change or experimental novelties.
This idea resonates quite well with the [NEO](https://journal.frontiersin.org/Journal/10.3389/fninf.2014.00010/full) format approach. An additional problem that arises with internal representations is keeping track of provenance since encapsulation might hide processing details that might be interesting to follow an experiment step by step.

## Personal conclusions

It seems to me that e-phys recording can be approached as a large scale logging problem, therefore:

1. Using a framework that aggregates **events** at scale is crucial to guarantee a **smooth and fast data analysis experience**. That is, including slicing by data recording sessions or any other criteria that the data (neuro)scientist decides.
2. Leaving the internal (intermediate) representation of data in point 1 untouched is the most convenient approach. Specially when HDF5 does not play well with modern parallel frameworks.
3. **Exporting data from point 1 as HDF5 for sharing**, given that is the most popular container within this science niche seems reasonable (to me, at least).
4. **Writing importers/exporters (serializers)** from Thunder to HDF5 seems like an interesting Hackathon challenge. **Adopting KWIK**, already used by many, as a particular specification could be interesting w.r.t interoperability.

Comments? Suggestions?