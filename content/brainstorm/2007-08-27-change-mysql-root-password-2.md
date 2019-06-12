---
author: brainstorm
date: '2007-08-26T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889117
excerpt: null
layout: post
modified: '2007-08-26T14:00:00.000000+00:00'
openid_comments:
- a:1:{i:0;s:4:"8559";}
permalink: https://blogs.nopcode.org/brainstorm/2007/08/27/change-mysql-root-password-2/
tags:
- howto
- security
- software
title: Change MySQL root password
---

So you forgot your MySQL root password ? Don't worry, logging in is easy as 1,2,3:

1.  Stop all running **mysqld** processes (/etc/init.d/mysqld stop or killall mysqld)
2.  \# mysqld **&#8211;skip-grant-tables**
3.  $ **mysql -uroot** and you're in, no password required

Till here, you'll be able to operate with your DB...**but**, you'll not be able to change your password right away:

<pre>mysql&gt; SET PASSWORD FOR root = PASSWORD('will_not_forget_again');
ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables
option so it cannot execute this statement
</pre>

Here's the workaround:

<pre>mysql&gt; USE mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql&gt; UPDATE user
    -&gt; SET PASSWORD=PASSWORD('really:I_will_not_forget_again')
    -&gt; WHERE user="root";
Query OK, 2 rows affected (0.02 sec)
Rows matched: 2  Changed: 2  Warnings: 0
mysql&gt; flush privileges;
Query OK, 0 rows affected (0.01 sec)
</pre>

Thanks to [netadmintools][1] for this quick recipe :)

 [1]: https://www.netadmintools.com/art90.html