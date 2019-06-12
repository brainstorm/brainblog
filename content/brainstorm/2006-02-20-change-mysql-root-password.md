---
author: brainstorm
date: '2006-02-19T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2006-02-19T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/02/20/change-mysql-root-password/
tags:
- howto
- security
- software
title: Change MySQL root password
---

So you forgot your MySQL database password ? Don't worry, I'll go straight to the point:

*   Stop your running **mysqld** instances (/etc/init.d/mysql-server stop or killall mysqld)
*   \# mysqld **&#8211;skip-grant-tables**
*   \# **mysql -uroot** and you're in, easy, isn't it ?

For god sake, don't do this on a production server leaving mysqld running like this X"D