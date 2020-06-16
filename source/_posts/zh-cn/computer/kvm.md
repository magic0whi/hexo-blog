---
title: kvm
category: computer
lang: zh-cn
date: 2020-05-20 10:31:33
tags:
toc: true
---

This article includes:
1. KVM + Libvirt + WebVirtCloud
2. PCI passthrough via OVMF

<!-- more -->

## KVM + Libvirt + WebVirtCloud 安装

先安装这个[webvirtcloud](https://github.com/magic0whi/aur-webvirtcloud-git)
然后执行 `/srv/webvirtcloud/configuration-install.sh`
最后启动服务 `systemctl start nginx supersivord libvirtd-tcp.socket`
基本就完事了

## Isolating the GPU

将显卡通过PCI透传提供给虚拟机使用
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

然后注意, 如果你的PCIe插槽未作隔离(PCIe的IOMMU组中有多个设备, 包括PCIe本身), 如这样
```
IOMMU Group 1:
	 00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v2/3rd Gen Core processor PCI Express Root Port [8086:0151] (rev 09)
	 01:00.0 VGA compatible controller: NVIDIA Corporation GM107 [GeForce GTX 750] (rev a2)
	 01:00.1 Audio device: NVIDIA Corporation Device 0fbc (rev a1)
```
解决方案(两种):
* 把要透传的显卡换一个PCIe插槽
* 安装带 ACS override patch 的 [linux-vfio](https://aur.archlinux.org/packages/linux-vfio/), 然后 [kernel parameter](https://wiki.archlinux.org/index.php/Kernel_parameter) 加上 `pcie_acs_override =id:8086:0151` , 其中 `8086:0151` 是你的PCI插槽的 VendorID:DeviceID

关于此问题更多详见[IOMMU Gotchas](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Gotchas)


### Binding vfio-pci via device ID

`lspci -nn | grep -i nvidia` 查看显卡VendorID, 得到
```
03:00.0 VGA compatible controller [0300]: NVIDIA Corporation GF119 [GeForce GT 610] [10de:104a] (rev a1)
03:00.1 Audio device [0403]: NVIDIA Corporation GF119 HDMI Audio Controller [10de:0e08] (rev a1)
```

[kernel parameter](https://wiki.archlinux.org/index.php/Kernel_parameter) 加上 `vfio-pci.ids=10de:13c2,10de:0fbb`

### Loading vfio-pci early

```conf /etc/mkinitcpio.conf
MODULES=(... vfio_pci vfio vfio_iommu_type1 vfio_virqfd ...)
...
HOOKS=(... modconf ...)
```

这里注意如果你两张显卡是相同型号(VendorID相同), 不能用这个方法, 请参阅[Using identical guest and host GPUs](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Using_identical_guest_and_host_GPUs)

### Add GPU device to VM

由于N卡有两个设备,一个是VGA(PCI地址03:00.0), 另一个是 Audio(PCI地址03:00.1)
就要有2个\<hostdev\>

执行`virsh -c qemu:///system edit <VM-Name>` 添加下列字段:
```xml
  <devices>
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

这里我总线地址选了 `0x09` , 只要没被xml下别的虚拟设备声明的PCI总线地址都行

这里我用了经过GOPupd升级的自定义的VBIOS, 因为我的GT610固件不支持EFI
5-22-20: GT610不行, PCI PassThrough 下只要从VGA模式切换到带3D加速的显卡驱动就会Kernel Freeze, 最终只能充当 boot_vga