---
author: brainstorm
date: '2016-05-31T14:00:00.000000+00:00'
excerpt: Howto cross-compile for "old" architectures such as ARMv5 with Docker
layout: post
modified: '2016-05-31T14:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2016/07/26/cross-compiling-with-docker
tags:
- code
- docker
- reversing
title: Cross-compiling binaries for multiple architectures with Docker
---

## Cross compiling toolchains today

When embarquing into **embedded systems development** to [fix relatively old hardware][repair_drone], a need becomes clear fairly early: recompiling software
on an embedded system becomes a **daunting task** due to too old base system or simply **irritatingly slow to iterate**.

That is why distributions such as Debian really shine with their [support and commitment for multiple architectures][debian_architectures]. So all is fun and happy development until someone reaches out to the documentation on how to [setup a cross-compiling toolchain][linaro_xcompile].

But wait, there's [crosstool-ng][crosstool-ng], seems to be a wrapper that could help me cross-build my binaries... 

```
$ ct-ng armv5-linux-gnueabi
make: *** No rule to make target `armv5-linux-gnueabi'.  Stop.
$ ct-ng armv6-rpi-linux-gnueabi
  LN    config
  MKDIR config.gen
  IN    config.gen/arch.in
  IN    config.gen/kernel.in
  IN    config.gen/cc.in
  IN    config.gen/binutils.in
  IN    config.gen/libc.in
  IN    config.gen/debug.in
  CONF  config/config.in
#
# configuration written to .config
#

***********************************************************

Initially reported by: Yann E. MORIN
URL: http://ymorin.is-a-geek.org/

Comment:
Toolchain for the Raspberry Pi, with hard-float.

***********************************************************
```

So that is [hard-float][hard-float] raspberry pi support by default, which will give me an `Invalid Instruction` exception so I should activate `CT_ARCH_FLOAT_SW`
in the `.config` and then... **sigh**.

![Dockerize compiling away](https://blogs.nopcode.org/brainstorm/images/2016/06/xcompile_jackie_chan.jpg)


## Cross compiling toolchains with DockCross

  Enter [dockcross][dockcross] where after [a small pullrequest][dockcross_thewtex] to support [ARMv5][arm7] with no hard-float support and **installing the dockcross tool once**:
  
```
$ docker run thewtex/cross-compiler-linux-armv5 > ~/bin/dockcross
$ chmod +x ~/bin/dockcross
```
  
  Cross-compiling for your [drone][repair_drone], your [favorite reverse engineering framework][radare_xcompile] or your [shader-based RetroPie frontend][TurboLoader] is just two commands away:
  
```
$ dockcross --image thewtex/cross-compiler-linux-armv5 ./configure --with-compiler=armel --host=armel
$ dockcross make
```

  For an extra icing in the cake, it is worth mentioning that [DockCross][dockcross] also supports **QEMU emulation** that **can be used in the same container**, right after your binaries are built.
  
  Furthermore, for the cherry in the cake, have a look at [TravisCI running QEMU for ARM][travisci-arm], unbelievable.
  
  Happy hacking and x-compiling!
  
## QEMUlating SDL/OpenGL inside Docker for Mac?

Using [Nut][nut-devel] to **spawn containers with X11 support**:

```
$ cat nut.yml
syntax_version: "6"
project_name: TurboLoader
macros:
  run:
    usage: xcompile and qemu TurboLoader in a docker container
    docker_image: aretroui
    privileged: true
    enable_gui: true
    security_opts:
    - seccomp=unconfined
```

While [other apps such as Chrome work well][docker-on-desktop], **raw OpenGL shader support is [unfortunately not there yet in Docker's HyperKit][hyperkit_gpu] under OSX** (hint: use native Linux instead for now with [nvidia-docker][nvidia-docker]):

```
$ nut run
** Test project **
¿¿ Smoke test Lua ??
¿¿ Startup environment ??
libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast
X Error of failed request:  BadValue (integer parameter out of range for operation)
  Major opcode of failed request:  150 (GLX)
  Minor opcode of failed request:  3 (X_GLXCreateContext)
  Value in failed request:  0x0
  Serial number of failed request:  82
  Current serial number in output stream:  83
```

 [dockcross]: https://github.com/dockcross/dockcross
 [dockcross_thewtex]: https://github.com/dockcross/dockcross/pull/9
 [thewtex_jessie]: https://github.com/dockcross/dockcross/pull/10
 [mxe]: https://github.com/mxe/mxe
 [linaro_xcompile]: https://wiki.linaro.org/Platform/DevPlatform/CrossCompile/CrossBuilding
 [hyperkit_gpu]: https://github.com/docker/hyperkit/issues/20
 [crosstool-ng]: http://crosstool-ng.org/
 [repair_drone]: http://blogs.nopcode.org/brainstorm/2016/06/10/the-right-to-repair-my-drone
 [debian_architectures]: https://wiki.debian.org/SupportedArchitectures
 [hard-float]: https://wiki.debian.org/ArmHardFloatPort
 [arm7]: https://en.wikipedia.org/wiki/ARM7#ARM7EJ
 [TurboLoader]: https://github.com/seriema/TurboLoader
 [radare_xcompile]: https://github.com/radare/radare2/pull/5060
 [nut-devel]: https://github.com/matthieudelaro/nut
 [docker-on-desktop]: https://blog.jessfraz.com/post/docker-containers-on-the-desktop/
 [nvidia-docker]: https://github.com/NVIDIA/nvidia-docker
 [travisci-arm]: https://www.tomaz.me/2013/12/02/running-travis-ci-tests-on-arm.html