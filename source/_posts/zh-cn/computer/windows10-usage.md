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

## Hyper-V Manager Troubleshoot

1. 首先客户机和宿主机都要启用 WinRM
   ```powershell
   PS C:\Users\ndoskrnl> Enable-PSRemoting -Force
   WinRM has been updated to receive requests.
   WinRM service type changed successfully.
   WinRM service started.
   
   Set-WSManQuickConfig : <f:WSManFault xmlns:f="http://schemas.microsoft.com/wbem/wsman/1/wsmanfault" Code="2150859113"
   Machine="localhost"><f:Message><f:ProviderFault provider="Config provider"
   path="%systemroot%\system32\WsmSvc.dll"><f:WSManFault xmlns:f="http://schemas.microsoft.com/wbem/wsman/1/wsmanfault"
   Code="2150859113" Machine="DESKTOP-1KNPPCL"><f:Message>WinRM firewall exception will not work since one of the network
   connection types on this machine is set to Public. Change the network connection type to either Domain or Private and
   try again. </f:Message></f:WSManFault></f:ProviderFault></f:Message></f:WSManFault>
   At line:116 char:17
   +                 Set-WSManQuickConfig -force
   +                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~
       + CategoryInfo          : InvalidOperation: (:) [Set-WSManQuickConfig], InvalidOperationException
       + FullyQualifiedErrorId : WsManError,Microsoft.WSMan.Management.SetWSManQuickConfigCommand
   ```
   遇到错误, 用 Set-NetConnectionProfile 将所有网络设置为 Private:
   ```powershell
   PS C:\Users\ndoskrnl> Get-NetConnectionProfile
   Name             : Network 5
   InterfaceAlias   : ZeroTier One [83048a06326c5a60]
   InterfaceIndex   : 9
   NetworkCategory  : Private
   IPv4Connectivity : LocalNetwork
   IPv6Connectivity : LocalNetwork
   
   Name             : Xiaomi_XXXX_5G
   InterfaceAlias   : vEthernet (External Network With Adapter)
   InterfaceIndex   : 16
   NetworkCategory  : Private
   IPv4Connectivity : Internet
   IPv6Connectivity : Internet
   PS C:\Users\ndoskrnl> Set-NetConnectionProfile -InterfaceIndex 9 -NetworkCategory Private
   ```
   有时候即使设置了还是会报错, 直接上参数 `-SkipNetworkProfileCheck` 吧:
   ```powershell
   PS C:\Users\ndoskrnl> Enable-PSRemoting -SkipNetworkProfileCheck -Force
   ```
2. 然后再在客户端设置 TrustedHosts
   ```powershell
   Set-Item WSMan:localhost\client\trustedhosts -value "DESKTOP-XXXXXXX.lan" -Force
   ```
   查看一下确保无错:
   ```powershell
   Get-Item WSMan:localhost\client\trustedhosts
   ```
   多个主机名可用逗号分隔
   ```powershell
   $curValue = (Get-Item wsman:\localhost\Client\TrustedHosts).value
   Set-Item wsman:\localhost\Client\TrustedHosts -Value `
   "$curValue, Server01.Domain01.Fabrikam.com"
   ```