---
title: kvm
category: computer
lang: zh-cn
date: 2020-05-20 10:31:33
tags:
toc: true
---

Some notes of KVM & GVT-g

<!-- more -->

for tpm support install `swtpm`

## GVT-g with i915ovmfPkg

Using the VBIOS [i915ovmfPkg](https://github.com/patmagauran/i915ovmfPkg), these paramters were no longer works:
```xml
<qemu:arg value='-set'/>
<qemu:arg value='device.hostdev0.ramfb=on'/>
<qemu:arg value='-set'/>
<qemu:arg value='device.hostdev0.driver=vfio-pci-nohotplug'/>
<qemu:arg value='-set'/>
<qemu:arg value='device.hostdev0.x-igd-opregion=on'/>
```
Instead, using these:
```xml
<qemu:arg value='-fw_cfg'/>
<qemu:arg value='name=etc/igd-opregion,file=/opregion.bin'/>
<qemu:arg value='-fw_cfg'/>
<qemu:arg value='name=etc/igd-bdsm-size,file=/bdsmSize.bin'/>
```

Meanwhile, I've set some minor stuffs such like KVM hidden, vendor_id, full KVM mode and cpu pins (specific for my i7-8750H, with a iothread created). Whereas some virtio features for disks and network bridges were enabled to satisfy my experience:
```xml
<domain type='kvm' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
  <iothreads>1</iothreads>
  <cputune>
    <vcpupin vcpu='0' cpuset='2'/>
    <vcpupin vcpu='1' cpuset='8'/>
    <vcpupin vcpu='2' cpuset='3'/>
    <vcpupin vcpu='3' cpuset='9'/>
    <vcpupin vcpu='4' cpuset='4'/>
    <vcpupin vcpu='5' cpuset='10'/>
    <vcpupin vcpu='6' cpuset='5'/>
    <vcpupin vcpu='7' cpuset='11'/>
    <emulatorpin cpuset='0,6'/>
    <iothreadpin iothread='1' cpuset='0,6'/>
  </cputune>
  <features>
    <hyperv mode='custom'>
      <vendor_id state='on' value='GenuineIntel'/>
    </hyperv>
    <kvm>
      <hidden state='on'/>
    </kvm>
    <ioapic driver='kvm'/>
  </features>
  <devices>
     <!-- Despite I've added the virtio mouse and keyboard, the PS2 devices cannot be removed as they are an internal function of the emulated Q35/440FX chipsets -->
    <input type='mouse' bus='virtio'/>
    <input type='keyboard' bus='virtio'/>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2' cache='none' io='native' discard='unmap' iothread='1' queues='8'/>
      <source file='/mnt/storage4/win10.qcow2'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <interface type='bridge'>
      <mac address='11:45:14:19:19:81'/>
      <source bridge='br0'/>
      <model type='virtio'/>
    </interface>
  </devices>
</domain>
```
