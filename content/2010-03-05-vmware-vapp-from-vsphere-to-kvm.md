---
author: brainstorm
categories:
- Uncategorized
date: '2010-03-04T13:00:00.000000+00:00'
dsq_thread_id:
- 2874888608
excerpt: null
layout: post
modified: '2010-03-04T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2010/03/05/vmware-vapp-from-vsphere-to-kvm/
tags:
- howto
- software
- unix
- virtualization
title: VMware vApp (from vSphere) to KVM
---

So we have a simple migration at hand looking at the current virtualization landscape, right ?

1.  Use virt-convert from python-virtinst. In other words: [<acronym title='Open Virtualization Format'>OVF</acronym>][1] to libvirt's XML
2.  Launch KVM with the resulting migrated files (both images and metadata)
3.  Do trivial configurations inside the guest machine to match host environment

That was my idealized view. It turns to be this way:

1.  Try **virt-convert** to discover that it **fails** when converting from OVF.
2.  Try to **fix libvirt python** code libraries (OVF XML parser).
3.  When the code is fixed, the resulting metadata files are **malformed**.
4.  Try with the commercial "vCenter Converter Standalone": **ERROR**: "OVF contains multiple virtual machines".
5.  Download ~500MB of "VMWare Server" to just use the "vmware-vdiskmanager" binary (1.4MB).
6.  Construct libvirt's xml, with some hand-edited sections.
7.  [virt-manager fails when connecting via VNC][2] to your remote VM due to a Ubuntu-CentOS cross-distro issue involving NetCat parameters. Place a workaround and report the bug.
8.  Optimize both virtual network and disk performance switching to VirtIO.
9.  **Rebuild guest ramdisk** image in order to load VirtIO drivers in the boot process.

<center>
  <br /> <a href="https://dilbert.com/strips/comic/2008-02-12/" title="Dilbert.com"><img src="http://dilbert.com/dyn/str_strip/000000000/00000000/0000000/000000/00000/1000/800/1869/1869.strip.gif" border="0" alt="Dilbert.com" /></a><br />
</center>

Want more details ? Keep reading...  
<!--more-->

## Conversion

At the time of writing these lines, converting a file to <acronym title='Open Virtualization Format'>OVF</acronym> is barely supported by the virt-convert migration tool:  
`<br />
$ virt-convert -v -i ovf -D qcow2 vApp.ovf<br />
Generating output in 'virt-image' format to vm/<br />
ERROR    Couldn't export to file "vm/vm.virt-image.xml":<br />
VM must have a memory setting<br />
`

After fixing some xpath code I manage to create an XML file lacking several sections. Since it seems that we reached a dead end here, it is time to [convert the vmdk image**s**][3] and define the libvirt xml schema later on:  
`<br />
$ qemu-img convert -f vmdk -O qcow2 vApp-disk1.vmdk pfam.qcow2<br />
qemu-img: error while reading<br />
`

Ok, the vmdk files are not in a format that qemu-img can understand, let's convert it with the proprietary tool:  
`<br />
$ ./vmware-vdiskmanager -r disk1.vmdk -t 0 final.vmdk<br />
SSLLoadSharedLibrary: Failed to load library<br />
libcrypto.so.0.9.8: libdir/lib/libcrypto.so.0.9.8/libcrypto.so.0.9.8:<br />
cannot open shared object file: No such file or directory<br />
`

Allright, it seems that needs a specific openssl library that does not match the symbols of the currently installed one on my system. Retrieving that library file from VMWare server tarball solves it:  
`<br />
LD_LIBRARY_PATH="$CWD" ./vmware-vdiskmanager -r disk1.vmdk -t 0 final.vmdk<br />
Converting 1%...<br />
`

It takes a little while, but succeds. I still want qcow2 files to do proper snapshots:  
`<br />
$ qemu-img convert final.vmdk -O qcow2 final.qcow2<br />
`

If you are running SELinux, don't forget to restore the context for your newly created files with "restorecon", otherwise the VM will not launch:  
`<br />
# restorecon final.qcow2<br />
-rw-r--r--  root root system_u:object_r:virt_image_t   final.qcow2<br />
`

Image migration complete !

## Working on the metadata

Now we have to describe basic properties about the virtual machine. Please note that this is a highly "YMMV" section, you should customize it to fit your needs ! But for the impatient:

