---
title: mohist-usage
category: minecraft
date: 2020-02-08 19:09:15
tags:
---

Mohist & Spigot 系插件使用笔记

<!-- more -->

## Multiverse-Core

### Import an exist world

Usage:

`/mv import {NAME} {ENV} [GENERATOR[:ID]]`

## WorldBorder

使用原版的世界边界`/worldborder` 是全局性的, **无法** 针对单个世界设置不同的边界大小

由于Mohist 目前为1.12.2, 实测1.13的WorldBorder插件无法正常添加边界, 遂使用旧版v1.8.7

插件命令为`/wb`, 具体见[Full list of Commands and Permissions](https://www.spigotmc.org/threads/worldborder.339635/#post-3162179)

常用命令:

1. `/wb dynmap on` - 开启Dynmap上的地图边界显示

2. `/wb [worldname] set <radiusX> [radiusZ] <x> <z>` - set border with the x and z coordinates of the center specified.
3. `/wb [worldname] radius <radiusX> [radiusZ]` - change border radius for this world. The world needs to have already had a border set, since the x and z values are not modified. If radiusZ isn't specified, the radiusX value will be used for both. A "+" or "-" can be added to the start of radiusX or radiusZ to have the current radius increased or decreased a specified amount, for example "+100" would increase the existing border radius by 100 blocks.
4. `/wb wshape [worldname] <elliptic|round|rectangular|square|default>` - Override the shape for this world only. The world needs to have already had a border set. The default shape used by other worlds (set via /wb shape) will not be changed by this. Note that "elliptic" and "round" are interchangeable here, as are "rectangular" and "square".

## GriefDefender

由于学生党暂时买不起, 所以只能从[源码](https://github.com/bloodmc/GriefDefender)编译了

遇到的坑:

1. 无法找到 `org/spigotmc/spigot/1.15.2-R0.1-SNAPSHOT/spigot-1.15.2-R0.1-SNAPSHOT.jar` 等相关文件

   网上搜索了一圈, 发现个能提供spigot二进制包的maven repository:

   ```gradle bukkit/build.gradle
   repositories {
       ...
       maven {
           name = 'notfab'
           url = 'https://maven.notfab.net/SpigotMC'
       }
   }
   ```
   注意这个是spigot不是spigot-api

2. CUI需要 `worldedit-spigot`, 但这个版本不支持mod方块

## ClearLag (已弃用)

由于Mohist目前表现良好, 服务器日常空载, 所以不需要定期清理实体和掉落物

## GriefPrevention (已弃用)

目前使用替代方案GriefDefender

## 挂载备份目录

```console
# mount -t cifs //192.168.x.x/public/Game/minecraft/mohist_backup /mnt/mohist_backup -o username=xxxxxxxx,password=xxxxxxxx,workgroup=workgroup,iocharset=utf8,uid=mohist,gid=mohist
```