---
title: openwrt
category: computer
lang: zh-cn
date: 2020-04-27 09:51:33
tags:
toc: true
---

记录小米路由器3G 的 Openwrt 配置


## 升级Openwrt

* sysupgrade方式
```console
# cd /tmp
# wget https://downloads.openwrt.org/snapshots/targets/ramips/mt7621/openwrt-ramips-mt7621-mir3g-squashfs-sysupgrade.tar
# sysupgrade /tmp/openwrt-ramips-mt7621-mir3g-squashfs-sysupgrade.tar
```

升级完后最好更新以下新版本系统的软件包
```console
# opkg update
# opkg list-upgradable
# opkg list-upgradable | sed -e "s/\s.*//" | while read PKG_NAME; do opkg upgrade "${PKG_NAME}"; done
```


## 安装的软件包

```
rng-tools
nano
diffutils
htop
openssh-sftp-server # if default ssh server dropbear cannnot connect
luci-ssl
luci-theme-material
luci-app-upnp
luci-app-wol
luci-app-mwan3
```

## 配置

1. 使用硬解随机数生成器(需要路由器支持):
   ```console
   # uci set system.@rngd[0].enabled="1"
   # uci set system.@rngd[0].device="/dev/urandom"
   # uci commit system
   # /etc/init.d/rngd restart
   ```