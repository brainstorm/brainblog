---
author: brainstorm
categories:
- Uncategorized
date: '2010-09-11T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2010-09-11T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2010/09/12/android-adb-no-permissions-issue/
tags:
- android
- gadgets
- howto
- software
title: Android ADB no permissions issue
---

It took a while to figure out this little annoying config issue while setting my android phone for development, so here it goes (thanks to comments on [androidboss.com][1]).

At the time of writing these lines, if you follow the official [android documentation][2] on this topic, you'll probably get this frustrating error message.

<pre>$ ~/bin/android/tools/adb devices
List of devices attached 
????????????	no permissions
</pre>

Then, to fix it, you should edit your "/etc/udev/rules.d/50-android.rules" that way:

SUBSYSTEM=="usb", SYSFS{idVendor}=="0fce", MODE**:=**"0666&#8243;

Please, mind the ":=" on MODE, instead of just "=". This last one doesn't work on Ubuntu 10.04. After restarting the udev daemon connecting/desconnecting the device, you should get something like:

<pre>$ ~/bin/android/tools/adb devices
List of devices attached 
CB511JDU5Y	device
</pre>

Hope that helps !

UPDATE: Filed the [issue][3] on android bugtracker.

 [1]: http://androidboss.com/using-android-debug-bridge-adb-in-linux/
 [2]: http://developer.android.com/guide/developing/device.html
 [3]: http://code.google.com/p/android/issues/detail?id=11172