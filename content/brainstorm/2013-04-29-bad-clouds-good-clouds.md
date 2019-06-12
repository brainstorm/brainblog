---
author: brainstorm
categories:
- Uncategorized
date: '2013-04-28T14:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2013-04-28T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2013/04/29/bad-clouds-good-clouds/
tags:
- cloud
- software
- university
- uppnex
- virtualization
title: Bad clouds, good clouds
---

This is an on-demand blog post, none of the actors are real institutions nor people, anything resembling real life might be pure coincidence :) 

So there's a day, that day when an organization realizes that there's a real need to have a solid cloud platform as an official infrastructure offering. Admit it, we all have some idle cycles we could make better use of.

<blockquote class="twitter-tweet">
  <p>
    Why the Rest of Us Need Virtualization Even If Facebook Doesn’t <a href="https://t.co/E3KdHCMLAu" title="http://feedproxy.google.com/~r/PuppetLabs/~3/tUgrKy9tTRk/">feedproxy.google.com/~r/PuppetLabs/…</a>
  </p>
  
  <p>
    &mdash; Roman Valls (@braincode) <a href="https://twitter.com/braincode/status/328053287620329472">April 27, 2013</a>
  </p>
</blockquote>



# A bad cloud

Then someone owning some computer resources types some commands frenetically in a console and *voilà*, a **beta** cloud service is born, a fiction dialog between **user and cloud provider** follows:

> - This is great! I want to have an account on your new service, where can I get it?  
> - Well, you have to come at our offices, we will **scan your passport**, have a **1 hour long meeting** and then give you an account.  
> - Ermm, ok, I just want to use that service... 

In the meeting, there's an introduction on how to use the service, many people did not prepare their machines before the session and they get stuck by the overcomplicated client installation instructions, which involve installing **pre-compiled binaries and config files inside a .tar.bz** ([ABI][1] issues galore!). Next, you are given a password via SMS, "abc123&#8243;, which **you cannot change** (nor are encouraged to). You should explicitly ask the admins to change it for you.

Dirty secret: if they are not forced to change it, nobody ever does.

After editing some cloud templates text files, your first instance is up and running. Time to clone your [CloudBioLinux][2] copy and get some bioinformatics software installed in it... Unfortunately, it does not take very long to discover that the base distribution is **more than 3 releases old**. The user emails the imaginary beta cloud support and says: 

> - Hi cloud-support! Is it possible to have the newest Ubuntu release as an image?  
> - No it's not at the moment.  
> - Emm, ok, I tried to apt-get dist-upgrade but it just runs out of space, can I get more space in the VM to do that upgrade myself then? **One cannot do much with 4GB of disk** these days, you know.  
> - No, it is not possible, you can use the 1TB NFS-mounted scratch space instead.  
> - **Why isn't it possible?** Anyway, I see no straightforward way to use that scratch as an extension of the OS, while the VM is running, and I cannot access the filesystem offline and move, say, /usr away without doing some hackish stuff involving squashfs, ramdisks, etc... this is actually giving me more headaches than is worth.  
> - I'm sorry, we cannot bundle another distribution for you. 

So what can we learn from that experience? What can a bad cloud do to become a better cloud?

1.  Distributing a **readily installable and tested client CLI package** for the most popular platforms instead of a precompiled *.tar.gz* would have cut down that **1 hour long meeting to nil**. Documentation should never be a substitute nor shortcut for a tested, directly installable package.
2.  Distributing passwords, even via SMS, **should adhere to basic [good password policies][3]** at all times, even in beta services. Go [double factor authentication][4] if you fancy it.
3.  All services should be **auto-provisioned**. Asking sysadmins to perform routine operations like changing passwords should be off the table.
4.  Dimensioning a cloud (disk, memory, network interfaces) is not an easy task if the users have wildly different needs, but at least, it should be possible to easily **increase VM image space**, within reasonable limits. Other metrics such as RAM, network interfaces, DNS records, mountpoints should be directly accessible to the user, auto-provisioned. 
5.  **Creating new cloud images** from vanilla OS's *automatically* should be in place somehow and before launching the beta.

One year passes, some more console typing and moving to new hardware resources should get the service a new face, ready for a second try.

The same issues arise, instead of automating the deployment of the whole cloud to other machines, it just has been moved to the new hardware. There is **[no evidence of automation][5] being done since last year**.

# A better cloud

[Automated testing][6] is about software, since clouds are software, why not automate bits and pieces of the deployment until it becomes fully automatic? It is easier said than done, it takes a great deal of patience to go and:

1.  Build and test a cloud component.
2.  Automate its deployment, testing it elsewhere.
3.  Take the whole cloud stack down, recreating it again from scratch.
4.  Automate basic user-side (stress)-testing: create instance, record a DNS change, attach new volumes, destroy instance, etc...

**Automation and testing are hard**, it takes time to get them right and not overfit your immediate environment. But look, those guys over there seem to have gotten it right:

> - So you **only need my public SSH key**? That's all? No meetings nor passport, fingerprints, blood samples or photos?  
> - That's exactly right, just login as root, **break as much as you want in your own cloud**, we can wipe your whole stuff out in less than 20 minutes. We'll of course be **gathering metrics from outside**, just in case we detect something bad coming out your instance(s). **We don't want to get in your way**.  
> - Nice! What about having the latest Ubuntu release...  
> - We just provisioned it as we speak (true story).  
> - I'm launching some hadoop jobs right now. [It took me a few minutes to provision the nodes][7]. thank you guys, you're awesome! 

