---
author: brainstorm
categories:
- Uncategorized
date: '2011-04-20T14:00:00.000000+00:00'
dsq_thread_id:
- 2874888812
excerpt: null
layout: post
modified: '2011-04-20T14:00:00.000000+00:00'
openid_comments:
- a:1:{i:0;s:4:"8654";}
permalink: https://blogs.nopcode.org/brainstorm/2011/04/21/supervisord-one-process-to-rule-them-all/
tags:
- admin
- software
- sysadmin
- unix
title: 'supervisord: one process to rule them all'
---

[<img src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2011/04/supervisord-150x60.gif" alt="supervisord logo" title="supervisord" width="150" height="60" class="alignleft size-thumbnail wp-image-470" />][1]

When one is developing a daemonized service, it's rather usual to encounter minor errors that require no further attention than just restarting the daemon. That could be like not being able to connect to a remote machine for some time:

`<br />
Traceback (most recent call last):<br />
(...)<br />
File "python2.6/urllib2.py", line 1170, in http_open<br />
   return self.do_open(httplib.HTTPConnection, req)<br />
File "python2.6/urllib2.py", line 1145, in do_open<br />
   raise URLError(err)<br />
urllib2.URLError: <urlopen error [Errno 111] Connection refused><br />
`

Granted, we want to fix this on the code so that the daemon does not die, but meanwhile it's good to have a safety net that we can rely on. That's were [supervisord][1] comes in handy. Let's see how it's done.

<!--more-->

Its [documentation][2] is comprehensive and well written, but one may need a "simplest" example out of the full-featured output from "echo\_supervisord\_conf". The absolute barebones to get up and running is what I wanted to share with you:

<pre>; Ideally to be put under /etc/supervisord.conf
; you can refer to it via "supervisord -c"
; if the fileconf lays somewhere else
[supervisord]
nodaemon=true

[program:name_the_daemon_to_monitor]
command=/path/to/your/daemon
</pre>

**IMPORTANT:** Since supervisord is meant to be the parent of all your monitored processes, you should not have instances of your program running before supervisord. Instead, supervisord will take care of (re)spawning them for you.

Now, we launch supervisord:

<pre>$ supervisord
2011-04-21 13:01:34,159 INFO supervisord started with pid 14806
2011-04-21 13:01:35,161 INFO spawned: 'analyze_sequences' with pid 14807
2011-04-21 13:01:36,162 INFO success: analyze_sequences entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
</pre>

Now, let's do a reality check, kill the program it's monitoring. In my case, "analyze_sequences":

<pre>$ kill 14807
</pre>

And supervisord should gracefully detect that its child dies and respawn it shortly after:

<pre>(...)
2011-04-21 14:09:12,764 INFO exited: analyze_sequences (exit status 143; not expected)
2011-04-21 14:09:13,768 INFO spawned: 'analyze_sequences' with pid 18380
2011-04-21 14:09:14,769 INFO success: analyze_sequences entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
</pre>

So supervisord got that the exit status was unexpected (143 instead of 0 or 2) and put the program back into RUNNING state, sweet !

After this check, one would want to get rid of the "nodaemon" directive so that supervisord runs on background and revise echo\_supervisord\_conf command to include the fancier features it offers. But after what I've shown, the default settings seem very reasonable to me.

Det var lätt som en plett, eller ? :)

 [1]: https://supervisord.org/
 [2]: https://supervisord.org/index.html