---
author: brainstorm
categories:
- Uncategorized
date: '2011-02-23T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2011-02-23T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2011/02/24/firefox-starta-inte-titta-pa-e-legitimation-och-64-bit-stod/
tags:
- howto
- inet
- software
- unix
title: "Firefox starta inte: Titta på e-legitimation och 64-bit stöd"
---

Så din webbläsaren funkar inte en dag, vad kan jag gjörde ? Kör GDB med firefox !

`<br />
$ firefox -g<br />
+ MOZDIR=/home/cristobal/.mozilla<br />
+ LIBDIR=/usr/lib/firefox-3.6.10<br />
+ APPVER=<br />
+ META_NAME=firefox<br />
+ GDB=/usr/bin/gdb<br />
+ DROPPED=abandoned<br />
+ which /usr/bin/firefox<br />
+ NAME=/usr/bin/firefox<br />
+ [ xfirefox != x ]<br />
+ NAME=/usr/bin/firefox<br />
+ basename /usr/bin/firefox<br />
+ APPNAME=firefox<br />
+ [ ! -f /usr/lib/firefox-3.6.10/firefox ]<br />
+ want_debug=0<br />
+ [ 1 -gt 0 ]<br />
+ want_debug=1<br />
+ shift<br />
+ [ 0 -gt 0 ]<br />
+ FOUND=<br />
+ [ -d /home/cristobal/.mozilla/firefox ]<br />
+ FOUND=firefox<br />
+ FOUND_BETA=<br />
+ BETA_LIST=<br />
+ [ -d /home/cristobal/.mozilla/firefox-3.1 ]<br />
+ [ -d /home/cristobal/.mozilla/firefox-3.5 ]<br />
+ [ -d /home/cristobal/.mozilla/firefox-3.6 ]<br />
+ [ firefox !=  -a  !=  ]<br />
+ [  !=  -a firefox =  ]<br />
+ [ 1 -eq 1 ]<br />
+ [ ! -x /usr/bin/gdb ]<br />
+ mktemp /tmp/mozargs.XXXXXX<br />
+ tmpfile=/tmp/mozargs.bnrfme<br />
+ trap  [ -f "/tmp/mozargs.bnrfme" ] && /bin/rm -f --<br />
"/tmp/mozargs.bnrfme" 0 1 2 3 13 15<br />
+ echo set args<br />
+ echo sh /usr/lib/firefox-3.6.10/run-mozilla.sh /usr/bin/gdb<br />
/usr/lib/firefox-3.6.10/firefox-bin -x /tmp/mozargs.bnrfme<br />
sh /usr/lib/firefox-3.6.10/run-mozilla.sh /usr/bin/gdb<br />
/usr/lib/firefox-3.6.10/firefox-bin -x /tmp/mozargs.bnrfme<br />
+ CMDNAME_USER=firefox sh /usr/lib/firefox-3.6.10/run-mozilla.sh<br />
/usr/bin/gdb /usr/lib/firefox-3.6.10/firefox-bin -x<br />
/tmp/mozargs.bnrfme<br />
GNU gdb (GDB) 7.1-ubuntu<br />
Copyright (C) 2010 Free Software Foundation, Inc.<br />
License GPLv3+: GNU GPL version 3 or later <http: //gnu.org/licenses/gpl.html><br />
This is free software: you are free to change and redistribute it.<br />
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"<br />
and "show warranty" for details.<br />
This GDB was configured as "i486-linux-gnu".<br />
For bug reporting instructions, please see:<br />
<http: //www.gnu.org/software/gdb/bugs/>...<br />
Reading symbols from /usr/lib/firefox-3.6.10/firefox-bin...(no<br />
debugging symbols found)...done.</p>
<p>(gdb) run<br />
Starting program: /usr/lib/firefox-3.6.10/firefox-bin<br />
[Thread debugging using libthread_db enabled]<br />
[New Thread 0xb4f07b70 (LWP 6792)]<br />
[New Thread 0xb46fbb70 (LWP 6793)]<br />
[New Thread 0xb3cffb70 (LWP 6794)]<br />
[New Thread 0xb34feb70 (LWP 6795)]<br />
[New Thread 0xb29ffb70 (LWP 6796)]<br />
[New Thread 0xb1cffb70 (LWP 6797)]<br />
[Thread 0xb1cffb70 (LWP 6797) exited]<br />
[New Thread 0xb1cffb70 (LWP 6798)]<br />
[New Thread 0xb0dffb70 (LWP 6799)]<br />
[New Thread 0xae8ffb70 (LWP 6800)]<br />
[New Thread 0xadeffb70 (LWP 6801)]<br />
[Thread 0xadeffb70 (LWP 6801) exited]<br />
[New Thread 0xadeffb70 (LWP 6806)]<br />
[New Thread 0xace49b70 (LWP 6807)]<br />
[New Thread 0xac648b70 (LWP 6808)]<br />
^C<br />
Program received signal SIGINT, Interrupt.<br />
0xb7fe2422 in __kernel_vsyscall ()</p>
<p>(gdb) info threads<br />
  14 Thread 0xac648b70 (LWP 6808)  0xb7fe2422 in __kernel_vsyscall ()<br />
  13 Thread 0xace49b70 (LWP 6807)  0xb7fe2422 in __kernel_vsyscall ()<br />
  12 Thread 0xadeffb70 (LWP 6806)  0xb7fe2422 in __kernel_vsyscall ()<br />
  10 Thread 0xae8ffb70 (LWP 6800)  0xb7fe2422 in __kernel_vsyscall ()<br />
  9 Thread 0xb0dffb70 (LWP 6799)  0xb7fe2422 in __kernel_vsyscall ()<br />
  8 Thread 0xb1cffb70 (LWP 6798)  0xb7fe2422 in __kernel_vsyscall ()<br />
  6 Thread 0xb29ffb70 (LWP 6796)  0xb7fe2422 in __kernel_vsyscall ()<br />
  5 Thread 0xb34feb70 (LWP 6795)  0xb7fe2422 in __kernel_vsyscall ()<br />
  4 Thread 0xb3cffb70 (LWP 6794)  0xb7fe2422 in __kernel_vsyscall ()<br />
  3 Thread 0xb46fbb70 (LWP 6793)  0xb7fe2422 in __kernel_vsyscall ()<br />
  2 Thread 0xb4f07b70 (LWP 6792)  0xb7fe2422 in __kernel_vsyscall ()<br />
* 1 Thread 0xb5df1770 (LWP 6789)  0xb7fe2422 in __kernel_vsyscall ()</p>
<p>(gdb) thread 14<br />
[Switching to thread 14 (Thread 0xac648b70 (LWP 6808))]#0  0xb7fe2422<br />
in __kernel_vsyscall ()</p>
<p>(gdb) bt<br />
#0  0xb7fe2422 in __kernel_vsyscall ()<br />
#1  0xb7fbb245 in sem_wait@@GLIBC_2.1 () from /lib/tls/i686/cmov/libpthread.so.0<br />
#2  0xacfe90e9 in event_wait () from /usr/local/lib/libtokenapi.so<br />
#3  0xad32a4a4 in CInternalThreadObject::EventHandlingThread() () from<br />
/usr/local/lib/personal/libP11.so<br />
#4  0xad32dec6 in CHandleTokenEventThread::Run() () from<br />
/usr/local/lib/personal/libP11.so<br />
#5  0xacfe8934 in CThread_posix::RunInternal() () from<br />
/usr/local/lib/libtokenapi.so<br />
#6  0xacfe8981 in CThread_posix::RunProcess(void*) () from<br />
/usr/local/lib/libtokenapi.so<br />
#7  0xb7fb496e in start_thread () from /lib/tls/i686/cmov/libpthread.so.0<br />
#8  0xb638ca4e in clone () from /lib/tls/i686/cmov/libc.so.6<br />
</http:>`

Visst, **/usr/local/lib/libtokenapi.so** är den skyldige. Följa [en andra post här][1] om du vill fixa det, lycka till ! :)

 [1]: http://blogs.nopcode.org/brainstorm/2010/06/21/swedish-vs-spanish-digital-certificate-hacks/