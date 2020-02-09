---
title: windows10-usage
category: computer
date: 2020-02-09 09:44:22
tags:
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