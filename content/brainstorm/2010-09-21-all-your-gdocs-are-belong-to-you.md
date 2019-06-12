---
author: brainstorm
categories:
- Uncategorized
date: '2010-09-20T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2010-09-20T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2010/09/21/all-your-gdocs-are-belong-to-you/
tags:
- admin
- inet
- software
- unix
title: All your gdocs are belong to you
---

![Google CL (command line)][1]

So you want to backup [all your bas][2][^H^H][3] google documents ? You may do it [manually][4], but isn't it more interesting to do it by code ?

[GoogleCL][5] has been around for a while, but it's becoming more stable and interesting as time goes on. Let's see how to install it first, since the documented way on INSTALL.txt is [broken][6] at the time of writing these lines.

<!--more-->

Since both pip and easy_install fail for me while installing [gdata-python-client][7], the underlying library, I just go the old-fashioned way:

`<br />
hg clone https://gdata-python-client.googlecode.com/hg/ gdata-python-client<br />
cd gdata-python-client && sudo setup.py install<br />
`

If your environment is sane, that would install your libs under /usr/local, not having to worry about munging with $PYTHONPATH.

Then, for googlecl, well, surprise: "easy_install googlecl" will do the trick (ignore INSTALL.txt this time) <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":)" class="wp-smiley" /> 

Now, issuing a simple (no wildcard "*" needed!):

<pre>$ google docs get
Please specify folder: 
Please specify title: 
Downloading GiTools to /home/romanvg/tmp/docs.txt/GiTools.ppt
Downloading (42) CasosUs to /home/romanvg/tmp/docs.txt/(42) CasosUs.txt
(...)
</pre>

Will download all your documents <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":)" class="wp-smiley" /> It's important that you leave the command run until its full completion, otherwise most of the documents (if not all) will not be written to disk. There's another catch as well: watch out for error messages like the following:

<pre>[Errno 2] No such file or directory: '/home/romanvg/tmp/docs.txt/Placement / Keyword Report, Feb 15, 2010 4:29:01 PM Central European Time.xls'
Does your destination filename contain invalid characters?
</pre>

So morale of the story: Keep your filenames short, lowercased and simple, the UNIX way or clutter your nifty code with error-handling <img src="http://blogs.nopcode.org/brainstorm/wp-includes/images/smilies/icon_smile.gif" alt=":)" class="wp-smiley" />

 [1]: http://www.ecko-labs.com/wp-content/themes/mystream/thumb.php?src=wp-content/uploads/2010/06/googlecl21.png&w=390&h=230&zc=1&q=90
 [2]: http://en.wikipedia.org/wiki/All_your_base_are_belong_to_us
 [3]: http://en.wikipedia.org/wiki/Backspace
 [4]: http://www.dataliberation.org/google/google-docs
 [5]: http://code.google.com/p/googlecl/
 [6]: http://code.google.com/p/googlecl/issues/detail?id=290&q=braincode&colspec=ID%20Type%20Stars%20Status%20Priority%20Milestone%20Owner%20Summary
 [7]: http://code.google.com/p/gdata-python-client/source/checkout