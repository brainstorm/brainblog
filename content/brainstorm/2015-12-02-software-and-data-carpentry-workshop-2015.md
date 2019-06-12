---
author: brainstorm
date: '2015-12-01T13:00:00.000000+00:00'
excerpt: Organizing a two day workshop on (scientific) software and data
layout: post
modified: '2015-12-01T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2015/12/02/software-and-data-carpentry-workshop-2015
tags:
- education
- code
title: Software and Data Carpentry Workshop 2015
---

## Organizing the workshop

On the 14th April 2015, three Software Carpentry Instructors from Stockholm, [Radovan Bast](http://bast.fr/), [Olav Vahtras](https://se.linkedin.com/in/olavvahtras) and myself
got an email from [Greg Wilson](http://www.third-bit.com/greg-wilson.html):


 > Subject: Any interest in putting together a workshop
 > for Stockholm this summer?


After getting the green light from [WWCRC](http://www.gla.ac.uk/researchinstitutes/cancersciences/ics/facilities/wwcrc/), my current employer, it did not took too long to include [Oxana Sachenkova](https://about.me/oxanas) to the team and start [planning the logistics](http://software-carpentry.org/workshops/operations.html), lessons, official PhD-level university credits and raise some money to support the event.

Since we recently received our [SWC training](http://teaching.software-carpentry.org/) and had some experience with a past <a href="https://github.com/pythonkurs">scientific python workshop</a> and a previous [edition of software carpentry](http://merenlin.com/2014/06/software-carpentry-scilifelab/), we decided to go for a two day workshop:

 * One day with python fundamentals [with a best practices twist (TDD)](http://blogs.nopcode.org/brainstorm/2013/03/04/automated-python-education-via-unit-testing-and-travis-ci/).
 * The second day with a more biological data analysis focus.

That is how the idea to do **Carpentry with Software and Data** came into fruition by the end of November 2015:

http://pythonkurs.github.io/2015-11-30-swc_data/

After innumerable emails, talks and commits the event was on the forge. Also the national swedish bioinformatics communities [BILS](https://www.bils.se/) and [WABI](https://www.scilifelab.se/facilities/wabi/) supported us. We would like to thank both of the organisations for their financial support.

## Day 1: Software Carpentry

For day one, Olav had some interactive python console sessions showing how basic Python data structures and control mechanisms look like.

Following up, Radovan prepared excellent [TDD lessons](https://github.com/bast/python-tdd-exercises/), inspired on three sources:

1. The already mentioned [Python Koans](https://github.com/brainstorm/python_koans).

2. Some ideas borrowed from the [BioPython's comprehensive testsuite](https://github.com/biopython/biopython/tree/master/Tests).

3. A late addition from an upcoming [SWC TDD lesson](https://bids.github.io/2016-01-14-berkeley/testing/09-tdd.html), released just a few days before our workshop.

The idea to teach [basic Git](http://swcarpentry.github.io/git-novice/), GitHub, TravisCI and [Coveralls](http://coveralls.io) in such a short time, was challenging for the instructors but had a very good general reception from the students side.

While [the infamous installation problem](http://ivory.idyll.org/blog/2014-what-version-of-python-for-science.html) is still an issue, students managed to follow through the lessons, getting the typical python installation issues, majorly solved by a proper installation of [Miniconda](http://conda.pydata.org/miniconda.html).

[The SWC installation tests](http://pythonkurs.github.io/2015-11-30-swc_data/setup/index.html), mostly distracted students since packages not being used in the workshop where flagged as uninstalled/failed (i.e: EasyMercurial). In general I perceived that **students were getting overwhelmed by too much information from SWC default guides** and stopped following up and reading the instructions early.

**We need more TL;DR's** in software and data carpentry. Perhaps starting by the [workshop template](https://github.com/swcarpentry/workshop-template).


## Day 2: BioData, Jupyter Notebooks, Pandas and Machine Learning

The morning is dedicated to brief students into the Pandas dataframe operations with Ethan's White [python-ecology dataset](http://www.datacarpentry.org/python-ecology/). Due to time constraints the merging and concatenation of dataframes is not covered but pointed out in the lesson. Now the students have enough knowledge to followup on Oxana's Gene Expression dataset:

https://plot.ly/ipython-notebooks/bioinformatics/

For which there are [exercises for those students willing to earn swedish university credits](https://github.com/pythonkurs/2015-11-30-swc_data/blob/gh-pages/bio-visualization-lab.ipynb). After some glitches with Python 2 vs Python 3 Jupyter notebooks, students get to know how to analyze data from the [FANTOM5 consortium](http://fantom.gsc.riken.jp/5/).

![oxana_ahmed]({{ site.url }}/images/2015/12/oxana_ahmed.jpg)

After getting some expression heatmaps and good insight from Oxana, [Ahmed KachKach](http://kachkach.com/), currently interning at Spotify AB machine learning division, delights the audience with a detailed analysis of a [toy dataset on breast cancer](https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+(Diagnostic)) by using an [extremely well documented introduction to machine learning notebook](https://github.com/halflings/bio-data-workshop/blob/master/notebook.ipynb).

In order to explain PCA graphically to students, Ahmed uses an excellent [web visualization to illustrate how variable decomposition/projection works in PCA](http://setosa.io/ev/).

![swc_data_students]({{ site.url }}/images/2015/12/swc_data_students.jpg)

Right after that machine learning introduction, I show how one can enact reproducible [(and interactive!) notebooks via mybinder.org](https://github.com/brainstorm/scikit-allel-tests) service by [exploring a small scikit-allel dataset](http://app.mybinder.org/2222694185/notebooks/scikit-allel-small-data.ipynb). Furthermore, more visualization techniques are shown via my current explorations of [HivePlots](http://hiveplot.net) as an alternative way of [visualizing structural genomic variations in cancer samples](https://github.com/brainstorm/cancer-hiveplots).

On top of that, I had a talk prepared about [structural variations processed with bcbio](https://bcbio-nextgen.readthedocs.org/en/latest/contents/teaching.html), but on the interest of time, I saved it for another event :)

Last but not least, Mikael Huss goes through [a fantastic notebook showing some gene expression prediction techniques](https://github.com/hussius/gene-expression-prediction/blob/master/predict_gene_expression.ipynb) and clever feature engineering from his [current efforts at WABI](https://wabi-wiki.scilifelab.se/dashboard.action).

## Thoughts and comments

Planned ahead of time, this workshop was a sustained effort to bring instructors and people
together, and I am glad it worked.

A surprising early realization of this workshop is how high demand those courses could be:
only a few minutes after announcing the event, [we got around 40 individuals interested](http://x90.es/carpentry+) and signing up. The retention changed over time
due to cancellations, but we managed to **run the workshop with 35+ participants**.

Regarding attendance, thanks to **link shorteners on our announcement emails and twitter we could track the "funnel" of students** that showed interest all the way down to those that were commited to actually show up and complete the courses.

## Actual feedback from students

**In our post-assessment polls we got an average rating of 8 over 10 on "General satisfaction with the workshop"**, here are some selected comments:

> I learned a lot of things these two days and the workshop really made me more motivated to use pandas next day in the office. :D

> Overall it was a very nicely arranged and well prepared workshop. My only suggestion would be to simplify a bit the exercises for day 2 (perhaps by introducing some intermediary steps between two problems).  Thanks for arranging such a nice workshop.

> great event, I'll recommend it further, should be on regular (annual?) basis.

And also some things to look after in the future:

> I enjoyed particularly the first day, particularly the list of challenges/exercises that looked quite overwhelming at first but turned out to be manageable. Also very much appreciated: collection of ideas and questions on etherpad, post-its to request help. It would have been even better with little stricter time management.

such as clearly stating the (minimum) requirements to attend day 2 (intermediate/advanced):

> My Python knowlegde was not high enough to follow. exercise.py was nice to learn Python, but didn't help to learn testing process (I was stuck with the exercises). Other exercise had too difficult instructions.
The python introduction was at a very basic level but the tasks were at intermediate or above level. This needs adjustment.

or again, not putting too much material in one day, no matter how exciting it sounds at first while preparing the lessons:

> The first day was great (11/10). Intro to Python was too basic for me, but I understand it was necessary for some participants of the workshop. Intro to Git, test-driven development etc. was very well performed and I learned a lot.  Second day was pretty good (7/10), but too hurried. I feel that there were too many things squeezed into the schedule. The visualization lab had a good premise, but also suffered from too little time.