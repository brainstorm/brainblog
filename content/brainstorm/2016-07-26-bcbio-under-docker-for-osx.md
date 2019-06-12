---
author: brainstorm
date: '2016-07-25T14:00:00.000000+00:00'
excerpt: Bioinformatics with bcbio-nextgen on your Mac
layout: post
modified: '2016-07-25T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2016/07/26/bcbio-under-docker-for-osx
tags:
- code
- bioinformatics
- bcbio
title: Running bcbio-nextgen and CWL with Docker for Mac
---

## The case for local, dockerized bioinformatics

![Dockerized portable bioinformatics](https://blogs.nopcode.org/brainstorm/images/2016/07/docker_bioinformatics.png)

Since I started using docker on my local computer (a MacBook Air 11" at the time 
of writing this), I **encountered issues with boot2docker+VirtualBox combo**. 
Installing VirtualBox (plus guest additions), [Docker Toolbox][11] via [Brew Casks][12], 
most of the problems [stem from the volume sharing and UID/GID mappings][13] between 
host and docker containers.

Then to my relief I requested access to the private [Docker for Mac beta program][1],
which uses a lightweight hypervisor and base image ([hyperkit][3]+[alpine][4]) to run containers
on OSX, **conveniently hiding the installation woes**. This setup worked quite well and
while [docker on OSX does not yet support GPU passthrough processing yet][2] (for those
interested in things like Tensorflow and Keras), **docker for osx is a really convenient local docker setup.**

Frustratingly, my local docker setup was always accompanied by tests on our 
local HPC cluster that has limited docker support and a small AWS instance. 
Almost **invariably I resumed my development efforts on the HPC/AWS setups instead**, 
[because, you know, beta][5].

In contrast, a large share of my **colleagues
do use OSX** as a ~~workstation~~ **mosh shell** to their respective HPC clusters where they  launch test runs. **Why don't we use the local CPUs a bit more for testing?**

**That was the state of the art with bcbio+docker+osx: not using it locally...**

Until today!

## bcbio-nextgen and CWL

As peers in the bioinformatics community have noticed, the [Common Workflow Language][6]
is getting workflow and pipeline engines migrating to CWL as their underlying
workflow representation, including but not limited to [Arvados][14], [Galaxy][7] and [bcbio-nextgen][8].

In order to have a minimal development environment while migrating bcbio-nextgen
internal logic to CWL, Brad Chapman wrapped a small demo that can launch [bcbio_vm][10],
CWL and [Toil][9] (SLURM support under Toil is a WIP right now).

So please go ahead and:

 1. Install [Docker for Mac][dockermac].
 2. Install [Miniconda][miniconda] if you don't have it already.
 3. `conda install bcbio-nextgen-vm -c bioconda`.
 4. `wget https://s3.amazonaws.com/bcbio/cwl/test_bcbio_cwl.tar.gz` && `tar xvfz test_bcbio_cwl.tar.gz` && `cd test_bcbio_cwl`.
 5. `chmod +x run_cwltool.sh` && `./run_cwltool`.
 
 Those will download a [~2GB bcbio docker image][bcbio_docker_image] and then run
 a sample bioinformatics workflow under docker for OSX in your computer.
 
 Thanks to Robin Andeer for being one of the first brave souls to test this out and
 please feel free to report back your experiences running this experimental setup 
 in the comments section below!

 [1]: https://blog.docker.com/2016/03/docker-for-mac-windows-beta/
 [2]: https://github.com/docker/hyperkit/issues/20
 [3]: https://github.com/docker/hyperkit
 [4]: http://alpinelinux.org/about/
 [5]: https://forums.docker.com/t/docker-volumes-flapping-between-start-and-stop-states-while-the-container-is-running/10641
 [6]: http://commonwl.org
 [7]: http://usegalaxy.org
 [8]: http://bcb.io
 [9]: https://github.com/BD2KGenomics/toil
 [10]: https://github.com/chapmanb/bcbio-nextgen-vm
 [11]: https://blog.docker.com/2015/08/docker-toolbox/
 [12]: https://github.com/caskroom/homebrew-cask
 [13]: https://github.com/docker/docker/issues/7198
 [14]: https://curoverse.com/
 [dockermac]: https://docs.docker.com/docker-for-mac/
 [miniconda]: http://conda.pydata.org/miniconda.html
 [bcbio_docker_image]: https://hub.docker.com/r/bcbio/bcbio/