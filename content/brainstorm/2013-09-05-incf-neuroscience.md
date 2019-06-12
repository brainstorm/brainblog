---
author: brainstorm
categories:
- Uncategorized
date: '2013-09-04T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2013-09-04T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2013/09/05/incf-neuroscience/
tags:
- bio
- security
- software
- university
- unix
- uppnex
title: INCF and the quest for global data sharing in neuroscience
---

<img alt="" src="http://www.can-acn.org/meeting2012/images/incf-logo.png" width="277" height="155" />

<font color="red">Disclaimer: Those are my opinions only and not from my employer, etc, etc...</font>

Today it has been two months since I joined the *International Neuroinformatics Coordinating Facility*, [INCF][1] located inside the [Karolinska Institutet][2] campus in Stockholm. Coincidentally I happened to land the job in the mist of a Neuroinformatics conference:

[<img class="aligncenter" alt="INCF Neuroinformatics congress 2013" src="http://neuroinformatics2013.org/INCF_NI2013.png" width="400" height="86" />][3]

Before that, I spent almost [3 years in another field][4] of (data) science, genomics. I think I'm extremely lucky to be involved on those two different cutting-edge computational life sciences disciplines, so rich and diverse at the science level and yet pretty similar in infrastructure needs: [more storage][5], more processing and **more standards needed**.

Also today I got to answer a series of seemingly routine questions prior to attending a workshop ([EUDAT][6]). While I was writing, I realized that I was drawing a nice portrait of today's data science wins and struggles, be it genomics, neuroscience or other data-hungry sciences I might encounter during my career.

Suddenly I couldn't resist to share my braindumpings, I hope you enjoy the read <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":)" class="wp-smiley" /> 

<!--more-->

### Who are your major scientific user communities and how they use the technology with concrete applications?

According to the official INCF's website "about":

> INCF develops and maintains databases and ***computational infrastructure*** for neuroscientists. Software tools and ***standards***  
> for the international neuroinformatics ***community*** are being developed through the INCF Programs (...)

So INCF's purpose is to be the [*glia*][7] between several neuroscience research groups around the world. Or as Nature put it recently about the upcoming [human brain project][8]:

<blockquote class="twitter-tweet" width="550" lang="en">
  <p>
    Neuroscience thinks big (and collaboratively) | Nature Neuroscience reviews | <a href="http://t.co/td1iiaKpl8">http://t.co/td1iiaKpl8</a>
  </p>
  
  <p>
    &mdash; INCF (@INCForg) <a href="https://twitter.com/INCForg/statuses/374453140906467328">September 2, 2013</a>
  </p>
</blockquote>



[Dataspace][9], [<acronym title="Digital Atlasing Infrastructure">DAI</acronym><acronym></acronym>][10] and [NeuroLex][11] are outcomes of the [INCF][12] initiatives, supporting the community over the years [since 2005][13].

<center>
  <img class="aligncenter size-medium wp-image-771" alt="INCF mug" src="http://blogs.nopcode.org/brainstorm/wp-content/uploads/2013/09/DSC_6316-230x300.jpg" width="230" height="300" />
</center>

Other interesting community projects that assist scientists in running and keeping track of experiments in the neuroinformatics community are: [Sumatra][14], [BrainImagingPipelines][15] and a [W3C-PROV][16] framework for [data provenance][17], make sure you open browser tabs on those links, they are definitely worth checking!

While those tools are pretty much domain-specific, they bear some resemblance with counterparts from genomics pipelines, for instance.

### What do you think are the most important interoperability aspects of workflows in scientific data infrastructures?

In today's HPC academic facilities there are no standards on "computation units". Most of the software is installed and [maintained manually][18] in non-portable ways. Without **canonical** software installation procedures the **moving computation where the data is** ideal situation will be increasingly difficult to attain.

It's striking to see those [common architectural needs][19] between research fields and the **lack of funding incentives** to ensure [standard packaging][20] of software and coherent data schemes.

A notable example of a data standard in genomics, as put by [Francesco][21], the [SAM/BAM file format][22]. It is barely 15 pages long, straight to the point and **enough to attract people** into using it as **de-facto standard for sequence alignment**. Even if it varies slightly between versions and has custom tracks, it seems it is here to stay.

Similarly, the OpenFMRI project is set to be an example worth following for the neuroscience community. With a single figure (see below), they **capture the essence of structuring brain scans** in a filesystem hierarchy:

<center>
  <a href="https://openfmri.org/content/data-organization"><img class="aligncenter" alt="OpenFMRI standard scheme" src="https://openfmri.org/system/files/dataorg_1.png" width="640" height="480" /></a>
