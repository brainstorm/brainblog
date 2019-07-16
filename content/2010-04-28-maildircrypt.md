---
author: brainstorm
categories:
- Uncategorized
date: '2010-04-27T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889635
excerpt: null
layout: post
modified: '2010-04-27T14:00:00.000000+00:00'
openid_comments:
- a:2:{i:0;s:4:"8538";i:1;s:4:"8545";}
permalink: https://blogs.nopcode.org/brainstorm/2010/04/28/maildircrypt/
tags:
- security
- sysadmin
- unix
title: MaildirCrypt
---

Just had a conversation with one good [friend][1], rambling on the possibility of having a server-side crypted Maildir INBOX.

It seems that [dovecot][2] has a nice server-side mail [compression plugin][3]... how come there are no implementations of a session-based cyphering of mail storage based on this same principle ?

The use case is quite simple to imagine:

<del datetime="2010-04-28T19:50:58+00:00"> 
  <ol>
    <li>
      <del datetime="2010-04-28T19:57:29+00:00">Mails keep coming to the <acronym title='Mail Delivery Agent'>MDA</acronym>, on plain text, as usual.</del>
    </li>
    <li>
      <del datetime="2010-04-28T19:58:34+00:00">User logs into the IMAP server with her credentials.</del>
    </li>
    <li>
      <del datetime="2010-04-28T19:58:34+00:00">A keypair stored on user's maildir, for instance, is unlocked using the login password.</del>
    </li>
    <li>
      <del datetime="2010-04-28T19:58:34+00:00">New mails are cyphered and all subsequent read/write operations are performed through this (de)cyphering mechanism.</del>
    </li>
    <li>
      <del datetime="2010-04-28T19:58:34+00:00">User logs out and the key is forgotten, shrinking the window of opportunity on possible sneakers or forensic forces.</del>
    </li>
  </ol>



  <p>
    </del>
  </p>



  <p>
    EDIT: Of course, keeping just the public key on the server is way smarter in this case:
  </p>



  <ol>
    <li>
      The user's GnuPG <strong>public</strong> key is stored on her maildir.
    </li>
    <li>
      All incoming emails are cyphered as they arrive with the previous public key.
    </li>
    <li>
      The user logs in and sees all her mailbox cyphered, ready to be decyphered with his private key residing on her mail client.
    </li>
    <li>
      Forensic analysis/spying on emails gets just a little bit harder :).
    </li>
  </ol>



  <p>
    He argued that just a quick UNIX pipework (using <a href="https://cr.yp.to/qmail.html">qmail</a>) should be sufficient, but I rather preferred to keep the <acronym title='Mail Transport Agent'>MTA</acronym> out of the equation. The reason is that the MTA just "sinks" mail to the mailbox, while the MDA usually has both read and write access to emails, so to me it makes more sense to keep this "plugin" on the MDA side...
  </p>



  <p>
    Is the idea clear at this point ? Is this already invented and passed under my radar ? Anyone has suggestions on top of that ?
  </p>

 [1]: https://kung-foo.dhs.org/killabyte/
 [2]: https://dovecot.org/
 [3]: https://wiki.dovecot.org/Plugins/Zlib