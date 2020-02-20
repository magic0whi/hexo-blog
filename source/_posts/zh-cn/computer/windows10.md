---
title: windows10
category: computer
date: 2020-02-08 16:42:49
tags:
---

记录 Windows10 的设置改动

<!-- more -->

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

### 启用UTC

Open `regedit` and add a `DWORD` value for 32-bit Windows, or `QWORD` for 64-bit one, with hexadecimal value `1` to the registry:
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\RealTimeIsUniversal
```

[原文](https://wiki.archlinux.org/index.php/System_time#UTC_in_Windows)

## AmazingApps

### MPV

放入 portable_config/{% asset_link mpv.conf.example "mpv.conf" %}