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

## linux

1. sysctl
   ```properties /etc/sysctl.d/99-sysctl.conf
   # BBR TCP Congestion
   net.core.default_qdisc = cake
   net.ipv4.tcp_congestion_control = bbr
   
   # Kernel Panic auto reboot after 30 minutes
   kernel.panic = 3780
   ```

## Windows 10/11

1. 禁用遥感
   组策略:
   ```
   Administrative Templates
    -> Windows Components
     -> Data Collection and Preview Builds
      -> Allow Telemetry -> Enabled, Options: 0 - Security [Enterprise Only]
   ```
   计划任务:
   ```
   Task Scheduler Library
    -> Microsoft
     -> Windows
      -> Application Experience
       -> Microsoft Compatibility Appraiser -> Disable
   ```
2. 禁用 Connected User Experiences and Telemetry
   ```powershell
   Set-Service DiagTrack -StartupType Disabled
   ```
3. 启用UTC
   ```batch
   reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f
   ```
   [参考 Archlinux Wiki](https://wiki.archlinux.org/index.php/System_time#UTC_in_Windows)
4. 禁用英特尔CPU幽灵/熔断/僵尸负载漏洞补丁
   ```batch
   reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /v FeatureSettingsOverride /t REG_DWORD /d 3 /f
   reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /v FeatureSettingsOverrideMask /t REG_DWORD /d 3 /f
   ```
   [参考 Microsoft Docs](https://support.microsoft.com/en-us/help/4073119/protect-against-speculative-execution-side-channel-vulnerabilities-in)
5. 任务栏时间显示秒钟
   ```batch
   reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowSecondsInSystemClock /t REG_DWORD /d 1 /f
   ```
6. 默认使用半角标点符号(全角/半角切换: Ctrl+.)
   ```
   Time & Language
     -> Language
       -> Chinese (Simplified, China)
         -> Microsoft Pinyin
           -> Options
             -> General
               -> Use English punctuations when in Chinese input mode: On

   ```
7. Scoop installed git bash: "Open Git Bash" here context menu
   * {% asset_link git-install-context.reg %}
   * {% asset_link git-uninstall-context.reg %}
8. Windows 11 full right-click menu
   enable: `reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve`
   disable: `reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`
   > Don't forget to reboot.
   reference: https://gist.github.com/nebula-moe/75c49c261d1fc9e780df8ed9d0baed97

## MPV

1. 脚本 `portable_config\scripts\auto-profiles.lua`
2. shaders文件 `portable_config\shaders\`, 请自行取消我的注释以启用shaders
   - `KrigBilateral.glsl`
   - `nnedi3-nns32-win8x4.hook`
   - `nnedi3-nns64-win8x4.hook`
   - `SSimDownscaler.glsl`
   - `SSimSuperRes.glsl`

## 升级Openwrt

记录小米路由器3G 的 Openwrt 配置

1. 使用 sysupgrade 方式升级
   ```console
   # cd /tmp
   # wget https://downloads.openwrt.org/snapshots/targets/ramips/mt7621/openwrt-ramips-mt7621-mir3g-squashfs-sysupgrade.tar
   # sysupgrade /tmp/openwrt-ramips-mt7621-mir3g-squashfs-sysupgrade.tar
   ```
2. 升级完后最好更新以下新版本系统的软件包
   ```console
   # opkg update
   # opkg list-upgradable
   # opkg list-upgradable | sed -e "s/\s.*//" | while read PKG_NAME; do opkg upgrade "${PKG_NAME}"; done
   ```
3. 安装的软件包
   ```
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

## Hexo

1. Config:
   ```yml hexo/_config.yml
   # 标签页标题
   title: Ndoskrnl's blog
   description: 'There is nothing to say now'
   author: Ndoskrnl
   # 多语言支持
   language:
   - zh-cn
   - en
   # 时区
   timezone: 'Asia/Shanghai'
   
   # Url
   url: http://blog.ndoskrnl.net
   # 文章永久链接(目录)
   permalink: :lang/:category/:year/:title/
   # 永久链接变量默认值配置
   permalink_defaults:
     lang: en
   
   # 新文章文件目录
   new_post_name: :lang/:category/:title.md
   
   # 资源文件夹
   post_asset_folder: true
   
   # 主题
   theme: icarus
   
   # 解决半角符号渲染成全角的问题
   marked:
     smartypants: false
   
   # 部署到 Github Pages
   deploy:
     type: git
     repo: git@github.com:ndoskrnl/ndoskrnl.github.io.git
     branch: master
   ```
2. Icarus 主题设置
   主题设置由于改动较多且时效性不高, 这里不作记录
   具体请见主题配置文件 `theme/icarus/_config.yml`
   将主页样式变为归档页形式: 直接将`theme/icarus/layout/index.jsx` 替换成 `theme/icarus/layout/archive.jsx` 内的内容
3. Git Hook
   ```bash /home/git/blog.git/hooks/post-update
   #!/bin/bash
   # 首先删除旧文件
   # 启用extglob才能用!(xx)之类的语法
   shopt -s extglob
   rm -rf /srv/git/www/!(node_modules)
   
   # git checkout
   git --work-tree=/srv/git/www --git-dir=/srv/git/blog.git checkout -f
   
   # 进入网页文件夹目录
   cd /srv/git/www
   
   # 若node_modules文件夹不存在, 安装依赖插件
   if [ ! -d /srv/git/www/node_modules ]; then
       npm install
   fi
   
   # 若package.json有改动, 更新插件
   CHANGED=$(git --git-dir=/srv/git/blog.git diff HEAD^! --stat -- package.json | wc -l)
   if [ $CHANGED -gt 0 ]; then
       rm -rf /srv/git/www/node_modules
       npm install
   fi
   
   # 生成静态网页
   hexo g
   ```
3. 额外安装的插件
   ```
   hexo-symbols-count-time # 字数统计
   hexo-filter-plantuml # UML Diagram for hexo
   hexo-deployer-git # Git 部署 (Github Pages)
   ```
