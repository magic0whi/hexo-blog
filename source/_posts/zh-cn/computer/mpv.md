---
title: mpv
category: computer
date: 2020-02-29 18:57:17
tags:
toc: true
---

MPV的配置
针对上网本注释了了一些占用高的选项
TODO: [可能的一份更好的配置](https://github.com/Argon-/mpv-config/blob/master/mpv.conf)

<!-- more -->

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