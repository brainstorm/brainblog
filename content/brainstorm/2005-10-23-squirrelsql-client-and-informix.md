---
author: brainstorm
date: '2005-10-22T14:00:00.000000+00:00'
dsq_thread_id:
- 2874888357
excerpt: null
layout: post
modified: '2005-10-22T14:00:00.000000+00:00'
openid_comments:
- a:1:{i:0;i:11387;}
permalink: https://blogs.nopcode.org/brainstorm/2005/10/23/squirrelsql-client-and-informix/
tags:
- software
- university
- unix
title: SquirrelSQL Client and Informix
---

I'm doing a [database][1] course in my [faculty][2]. First of all, I must say that I and some of my friends disagree on the <acronym title="DataBase Management System"><a href="http://en.wikipedia.org/wiki/DBMS">DBMS</a></acronym> software our teachers use to introduce SQL and common database features. They use [Informix][3], IMHO, a somewhat deprecated, creepy and [privative][4] DBMS. But the purpose of this post is not about complaining, instead, I'll show the features of an impressive opensource cross-platform interactive SQL client I've discovered recently ([squirrel-sql client][5]) and a quick "for dummies" guide to connect to a remote Informix database server (in this particular case, my university DBMS):

As the project main page states: <cite>SQuirreL SQL Client is a graphical Java program that will allow you to view the structure of a JDBC compliant database, browse the data in tables, issue SQL commands etc.</cite>

But it's far better than that, with the included plugins you can have a full-fledged interactive SQL client with syntax highlighting, code completion, batch execution and SQL sentence bookmarks among other features. It has lots of database connectors ready to be used with the most popular database engines without forgetting about rare ones. Unfortunately, our "beloved" informix is a weird case, and it's not included by default. In any case, don't worry, the process to add an Informix connector is not complicated at all, and I'll describe it step by step:  
<!--more-->

1.  Dowload the .jar from the official sourceforge squirrel-sql [project page][5].
2.  I assume that you have Java Runtime Environment installed correctly on your machine, if it's not the case, install it first (hint: java.sun.com).
3.  Download the latest Informix JDBC driver from IBM [website][6]. And uncompress it wherever you want. You can use [bugmenot][7] if you don't want to register. If you're using Gentoo GNU/Linux, just do an "emerge dev-java/jdbc-informix" and you're done.
4.  Execute the .jar file "java -jar squirrel-sql-2.0final-install.jar ".
5.  The installation is straightforward (next, next, yes...), as opposed to Informix DBMS installation procedure (a real <acronym title="Pain In The *ss">PITA</acronym>).
6.  Configure your VPN connection to FIB using their [howto][8].
7.  Now, just fill the missing fields according to the following screenshots:

<table align="center" cellpadding="8">
  <tr>
    <td>
      <a href="http://blogs.nopcode.org/brainstorm/wp-content/images/sqlclient_splash.png"><br /> <img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sqlclient_splash.png' alt='squirrel-sql splash screen' /></a>
    </td>
    
    <td>
      <a href="http://blogs.nopcode.org/brainstorm/wp-content/images/sqlclient1.png"><img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sqlclient1.png' alt='jdbc driver blueprint' /></a>
    </td>
  </tr>
  
  <tr>
    <td>
      Beautiful splash screen, isn't it ?
    </td>
    
    <td>
      Remember to click on "list drivers" after loading the jars.
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://blogs.nopcode.org/brainstorm/wp-content/images/sqlclient2.png"><img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sqlclient2.png' alt='driver alias blueprint' /></a>
    </td>
    
    <td>
      <a href="http://blogs.nopcode.org/brainstorm/wp-content/images/sqlclient4.png"><img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sqlclient4.png' alt='connection settings' /></a>
    </td>
  </tr>
  
  <tr>
    <td>
      Username is your "raco" id.
    </td>
    
    <td>
      Driver properties-> sub_bbdd <br /> is your database.
    </td>
  </tr>
  
  <tr>
    <td>
      <a href="http://blogs.nopcode.org/brainstorm/wp-content/images/sqlclient5.png"><img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sqlclient5.png' alt='catalog view' /></a>
    </td>
    
    <td>
      <a href="http://blogs.nopcode.org/brainstorm/wp-content/images/sqlclient6.png"><img src='http://blogs.nopcode.org/brainstorm/wp-content/images/thumb-sqlclient6.png' alt='sql interactive editor view' /></a>
    </td>
  </tr>
  
  <tr>
    <td>
      View of the databases and catalog.
    </td>
    
    <td>
      SQL editor view.
    </td>
  </tr>
</table>

I've been trying this program for a while and I should say that it works nicely with standard sql select queries, but there are bugs with table and procedure creation (JDBC reports syntax errors (SQLState -201) where none exist (i'll try if the same code works with informix sql editor). I'm almost sure that those problems come from the buggy IBM's JDBC drivers, so it's not a squirrel-sql editor fault.

I wrote a quick [tutorial][9] a few months ago (in catalan) to connect to Informix via socks5 faculty's gateway using [unixODBC][10] and [tsocks][11], it's now deprecated (there was no VPN then).

 [1]: http://www.fib.upc.es/ca/Estudis/Assignatures/BD.html
 [2]: http://www.fib.upc.es
 [3]: http://www-306.ibm.com/software/data/informix/
 [4]: http://es.wikipedia.org/wiki/Software_privativo
 [5]: http://squirrel-sql.sourceforge.net/
 [6]: http://www14.software.ibm.com/webapp/download/search.jsp?go=y&rs=ifxjdbc
 [7]: http://www.bugmenot.com/
 [8]: https://raco.fib.upc.es/vpn
 [9]: http://www-pagines.fib.upc.es/%7Ebd/casaLinux.html
 [10]: http://www.unixodbc.org/
 [11]: http://tsocks.sourceforge.net/