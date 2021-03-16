---
title: my-software-configuration
toc: true
category: computer
lang: zh-cn
date: 2021-03-10 13:14:39
tags:
---

配置文件整合

<!-- more -->

记录linux的配置修改

## 配置文件

### ssh

```conf .ssh/config
# ssh代理设置
ServerAliveInterval 22
ProxyCommand nc -v -x 127.0.0.1:1080 %h %p

# 压缩频宽
Compression yes

# 减少重复连线的时间
ControlMaster auto
ControlPath /tmp/ssh-%r@%h:%p

# 延长连线时间
ControlPresist yes

# github的密钥配置
Host github.com
    User git
    IdentityFile ~/.ssh/id_ed25519.github
```

### ~/.bashrc

{% asset_link bashrc.example "~/.bashrc" %}

### ~/.zshrc

{% asset_link zshrc.example "~/.zshrc" %}

### aria2

{% asset_link aria2.conf.example "aria2.conf" %}

### ddclient

{% asset_link ddclient.conf.example "/etc/ddclient/ddclient.conf" %}

### sysctl

```conf
# BBR TCP Congestion
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr

# Reboot after 30 minutes
kernel.panic = 3780
```

记录 Windows10 的设置改动

## Settings (UWP)

默认使用半角标点符号
```
Time & Language
  -> Language
    -> Chinese (Simplified, China)
      -> Microsoft Pinyin
        -> Options
          -> General
            -> Use English punctuations when in Chinese input mode: On

```

## 组策略

### 禁用遥感 [1/2]

```
Administrative Templates
 -> Windows Components
  -> Data Collection and Preview Builds
   -> Allow Telemetry -> Enabled, Options: 0 - Security [Enterprise Only]
```

## 计划程序

### 禁用遥感 [2/2]

```
Task Scheduler Library
 -> Microsoft
  -> Windows
   -> Application Experience
    -> Microsoft Compatibility Appraiser -> Disable
```

## 注册表

## 服务

## 禁用 Connected User Experiences and Telemetry

```powershell
Set-Service DiagTrack -StartupType Disabled
```

### 启用UTC

Open `regedit` and add a `DWORD` value for 32-bit Windows, or `QWORD` for 64-bit one, with hexadecimal value `1` to the registry:
```batch
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\RealTimeIsUniversal
```

[原文](https://wiki.archlinux.org/index.php/System_time#UTC_in_Windows)

### 禁用英特尔CPU幽灵/熔断/僵尸负载漏洞补丁

```batch
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /v FeatureSettingsOverride /t REG_DWORD /d 3 /f

reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /v FeatureSettingsOverrideMask /t REG_DWORD /d 3 /f
```

[微软原文](https://support.microsoft.com/en-us/help/4073119/protect-against-speculative-execution-side-channel-vulnerabilities-in)

### 启用TSX

```batch
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Kernel" /v DisableTsx /t REG_DWORD /d 0 /f
```

[微软原文](https://support.microsoft.com/en-us/help/4531006/guidance-for-disabling-intel-transactional-synchronization-extensions)

### 任务栏时间显示秒钟

```batch
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowSecondsInSystemClock /t REG_DWORD /d 1 /f
```

### MPV

放入 portable_config/{% asset_link mpv.conf.example "mpv.conf" %}

MPV的配置
针对上网本注释了了一些占用高的选项
TODO: [可能的一份更好的配置](https://github.com/Argon-/mpv-config/blob/master/mpv.conf)

## mpv.conf

为了区别化特意用 **\#@\#** 标记出了我做的注释
{% asset_link mpv.conf "mpv.conf" %}

额外的文件:
1. 脚本 `portable_config\scripts\auto-profiles.lua`
2. shaders文件 `portable_config\shaders\`, 请自行取消我的注释以启用shaders
   - `KrigBilateral.glsl`
   - `nnedi3-nns32-win8x4.hook`
   - `nnedi3-nns64-win8x4.hook`
   - `SSimDownscaler.glsl`
   - `SSimSuperRes.glsl`

此配置fork于[@cczzhh的配置](http://bbs.vcb-s.com/thread-2730-1-1.html)

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

