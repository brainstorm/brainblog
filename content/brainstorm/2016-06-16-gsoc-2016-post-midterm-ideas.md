---
author: brainstorm
date: '2016-06-15T14:00:00.000000+00:00'
excerpt: Midway mentoring and future activities for Google Summer of Code 2016 with
  Anton Khodak
layout: post
modified: '2016-06-15T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2016/06/16/gsoc-2016-post-midterm
tags:
- code
- bioinformatics
- bcbio
title: GSoC 2016 Post-midterm ideas
---

## GSoC 2016 Post-midterm ideas

After having a successful **midterm evaluation for argparse2cwl** where [Anton Khodak][blog_anton] successfully:

 1. Surveyed the [python argument parsing landscape][python_args].
 2. Covered the [argparse API with equivalent CWL terms][argparse2cwlterms].
 3. Generated CWL files out of an example bioinformatics package, CNVkit.
 4. Wrote tests and documentation for the above points.
 5. Engaging with the community through [gitter][gitter_cwl], [mailing list][cwl_mailing_list] and [his project blog][blog_anton], while getting ideas on how to improve and conduct the project in a way that is helpful to the community.

He will be tackling the next stage of this GSoC 2016 on CWL. This is a summary of the brainstorming and ideas that **Anton is going to pick and undertake during the next 6 weeks**.

## pypi2cwl

The idea would be to have a commandline tool that:

 1. **Queries/downloads an arbitrary pypi** package specified by the user (i.e: ./pypi2cwl cnvkit).
 2. **Checks for argparse** presence in its scripts/code.
 3. Runs argparse2cwl against it and **generates the associated CWL output** files.
 4. Generates a pull request for review to add the templates to the CWL community repository

Building on top of the CNVkit experience that Anton has, this would be very valuable to **automate the wrapping process further** and potentially explore the conversion of python packages in bulk.

A desired outcome for this sub-project would be to have an output similar to that of https://pythonwheels.com, where the overall coverage and bugs can be spotted quickly.

## cwl2argparse

As any conversion tool that has a “2” in the middle, isn’t it worth to flip it over? That’s what cwl2argparse would do ;)

In a future where everyone wraps their tools with CWL (ha!), one would like to be able to:

 1. Write a cwltool definition.
 2. Run cwl2argparse against **you_newly_wrapped_tool.cwl**.
 3. **Generate argparse code** to import into your Python program.

Since we have already generated CNVkit’s CWL, what would be the expected output of, for instance cwl2argpase tools/cnvkit-batch.cwl would generate an argparse definition that could be imported in a Python program and used, i.e:

```
P_batch = AP_subparsers.add_parser('batch', help=_cmd_batch.__doc__)
P_batch.add_argument('bam_files', nargs='*',
        help="Mapped sequence reads (.bam)")
P_batch.add_argument('-y', '--male-reference', action='store_true',
        help="""Use or assume a male reference (i.e. female samples will have +1
                log-CNR of chrX; otherwise male samples would have -1 chrX).""")
P_batch.add_argument('-c', '--count-reads', action='store_true',
        help="""Get read depths by counting read midpoints within each bin.
                (An alternative algorithm).""")
P_batch.add_argument("--drop-low-coverage", action='store_true',
        help="""Drop very-low-coverage bins before segmentation to avoid
                false-positive deletions in poor-quality tumor samples.""")
(...)
```

Pretty much what [John Chilton does for Bash via cwl2script][cwl2script], Anton can try to do it for python’s argparse so that **a developer only has to update their tool definition to generate argparse code for newly introduced flags.**
Adding in new python argparsers such as [click][click], [arg[N]][sys_argv] or [optparse][optparse]

According to [Anton's first blogpost][python_args], there are a few **other argument parsers that could use some CWL automation**. The next on the list to implement, when accounting for packages that use it, would be **python’s core argument array (arg[0], arg[1], ...)**. On the other hand, **given its free-form nature, it can be challenging to assign and transform semantics from it to CWL like the others parsers**. For instance, flags, arguments and types are all well specified on click or argparse.

Or perhaps [click][click] would be more interesting to wrap given its ramping up in usage by the community? Worth exploring in any case.

## Discarded idea (for now): Fuzzing support for argparse2cwl: argparse2cwl-fuzz

Last and least, after some deliberation, **argparse2cwl-fuzz** could be a bit too **far away from this project scope and resources: writing a stub for bioinformatics fuzzing.**

As pointed out in the original proposal:

>(...) explore and optimize the parameter/flag space of i.e, bioinformatic aligners with Teaser.”

Instead of going all the way and create a benchmarking/validation suite and [file format fuzzing][fuzzing], just providing some support for argparse2cwl and generating **different commandline flag combinations**, somewhat like [biopsy][biopsy] and [Teaser][teaser_paper] do.

Could be tackled in a future GSoC project maybe?

[blog_anton]: https://anton-khodak.github.io/argparse2cwl-blog/
[python_args]: https://anton-khodak.github.io/argparse2cwl-blog/2016/05/16/argument-parsers.html
[argparse2cwlterms]: https://anton-khodak.github.io/argparse2cwl-blog/2016/05/29/week-1.html
[gitter_cwl]: https://gitter.im/common-workflow-language/common-workflow-language
[cwl_mailing_list]: https://groups.google.com/forum/?utm_medium=email&utm_source=footer#!forum/common-workflow-language
[cwl2script]: https://github.com/common-workflow-language/cwl2script
[click]: https://click.pocoo.org/5/
[sys_argv]: https://docs.python.org/3/library/sys.html?highlight=sys.arg#sys.argv
[optparse]: https://docs.python.org/3/library/optparse.html?highlight=optparse#module-optparse
[biopsy]: https://github.com/Blahah/biopsy
[teaser_paper]: https://www.ncbi.nlm.nih.gov/pubmed/26494581
[fuzzing]: [https://en.wikipedia.org/wiki/Fuzz_testing]