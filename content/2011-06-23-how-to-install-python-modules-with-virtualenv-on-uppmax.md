---
author: brainstorm
categories:
- Uncategorized
date: '2011-06-22T14:00:00.000000+00:00'
dsq_thread_id:
- 2874889174
excerpt: null
layout: post
modified: '2011-06-22T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2011/06/23/how-to-install-python-modules-with-virtualenv-on-uppmax/
tags:
- howto
- python
- software
- unix
- uppnex
title: How to install python modules with VirtualEnv... on UPPMAX
---

# UPDATE:

<del datetime="2014-02-03T13:38:42+00:00">This documentation below has been superseeded by a much simpler, generalized and automated alternative: <a href="https://github.com/brainsik/virtualenv-burrito">VirtualEnv-burrito</a></del>.

# UPDATE 2:

Strike last update, seems that [pyenv][1] is now the canonical way to have a virtual python environment across python 2 and python 3...

<!--more-->

## Why bother ?

Both [virtualenv][2] and [virtualenvwrapper][3] ease the hassle of managing python modules when one does not have root access on a system. In addition, no more "&#8211;prefix" flags are needed when installing modules. Or maybe better explained, from the official docs:

> The basic problem being addressed is one of dependencies and versions, and indirectly permissions. Imagine you have an application that needs version 1 of LibFoo, but another application requires version 2. How can you use both these applications? If you install everything into /usr/lib/python2.7/site-packages (or whatever your platform's standard location is), it's easy to end up in a situation where you unintentionally upgrade an application that shouldn't be upgraded.
> 
> Or more generally, what if you want to install an application and leave it be? If an application works, any change in its libraries or the versions of those libraries can break the application.
> 
> Also, what if you can't install packages into the global site-packages directory? For instance, on a shared host.
> 
> In all these cases, virtualenv can help you. It creates an environment that has its own installation directories, that doesn't share libraries with other virtualenv environments (and optionally doesn't access the globally installed libraries either). 

After this howto you'll be able to create an isolated clean python environment where you can install as many python modules as you want and where your PYTHONPATH, PYTHONHOME and friends are not tainted... unless there's a [module system][4] in the way, oh, my !

We'll see how to tame that beast too. Keep reading.

## Go ahead

First we'll edit our **.bashrc**. The "~/opt/mypython" directory is needed in order to bootstrap the virtualenvs:

`<br />
# User specific aliases and functions</p>
<p>export PATH=$PATH:~/opt/mypython/bin<br />
export PYTHONPATH=~/opt/mypython/lib/python2.6/site-packages<br />
source ~/opt/mypython/bin/virtualenvwrapper.sh<br />
export WORKON_HOME=~/.virtualenvs<br />
`

Then, we install virtualenv(wrapper) by running 3 commands:

`<br />
$ source ~/.bashrc >& /dev/null && mkdir -p $HOME/opt/mypython/lib/python2.6/site-packages<br />
$ easy_install --prefix=~/opt/mypython pip<br />
$ pip install virtualenvwrapper --install-option="--prefix=~/opt/mypython" && source ~/.bashrc<br />
`

Once we have that, we create a virtual environment called "devel", or whatever name you prefer. That will ignore whatever is installed on the cluster (note the &#8211;no-site-packages flag):

`<br />
$ mkvirtualenv --python=python2.6 --no-site-packages devel<br />
`

If you need another python version (such as 2.7) you first have to deactivate the [module system][5] and install a new clean environment:

`<br />
module unload python && mkvirtualenv --no-site-packages --python=/sw/comp/python/2.7_kalkyl/bin/python py2.7<br />
`

## Module system fixes

Finally, if you want this virtual environment to go along well with [the module system][4] in [UPPMAX][6] you should define the following code in **~/.virtualenvs/devel/bin/postactivate**:

`<br />
#!/bin/bash<br />
# This hook is run after this virtualenv is activated.</p>
<p>source ~/bin/reload_uppmax_modules.sh</p>
<p># We don't want UPPMAX's custom python<br />
PATH="${PATH/'/sw/comp/python/2.6.6_kalkyl/bin'}"</p>
<p># We unset PYTHONHOME set by the module system,<br />
# otherwise the system will not use the python<br />
# of virtualenv<br />
unset PYTHONHOME<br />
`

You can have a look [at how I load my modules][7], or skip reload\_uppmax\_modules entirely if you use another approach.

## Installing my crazy module

Installing new modules is as simple as running "pip", no more 90&#8242;s .tar.bz2 and environment variables manual munging needed:

`<br />
(devel)$ pip search cutadapt</p>
<p>cutadapt                  - trim adapters from high-throughput sequencing reads</p>
<p>(devel)$ pip install cutadapt</p>
<p>Downloading/unpacking cutadapt<br />
  Downloading cutadapt-0.9.4.tar.gz (46Kb): 46Kb downloaded<br />
  Running setup.py egg_info for package cutadapt<br />
Installing collected packages: cutadapt<br />
  Running setup.py install for cutadapt<br />
    building 'cutadapt.calign' extension<br />
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c lib/cutadapt/calignmodule.c -o build/temp.linux-x86_64-2.6/lib/cutadapt/calignmodule.o<br />
    gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions build/temp.linux-x86_64-2.6/lib/cutadapt/calignmodule.o -o build/lib.linux-x86_64-2.6/cutadapt/calign.so<br />
    changing mode of build/scripts-2.6/cutadapt from 644 to 755<br />
    changing mode of /home/romanvg/.virtualenvs/devel/bin/cutadapt to 755<br />
Successfully installed cutadapt<br />
Cleaning up...<br />
`

See your "(devel)" prefix on your prompt ? Remember that you can create as many virtual environments as you want with the **mkvirtualenv** command shown above... And switch between those environments with the "workon" command:

`<br />
$ cutadapt<br />
-bash: cutadapt: command not found<br />
$ workon devel<br />
(devel)$ cutadapt --version<br />
0.9.4<br />
`

For the [rubyists][8] out there, there's a similar tool called [RVM][9] and for the [camels][10], there's [local::lib][11].

Choose your weapon and enjoy your new [userland][12] freedom :)

 [1]: https://www.uppmax.uu.se/how-to-install-your-own-python-modules-or-specific-python-version
 [2]: https://pypi.python.org/pypi/virtualenv
 [3]: https://www.doughellmann.com/projects/virtualenvwrapper/
 [4]: https://modules.sourceforge.net/
 [5]: https://blogs.nopcode.org/brainstorm/2011/11/23/module-system-bad-and-ugly/
 [6]: https://www.uppmax.uu.se/
 [7]: https://raw.github.com/brainstorm/scilifelab/master/scripts/reload_uppmax_modules.sh
 [8]: https://www.ruby-lang.org/en/
 [9]: https://rvm.beginrescueend.com/
 [10]: https://www.perl.org/
 [11]: https://terrarum.net/development/perl-virtual-environments.html#locallib
 [12]: https://en.wikipedia.org/wiki/User_space