<!--more-->

# Look around, good (and some free beer) clouds!

Now, while our inner clouds get better, how can I get my idea without spending much money on it. What should we tell our users? **They want some cloud action and they want it now**. While [OpenStacks][8] get stacked [properly inside your walls][9], you can go and explore the outside world a bit, we might learn a lot from them!

Those impatient early birds can try locally with something like [Cloudify][10]... Ooops, [Chef recipes][11] do not work out of the box if you have an Apple user.

<blockquote class="twitter-tweet">
  <p>
    Giving a try to @<a href="https://twitter.com/cloudifysource">cloudifysource</a>… <a href="https://twitter.com/search/%23fail">#fail</a> when installing <a href="https://twitter.com/search/%23Chef">#Chef</a> in os.getVendor()==Apple… <a href="https://t.co/iCTSkienR9" title="http://jtimberman.housepub.org/blog/2012/07/29/os-x-workstation-management-with-chef/">jtimberman.housepub.org/blog/2012/07/2…</a>
  </p>
  
  <p>
    &mdash; Roman Valls (@braincode) <a href="https://twitter.com/braincode/status/315120968236412928">March 22, 2013</a>
  </p>
</blockquote>



There are some hiccups that could be circumvented with Virtualbox though:

<blockquote class="twitter-tweet">
  <p>
    @<a href="https://twitter.com/braincode">braincode</a> check this project from @<a href="https://twitter.com/fastconnect">fastconnect</a> <a href="https://t.co/mk72kraH21" title="http://ow.ly/1TRJsg">ow.ly/1TRJsg</a>. We plan to have this as a native capability in Cloudify as well
  </p>
  
  <p>
    &mdash; Uri Cohen (@uri1803) <a href="https://twitter.com/uri1803/status/315195558610485248">March 22, 2013</a>
  </p>
</blockquote>



But wait, there are other alternatives that don't depend on your workstation/laptop whacky configurations, they might not be as powerful (HPCCloud users, move along), but enough for starters. Heroku is a classic, with an excellent CLI tool and **mandatory version control** that can get you started very quickly in the cloud <acronym title="Platform as a Service">PaaS</acronym> arena:

[<img src="https://3.bp.blogspot.com/-bxj9LtU6bJE/UGl0Idls8_I/AAAAAAAAAFo/ld8Pk5OWGGE/s1600/heroku-logo-white.jpg" alt="heroku logo" width="186" height="43" class="alignnone" />][12]

Then, another interesting platform is dotcloud, with its recently published cloud stack/backend, following the same steps as heroku:

[<img src="https://www.dotcloud.com/static/img/logo.png" width="186" height="43" class="alignnone" />][13]

Until a few days ago, free for **best effort** instances were offered, which meant "put your application here, we will run it if we have enough resources left". Now you can build your [own dotcloud][14] in your organization, if [PaaS][15] is what you need.

# Conclusion

Being a <acronym title="Infrastructure as a Service">IaaS</acronym> cloud provider can be a bumpy experience, it is not trivial. In this post I wanted to outline three different worldviews: the hurried cloud in-house service provider, the more automated in-house provider and the industry-quality for-pay provider. The goal is clear yet a tricky one to implement in reality. We want to reach a point where:

1.  **No human intervention** and support is required to instantiate basic and small clouds. **Obsessive automation culture** is paramount to reach that stage.
2.  Enable user auto-provisioning as a result of the first point.
3.  Commonly used services wrapped through **public** [**deployment recipes**][16] (cloudify or chef are examples). Encourage [re-use and improvement of those recipes][17].
4.  Enforce good practices in the developer/user side such as **version control as the only way to deploy applications**.
5.  Enforce security **from outside the instances**, **don't get in the way of the users** unless bad things are happening or about to happen. Make sure you have implemented [some non-intrusive security measures][18] before announcing the cloud service.
6.  Provide a consistent **API/CLI toolbox/toolbelt** to control various aspects of the instances, like the industry-quality providers have.

 [1]: https://en.wikipedia.org/wiki/Application_binary_interface "ABI"
 [2]: https://cloudbiolinux.org/
 [3]: https://en.wikipedia.org/wiki/Password_strength
 [4]: https://code.google.com/p/google-authenticator/
 [5]: https://github.com/puppetlabs/puppetlabs-opennebula
 [6]: https://ivory.idyll.org/blog/automated-testing-and-research-software.html
 [7]: https://github.com/guillermo-carrasco/hadoop
 [8]: https://www.openstack.org/
 [9]: https://www.opscode.com/press-releases/opscode-announces-chef-for-openstack/
 [10]: https://www.cloudifysource.org/
 [11]: https://github.com/opscode
 [12]: https://get.heroku.com/
 [13]: https://www.dotcloud.com/
 [14]: https://blog.dotcloud.com/new-sandbox
 [15]: https://en.wikipedia.org/wiki/Platform_as_a_service
 [16]: https://github.com/opscode/cookbooks
 [17]: https://ivory.idyll.org/blog/research-software-reuse.html
 [18]: https://aws.amazon.com/security