---
title: windows10-usage
category: computer
date: 2020-02-09 09:44:22
tags:
toc: true
---

记录一些windows的使用技巧

<!-- more -->

## 查看哪些进程正在使用我的摄像头

前往微软文档下载[Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)

注意要以**管理器权限运行procexp64.exe**

具体使用方法:

1. 首先在设备管理器找到摄像头的"物理设备对象名称", 右键复制

   {% asset_img 1.png %}

2. 打开 Process Explorer 然后 Ctrl + F, 查询句柄. (这里我打开了mumu模拟器的相机应用)

   {% asset_img 2.png %}

注意事项:

    1. 如果只有svchost.exe而查不到其他使用该句柄的程序, 则一般是某个uwp应用在使用该设备
    2. 也可以查看其他设备(如麦克风)

## 启用 Compact 压缩

```powershell
compact /compactos:always # 压缩所有系统文件
compact /compactos:query # 查询系统的压缩状态。
compact /compactos:never # 取消所有系统文件的压缩
```

## 清理组件存储(WinSxS )

```powershell
dism.exe /Online /Cleanup-Image /AnalyzeComponentStore # 查看组件存储大小
dism.exe /online /Cleanup-Image /StartComponentCleanup # 执行组件存储清理
```

## Intel ME Firmware

1. 查看 ME 信息
   ```powershell
   > MEInfoWin.exe
   ```
2. 备份 ME 固件
   ```powershell
   FWUpdLcl.exe -SAVE xxxx.bin
   ```
3. 刷写 ME 固件
   ```powershell
   FWUpdLcl64.exe -f ..\ME8_1.5M_Production.bin
   ```