<pre class="brush: xml; title: ; notranslate" title="">&lt;domain type='kvm'&gt;
  &lt;name&gt;vm&lt;/name&gt;
  &lt;memory&gt;2097152&lt;/memory&gt;
  &lt;currentmemory&gt;2097152&lt;/currentmemory&gt;
  &lt;vcpu&gt;1&lt;/vcpu&gt;
  &lt;os&gt;
    &lt;type arch='x86_64' machine='pc'&gt;hvm&lt;/type&gt;
    &lt;boot dev='hd'/&gt;
  &lt;/os&gt;
  &lt;features&gt;
    &lt;acpi /&gt;
    &lt;apic /&gt;
    &lt;pae /&gt;
  &lt;/features&gt;
  &lt;clock offset='utc'/&gt;
  &lt;on_poweroff&gt;destroy&lt;/on_poweroff&gt;
  &lt;on_reboot&gt;restart&lt;/on_reboot&gt;
  &lt;on_crash&gt;restart&lt;/on_crash&gt;
  &lt;devices&gt;
    &lt;emulator&gt;/usr/local/bin/kvm&lt;/emulator&gt;
    &lt;disk type='file' device='disk'&gt;
      &lt;source file='/var/lib/libvirt/images/vm.qcow2'/&gt;
      &lt;target dev='vda' bus='virtio'/&gt;
    &lt;/disk&gt;
    &lt;interface type='bridge'&gt;
      &lt;mac address='54:52:00:24:34:16'/&gt;
      &lt;source bridge='br0'/&gt;
      &lt;target dev='vnet0'/&gt;
      &lt;model type='virtio'/&gt;
    &lt;/interface&gt;
    &lt;serial type='pty'&gt;
      &lt;source path='/dev/pts/4'/&gt;
      &lt;target port='0'/&gt;
    &lt;/serial&gt;
    &lt;console type='pty' tty='/dev/pts/4'&gt;
      &lt;source path='/dev/pts/4'/&gt;
      &lt;target port='0'/&gt;
    &lt;/console&gt;
    &lt;input type='mouse' bus='ps2'/&gt;
    &lt;graphics type='vnc' port='5900' autoport='yes' keymap='en-us'/&gt;
  &lt;/devices&gt;
&lt;/domain&gt;
</pre>

I use to place this file under: /etc/libvirt/qemu/vm.xml.tmpl. In that way I can edit the template and reload it back to libvirtd.  
`<br />
# virsh undefine vm<br />
(edit)<br />
# virsh define vm.xml<br />
`

You can also use the handy command "virsh edit vm".

Keep in mind "virtio, acpi, apic" tags/attributes, they are not set by default, and can lead to several booting problems.

## Optimize and tweak VM

The default fully virtualized devices are generally slow for production, switching to [paravirtualized drivers][4] is advised to gain good speedups both in HDD and networking.

But this speedups need some tweaks in the guest machine, basically changing "sda" or "hda" to "vda" and loading the appropiate modules during ramdisk init:  
`<br />
(in /boot/grub/menu.lst):</p>
<p>title           Debian GNU/Linux, kernel 2.6.26-2-amd64<br />
root            (hd0,0)<br />
kernel         /boot/vmlinuz-2.6.26-2-amd64 root=/dev/vda1 ro clocksource=acpi_pm quiet<br />
initrd          /boot/initrd.img-2.6.26-2-amd64<br />
`  
`<br />
(in /etc/initramfs-tools/modules) add:</p>
<p>virtio_pci<br />
virtio_blk<br />
`

Then, update the ramdisk image by running:  
`<br />
# update-initramfs -u<br />
`

## Bringing it up

Now, using virt-manager, you expect to see your virtual machine booting... but it complains about not being able to connect via VNC. Looking around launchpad bug I get some pointers and manage to fix the issue and [report my case][2].

As usual I only wish others to avoid wasting their time on those bugs, good luck :)

 [1]: https://en.wikipedia.org/wiki/Open_Virtualization_Format
 [2]: https://bugs.launchpad.net/ubuntu/+source/libvirt/+bug/517478
 [3]: https://blog.bodhizazen.net/linux/convert-vmware-vmdk-to-kvm-qcow2-or-virtualbox-vdi/
 [4]: https://wiki.libvirt.org/page/Virtio