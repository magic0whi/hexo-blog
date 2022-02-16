---
title: kvm
category: computer
lang: zh-cn
date: 2020-05-20 10:31:33
tags:
toc: true
---

This article includes:
1. KVM + Libvirt + WebVirtCloud (TODO)
2. PCI passthrough via OVMF
3. Intel's GVT-g

TODO:
virtio drivers 
https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso

```properties /etc/modules-load.d/kvm.conf
kvmgt
#vfio-iommu-type1
vfio-mdev
```

```properties /etc/mkinitcpio.conf
MODULES=(vfio_pci vfio vfio_iommu_type1 vfio_virqfd)
```



<!-- more -->

## KVM + Libvirt + WebVirtCloud 安装

先安装这个[webvirtcloud](https://github.com/magic0whi/aur-webvirtcloud-git)
然后执行 `/srv/webvirtcloud/configuration-install.sh`
最后启动服务 `systemctl start nginx supersivord libvirtd-tcp.socket`
基本就完事了

## Isolating the GPU

1. Setting up IOMMU
   设置 [kernel parameter](https://wiki.archlinux.org/index.php/Kernel_parameter) , 加上 `intel_iommu=on`
   用这个脚本可以查看IOMMU的分组
   ```bash check-iommu.sh
   #!/bin/bash
   shopt -s nullglob
   for g in /sys/kernel/iommu_groups/*; do
       echo "IOMMU Group ${g##*/}:"
       for d in $g/devices/*; do
           echo -e "\t$(lspci -nns ${d##*/})"
       done;
   done;
   ```
   注意, 如果你的PCIe插槽未作隔离(PCIe的IOMMU组中有多个设备, 包括PCIe本身), 如这样
   ```
   IOMMU Group 1:
	    00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v2/3rd Gen Core processor PCI Express Root Port [8086:0151] (rev 09)
	    01:00.0 VGA compatible controller: NVIDIA Corporation GM107 [GeForce GTX 750] (rev a2)
	    01:00.1 Audio device: NVIDIA Corporation Device 0fbc (rev a1)
   ```
   如果你不想这个 PCI-E 下的 SSD 或其他设备也要透传进虚拟机, 有两种解决方案:
   * 把要透传的显卡换一个PCIe插槽
   * 安装带 ACS override patch 的 [linux-vfio](https://aur.archlinux.org/packages/linux-vfio/), 然后 [kernel parameter](https://wiki.archlinux.org/index.php/Kernel_parameter) 加上 `pcie_acs_override =id:8086:0151` , 其中 `8086:0151` 是你的PCI插槽的 VendorID:DeviceID
   关于此问题更多详见[IOMMU Gotchas](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Gotchas)
2. Binding vfio-pci via device ID
   [kernel parameter](https://wiki.archlinux.org/index.php/Kernel_parameter) 加上 `vfio-pci.ids=10de:13c2,10de:0fbb`
   > If, as noted in [#Plugging your guest GPU in an unisolated CPU-based PCIe slot](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Plugging_your_guest_GPU_in_an_unisolated_CPU-based_PCIe_slot), your pci root port is part of your IOMMU group, you **must not** pass its ID to `vfio-pci`, as it needs to remain attached to the host to function properly. Any other device within that group, however, should be left for `vfio-pci` to bind with.
3. Loading vfio-pci early
   ```properties /etc/mkinitcpio.conf
   MODULES=(... vfio_pci vfio vfio_iommu_type1 vfio_virqfd ...)
   ...
   HOOKS=(... modconf ...)
   ```
   这里注意如果你两张显卡是相同型号(VendorID相同), 不能用这个方法, 请参阅[Using identical guest and host GPUs](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Using_identical_guest_and_host_GPUs)
4. Verifying that the configuration worked
   ```console
   $ lspci -nnk -d 10de:13c2
   06:00.0 VGA compatible controller: NVIDIA Corporation GM204 [GeForce GTX 970] [10de:13c2] (rev a1)
	 Kernel driver in use: vfio-pci
	 Kernel modules: nouveau nvidia
   ```
   ```console
   $ lspci -nnk -d 10de:0fbb
   06:00.1 Audio device: NVIDIA Corporation GM204 High Definition Audio Controller [10de:0fbb] (rev a1)
	 Kernel driver in use: vfio-pci
	 Kernel modules: snd_hda_intel
   ```

## Add GPU device to VM

由于N卡有两个设备,一个是VGA(PCI地址03:00.0), 另一个是 Audio(PCI地址03:00.1)
就要有2个 `<hostdev>`

执行 `virsh -c qemu:///system edit <VM-Name>` 添加下列字段:
```xml
  <devices>
  ...
  <features>
    ...
    <hyperv>
      ...
      <vendor_id state='on' value='randomid'/>
      ...
    </hyperv>
    ...
    <kvm>
      <hidden state='on'/>
    </kvm>
    ...
  </features>
  ...
  <hostdev mode='subsystem' type='pci' managed='yes'>
    <driver name='vfio'/>
    <source>
      <address domain='0x0000' bus='0x03' slot='0x00' function='0x0'/>
    </source>
    <rom file='/var/lib/libvirt/customroms/gt610_updGOP.rom'/>
    <address type='pci' domain='0x0000' bus='0x09' slot='0x00' function='0x0' multifunction='on'/>
  </hostdev>
  <hostdev mode='subsystem' type='pci' managed='yes'>
    <driver name='vfio'/>
    <source>
      <address domain='0x0000' bus='0x03' slot='0x00' function='0x1'/>
    </source>
    <rom file='/var/lib/libvirt/customroms/gt610_updGOP.rom'/>
    <address type='pci' domain='0x0000' bus='0x09' slot='0x00' function='0x1'/>
  </hostdev>
  ...
  </devices>
```


* 如果是笔记本, 注意 ["Error 43: Driver failed to load" with mobile (Optimus/max-q) nvidia GPUs](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#%22Error_43:_Driver_failed_to_load%22_with_mobile_(Optimus/max-q)_nvidia_GPUs)
* 这里我总线地址选了 `0x09`, 只要没被别的虚拟设备声明的 PCI 总线地址都行
* 这里我用了经过 GOPupd 升级的自定义的 VBIOS, 因为我的 GT610 固件不支持 EFI
* GT610不行, PCI PassThrough 下只要从 VGA 模式切换到带 3D 加速的显卡驱动就会 Kernel Freeze, 最终只能充当 boot_vga

## GVT-g

1. Set i915 module parameter `enable_gvt=1`
   You can look up what types are available in your system (and cat description inside of each type to discover what it's capable of) like this:
   ```console
   # ls /sys/devices/pci${GVT_DOM}/$GVT_PCI/mdev_supported_types
   i915-GVTg_V5_1  # Video memory: <512MB, 2048MB>, resolution: up to 1920x1200
   i915-GVTg_V5_2  # Video memory: <256MB, 1024MB>, resolution: up to 1920x1200
   i915-GVTg_V5_4  # Video memory: <128MB, 512MB>, resolution: up to 1920x1200
   i915-GVTg_V5_8  # Video memory: <64MB, 384MB>, resolution: up to 1024x768
   ```
2. libvirt qemu hook
   ```bash etc/libvirt/hooks/qemu
   #!/bin/bash
   GVT_PCI=0000:00:02.0
   GVT_GUID=db10f5f6-0546-4c1c-96c7-1ffe204ae6ce # generated by uuidgen
   # The XML of the domain is feed to the hook script through stdin.
   # You can use xmllint and XPath expression to extract GVT_GUID from stdin, e.g.:
   # GVT_GUID="$(xmllint --xpath 'string(/domain/devices/hostdev[@type="mdev"][@display="on"]/source/address/@uuid)' -)"
   MDEV_TYPE=i915-GVTg_V5_4
   DOMAIN=win10 # Your virtual machine name
   if [ $# -ge 3 ]; then
       if [ $1 = "$DOMAIN" -a $2 = "prepare" -a $3 = "begin" ]; then
           echo "$GVT_GUID" > "/sys/bus/pci/devices/$GVT_PCI/mdev_supported_types/$MDEV_TYPE/create"
       elif [ $1 = "$DOMAIN" -a $2 = "release" -a $3 = "end" ]; then
           echo 1 > /sys/bus/pci/devices/$GVT_PCI/$GVT_GUID/remove
       fi
   fi
   ```
2. Configuration for the VM(`$ virsh -c qemu:///system edit <Your VM Name>`):
   ```xml
   <domain type='kvm' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'>
     ...
     <features>
       ...
       <kvm>
         <hidden state='on'/>
       </kvm>
       ...
     </features>
     ...
     <devices>
       ...
       <graphics type='spice'>
         <listen type='none'/>
         <gl enable='yes'/>
       </graphics>
       ...
       <video>
         <model type='none'/>
       </video>
       ...
       <hostdev mode='subsystem' type='mdev' managed='no' model='vfio-pci' display='on'>
         <source>
           <address uuid='db10f5f6-0546-4c1c-96c7-1ffe204ae6ce'/>
         </source>
         <address type='pci' domain='0x0000' bus='0x05' slot='0x00' function='0x0'/>
       </hostdev>
       <hostdev mode='subsystem' type='pci' managed='yes'>
         <driver name='vfio'/>
         <source>
           <address domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
         </source>
         <address type='pci' domain='0x0000' bus='0x09' slot='0x00' function='0x0' multifunction='on'/>
       </hostdev>
       ...
     </devices>
     ...
     <qemu:commandline>
       <qemu:arg value='-acpitable'/>
       <qemu:arg value='file=/home/ndoskrnl/Work/SSDT1.dat'/>
       <qemu:arg value='-set'/>
       <qemu:arg value='device.hostdev0.x-igd-opregion=on'/>
       <qemu:arg value='-set'/>
       <qemu:arg value='device.hostdev0.ramfb=on'/>
       <qemu:arg value='-set'/>
       <qemu:arg value='device.hostdev0.driver=vfio-pci-nohotplug'/>
       <qemu:arg value='-set'/>
       <qemu:arg value='device.hostdev0.romfile=/home/ndoskrnl/Work/vbios_gvt_uefi.rom'/>
     </qemu:commandline>
   </domain>
   ```
   
