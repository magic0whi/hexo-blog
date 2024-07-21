---
title: kvm
category: computer
lang: zh-cn
date: 2020-05-20 10:31:33
tags:
toc: true
---

Some notes in KVM & GVT-g

<!-- more -->

for tpm support install `swtpm`
## For btrfs

If you store your kvm images under btrfs filesystem. It is recommend to enable nocow, for example `chattr +C /var/lib/libvirt/images`.

## GVT-g with i915ovmfPkg

Using the VBIOS [i915ovmfPkg](https://github.com/patmagauran/i915ovmfPkg), these paramters were **no longer works**:
```xml
<qemu:arg value='-set'/>
<qemu:arg value='device.hostdev0.ramfb=on'/>
<qemu:arg value='-set'/>
<qemu:arg value='device.hostdev0.driver=vfio-pci-nohotplug'/>
```

Using Qemu GTK to get a more smoothly experience:
```xml
<domain type='kvm' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
  <qemu:commandline>
    <qemu:arg value='-display'/>
    <qemu:arg value='gtk,gl=on'/>
    <qemu:env name='DISPLAY' value=':0'/>
    <qemu:env name='MESA_LOADER_DRIVER_OVERRIDE' value='iris'/>
  </qemu:commandline>
  <qemu:override>
    <qemu:device alias='hostdev0'>
      <qemu:frontend>
        <qemu:property name='display' type='string' value='on'/>
        <qemu:property name='romfile' type='string' value='/i915ovmf.rom'/>
        <qemu:property name='x-igd-opregion' type='bool' value='true'/>
      </qemu:frontend>
    </qemu:device>
  </qemu:override>
</domain>
```

### Hypervisor XML

The number of hugepages is memory size of virtual machine / Hugepagesize.
```console
$ echo "scale=2;<VM Memory size in GiB>*1024^2/$(grep Hugepagesize /proc/meminfo | awk '{print $2}')+1180" | bc
```

E.G. 4096 for a 8GiB VM, add additional 1180 for other uses.
```properties /etc/sysctl.d/40-hugepage.conf
vm.nr_hugepages=5276
```

```xml
<domain type='kvm' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
  <memory unit='KIB'>8388608</memory>
  <memoryBacking>
    <hugepages/>
  </memoryBacking>
  <os>
    <!-- Secure Boot -->
    <loader readonly='yes' secure='yes' type='pflash'>/usr/share/edk2/x64/OVMF_CODE.secboot.4m.fd</loader>
  </os>
  <iothreads>1</iothreads>
  <vcpu placement='static'>8</vcpu>
  <cputune>
    <!-- Use lscpu -e to see cpu topology. Here I pinned CPU2,3,4,5 to hypervisor, CPU1 to emulator and iothread -->
    <vcpupin vcpu='0' cpuset='2'/>
    <vcpupin vcpu='1' cpuset='10'/>
    <vcpupin vcpu='2' cpuset='3'/>
    <vcpupin vcpu='3' cpuset='11'/>
    <vcpupin vcpu='4' cpuset='4'/>
    <vcpupin vcpu='5' cpuset='12'/>
    <vcpupin vcpu='6' cpuset='5'/>
    <vcpupin vcpu='7' cpuset='13'/>
    <emulatorpin cpuset='1,9'/>
    <iothreadpin iothread='1' cpuset='1,9'/>
  </cputune>
  <cpu mode='host-passthrough' check='none' migratable='off'>
    <topology sockets='1' dies='1' cores='4' threads='2'/>
    <cache mode='passthrough'/>
    <numa>
      <cell memory='memory size of virtual machine' unit='KiB' memAccess='shared'/>
    </numa>
  </cpu>
  <features>
    <hyperv mode='custom'>
      <relaxed state='on'/>
      <vapic state='on'/>
      <spinlocks state='on' retries='8191'/>
      <vpindex state='on'/>
      <runtime state='on'/>
      <synic state='on'/>
      <!-- Enable stimer may cause win10 KVM don't boot on i7-8750H -->
      <stimer state='on'>
        <direct state='on'/>
      </stimer>
      <reset state='on'/>
      <vendor_id state='on' value='GenuineIntel'/>
      <!-- vendor_id state='on' value='AuthenticAMD'/> -->
      <frequencies state='on'/>
      <reenlightenment state='on'/>
      <tlbflush state='on'/>
      <ipi state='on'/>
      <evmcs state='on'/>
    </hyperv>
    <kvm>
      <hidden state='off'/>
    </kvm>
    <ioapic driver='kvm'/>
    <!-- Secure Boot -->
    <smm state='on'/>
  </features>
  <clock offset='localtime'>
    <timer name='rtc' tickpolicy='catchup' track='guest'/>
    <timer name='pit' tickpolicy='delay'/>
    <timer name='hpet' present='no'/>
    <timer name='kvmclock' present='no'/>
    <timer name='hypervclock' present='yes'/>
    <timer name='tsc' present='yes' mode='native'/>
  </clock>
  <devices>
     <!-- Through the virtio mouse and keyboard added, the PS2 devices cannot be removed as they are internal function of the emulated Q35/440FX chipsets -->
    <input type='mouse' bus='virtio'/>
    <input type='keyboard' bus='virtio'/>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2' cache='none' io='native' discard='unmap' iothread='1' queues='8'/>
      <source file='/mnt/storage4/win10.qcow2'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <filesystem type='mount' accessmode='passthrough'>
      <driver type='virtiofs'/>
      <source dir='/mnt/storage2/virt_share_dir'/>
      <target dir='mount_tag'/>
    </filesystem>
    <interface type='direct'>
      <mac address='11:45:14:19:19:81'/>
      <source dev='macvtap0' mode='vepa'/>
      <model type='virtio'/>
    </interface>
    <rng model='virtio'>
      <backend model='random'>/dev/random</backend>
    </rng>
    <panic model='hyperv'/>
  </devices>
</domain>
```
