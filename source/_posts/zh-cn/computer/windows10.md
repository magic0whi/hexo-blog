---
title: windows10
category: computer
date: 2020-02-08 16:42:49
tags:
toc: true
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