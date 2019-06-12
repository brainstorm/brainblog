---
author: brainstorm
categories:
- Uncategorized
date: '2013-03-03T13:00:00.000000+00:00'
dsq_thread_id:
- 2874890237
excerpt: null
layout: post
modified: '2013-03-03T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2013/03/04/automated-python-education-via-unit-testing-and-travis-ci/
tags:
- KTH
- university
- unix
- uppnex
title: Automated Python education via unit testing and Travis-CI
---

[<img src="https://secure.gravatar.com/avatar/fa4bdc52c6751783a1bf9f7c084acea3?s=420&d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-org-420.png" width="420" height="420" class />][1]

Sometimes **education can be a daunting process**. It is quite obvious from the student side, we all have gone through exercises, corrections, learning what we did wrong on some of them, fixing and learning from those errors, rinse and repeat. That's how it generally works.

On the teacher's side, correcting assignments is easy and unbiased unless the number of students is considerably large. At  
one of the sessions of our now official <a href="http://www.kth.se/" title="Kungliga Tekniska Högskolan" target="_blank">KTH</a> course <a href="http://www.kth.se/student/kurser/kurs/DD3436?l=en" title="Scientific Programming in Python for Computational Biology" target="_blank">"DD3436 Scientific Programming in Python for Computational Biology"</a> I was given the task to hold a session on **software testing and continuous integration in Python**... for around 50 students.

<!--more-->

So I wanted to find a simple way to **teach Python** without going through the old beaten track of the boring fibonnacci function and endless lists of exercises. Something like little incremental goals that kept students hooked until they finished the training for the session.

Then I discovered the [python koans][2] by [Greg Malcolm][3]. By themselves, they are a very good way to learn Python basics and unit testing.

But what would happen if those pykoans respected standard UNIX [exit codes][4] and had a basic [Travis-CI integration][5]? Indeed, that brings us testing and continuous integration for free!

And then, as a teacher, what if there were easy means to [monitor][6] students progress and [correct][7] assignments via some [API's and JSON][8]?

The end result is that teaching basic Python along with **good programming practices** such as <acronym title="Test Driven Development">TDD</acronym> and <acronym title="Continuous Integration">CI</acronym> is much easier nowadays. Teachers get to help students better without **correcting tons of assignments manually** and students get [green TDD lights][9] as they go.

 [1]: https://github.com/pythonkurs
 [2]: https://github.com/brainstorm/python_koans "python koans"
 [3]: https://github.com/gregmalcolm
 [4]: https://github.com/gregmalcolm/python_koans/pull/40
 [5]: https://github.com/gregmalcolm/python_koans/pull/41
 [6]: https://github.com/brainstorm/pytravis/blob/master/eval_pykoans.py
 [7]: https://github.com/brainstorm/pytravis/blob/master/koans_completed.py
 [8]: https://api.travis-ci.org/docs/
 [9]: https://travis-ci.org/hugerth/python_koans