</center>So, important interoperability aspects would be to, among others:

1.  To give incentives to portable [software packaging efforts][23].
2.  [Early data and paper publication][24], even [before official acceptance][25].

The first requires [skilled people][26] willing and able to dedicate efforts to pave the way towards **solid and repeatable software installations**.

An example of realizing in time how important is to package software comes from the computer security world, with efforts like [BackTrack][27] and [Kali][28] Linux. Both are essentially the same [pentest][29] linux distribution, except that as a result of a [change in policy][30] in regards to software maintenance, Kali can now be ported to a [flurry of different hardware][31], which wasn't so straightforward with Backtrack.

The second requires a paradigm shift when it comes to **data sharing and publication in life sciences**:

<blockquote class="twitter-tweet" width="550" lang="en">
  <p>
    <a href="https://twitter.com/ctitusbrown">@ctitusbrown</a> <a href="https://twitter.com/braincode">@braincode</a> I see, so it's my field that's lagging. I noticed there now being a Genomics section under quant biol, so Im hopeful
  </p>
  
  <p>
    &mdash; Daniel Klevebring (@dklevebring) <a href="https://twitter.com/dklevebring/statuses/368380087441170432">August 16, 2013</a>
  </p>
</blockquote>



### Where do you see major barriers with respect to workflow support near the data storage?

More efforts into creating an interoperable infrastructure via software and data encapsulation.

Willingness to take some of the academic computation efforts towards **HPC research computing** as opposed to throw more manual maintenance work at it. The final aim is to run experiments in different HPC facilities without changing default configurations significantly.

More specifically, dedicate some research efforts on deploying and testing solutions like [docker and openstack][32] as opposed to stick with silo HPC installations.

### What are the most urgent needs of the scientific communities with respect to data processing?

To put it simply, prioritize standardization and incentivize incremental science as opposed to "novel" approaches that do not contribute to the field significantly (wheel reinvention).

![][33]

Jokes aside, [this happens all the time][34] in genomics data formats with major overlap in features.

Let's hope neuroscience learns from it gets a better grip on community consensus over the years.

 [1]: http://www.incf.org/
 [2]: http://www.ki.se
 [3]: http://neuroinformatics2013.org/
 [4]: http://www.scilifelab.se
 [5]: http://www.biostars.org/p/80575/
 [6]: http://www.eudat.eu/
 [7]: http://en.wikipedia.org/wiki/Neuroglia
 [8]: http://www.humanbrainproject.eu/
 [9]: http://www.incf.org/resolveuid/854cfc1a-ed0c-4ac3-9409-673a3895d156
 [10]: http://atlasing.incf.org/wiki/Main_Page
 [11]: http://neurolex.org/wiki/Main_Page
 [12]: http://www.incf.org/resources/incf-products-and-services
 [13]: http://en.wikipedia.org/wiki/International_Neuroinformatics_Coordinating_Facility
 [14]: http://neuralensemble.org/sumatra/
 [15]: https://github.com/INCF/BrainImagingPipelines
 [16]: http://www.w3.org/TR/prov-overview/
 [17]: ttps://github.com/INCF/ProvenanceLibrary
 [18]: http://blogs.nopcode.org/brainstorm/2011/11/23/module-system-bad-and-ugly/
 [19]: http://ivory.idyll.org/blog/software-architecture-for-heterogeneous-data-integration.html
 [20]: http://ivory.idyll.org/blog/research-software-reuse.html
 [21]: https://twitter.com/vezzi84
 [22]: http://samtools.sourceforge.net/SAMv1.pdf
 [23]: https://wiki.debian.org/DebianMed/Meeting/Aberdeen2014
 [24]: http://simplystatistics.org/2012/08/17/interview-with-c-titus-brown-computational-biologist/
 [25]: http://ivory.idyll.org/blog/grants-posted.html
 [26]: http://www.timeshighereducation.co.uk/news/save-your-work-give-software-engineers-a-career-track/2006431.article
 [27]: http://www.backtrack-linux.org/
 [28]: http://www.kali.org/
 [29]: http://en.wikipedia.org/wiki/Penetration_test
 [30]: http://www.kali.org/news/kali-linux-whats-new/
 [31]: http://docs.kali.org/category/development
 [32]: https://review.openstack.org/#/c/32960/
 [33]: http://imgs.xkcd.com/comics/standards.png
 [34]: http://genome.ucsc.edu/FAQ/FAQformat.html