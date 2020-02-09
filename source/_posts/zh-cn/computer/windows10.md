---
title: windows10
category: computer
date: 2020-02-08 16:42:49
tags:
---

记录 Windows10 的设置改动

<!-- more -->

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