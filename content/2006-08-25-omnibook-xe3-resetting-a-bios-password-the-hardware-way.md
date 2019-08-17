---
author: brainstorm
date: '2006-08-24T14:00:00.000000+00:00'
dsq_thread_id:
- 2874888940
excerpt: null
layout: post
modified: '2006-08-24T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2006/08/25/omnibook-xe3-resetting-a-bios-password-the-hardware-way/
tags:
- diy
- hardware
- howto
title: 'Omnibook XE3: Resetting a BIOS password the hard(ware) way'
---

Everything started with the following question:

> Hi dude, tomorrow I'm going to Helsinki and I don't remember this laptop's password, can you help me ? 

A few searches looking for the typical master or universal passwords didn't help much (nope, "admin" does not work as a backdoor bios password as most sites say, really). The owner was really fed up with HP support and it was quite late at night, so calling nor sending emails were going to help either.

Searching a bit deeper on forums, people were complaining about the same problem without solution: "Cannot reset omnibook xe3 bios password", and the well known common thrick to erase the bios password was futile (removing the CMOS battery for a while to erase the password and settings). According to some posts, the password was actually stored on a [24C16][1] [EEPROM][2] close to the BIOS chip... So there we go :) 

<!--more-->

First, locate and remove the [smd][3] chip (excuse me for the poor quality of the picture :-/):

<center>
  <a class="imagelink" href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/p9200372.jpg" title="Omnibook motherboard"><img id="image43" src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/p9200372.thumbnail.jpg" alt="Omnibook motherboard" /></a>
</center>

Second, solder wires with metal legs (taken from spare resistors, for instance) to the chip to adapt it to a TE20 programmer:

<center>
  <a class="imagelink" href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/p9200371.jpg" title="TE20 programmer with 24C16 attached"><img id="image44" src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/p9200371.thumbnail.jpg" alt="TE20 programmer with 24C16 attached" /></a>
</center>

Third, download [icprog][4] and [configure it properly][5] (I use TE20 which is recognized as a JDM programmer, but YMMV). Now, it comes the most interesting part... figuring out where is the password from the eeprom hexdump... will it be human readable ? cyphered ? Let's see:

<center>
  </p> <table>
    <tr>
      <td>
        <a class="imagelink" href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/icprog_capt2.PNG" title="ICprog screenshot 2"><img id="image46" src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/icprog_capt2.thumbnail.PNG" alt="ICprog screenshot 2" /></a>
      </td>
      
      <td>
        <a class="imagelink" href="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/icprog_capt1.PNG" title="ICprog screenshot one"><img id="image45" src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2006/08/icprog_capt1.thumbnail.PNG" alt="ICprog screenshot one" /></a>
      </td>
    </tr>
  </table>
  
  <p>
    </center>
  </p>
  
  <p>
    The first number matches with the laptop's serial number, good... there's garbage in the middle (perhaps a ciphered password?) and then more serial numbers found on the chassis (some of them partially) and one unknown string: 15371K... perhaps the cleartext password ?
  </p>
  
  <p>
    I was running out of time so I decided to write the eeprom filling the strange middle characters with 0&#8242;s in an attempt to clear a possibly ciphered password... and if the laptop prompted for a password, I would try 15371K... None of these finally occured:
  </p>
  
  <p>
    Re-soldered the chip back, reassembled the laptop (a painful task, lot's of screws !) and switched it on. The initial boot screen was frozen on the first 2 reboots, without password prompt, just a static HP logo in the middle of the screen with no response, but wait, don't panic !
  </p>
  
  <p>
    After rebooting for three times on a row, the laptop complained about corrupted bios, one more reboot and I was able to enter the BIOS ! :) Happy end, isn't it ? Well, sort of, there are some pending curiosity issues here:
  </p>
  
  <ul>
    <li>
      It seems that the bios detected corrupted data on 24C16 and then restored or cleared its contents, then:
    </li>
    <ol>
      <li>
        Which was the actual password ? The cleartext one or the "ciphered" one ?
      </li>
      <li>
        Was the password really inside this particular eeprom or somewhere else ?
      </li>
    </ol>
    
    <li>
      Perhaps this process was not necessary and the forums guys were wrong about the uselessness of the battery trick
    </li>
  </ul>
  
  <p>
    Nevermind, the laptop is now 100% functional with Ubuntu installed and flying to Helsinki ;)
  </p>

 [1]: https://www.futurlec.com/Memory/24C16SMD.shtml
 [2]: https://en.wikipedia.org/wiki/EEPROM
 [3]: https://en.wikipedia.org/wiki/Surface-mount_technology
 [4]: https://www.mercaelectronica.com/descargas/index.htm
 [5]: https://www.iearobotics.com/proyectos/skypic/docs/conf_icprog.html
