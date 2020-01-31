---
title: catserver
date: 2020-01-31 06:04:08
tags:
---
# Catserver & Spigot 系Mod服务器配置

## Spigot.yml

1. 关闭启动时打印世界配置信息: world-settings -> default-> verbose: true

2. 给我的提醒: 诸如seed-village之类的不需要管它, 默认值就是根据世界种子而配置

## WorldBorder

使用原版的世界边界`/worldborder` 是全局性的, **无法** 针对单个世界设置不同的边界大小

由于Catserver 目前为1.12.2, 实测1.13的WorldBorder插件无法正常添加边界.
遂使用旧版v1.8.7, [链接](https://dev.bukkit.org/projects/worldborder)

插件命令为`/wb`, 具体见[Full list of Commands and Permissions](https://www.spigotmc.org/threads/worldborder.339635/#post-3162179)

常用命令:

1. `/wb dynmap on` - 开启Dynmap上的地图边界显示

2. `/wb [worldname] set <radiusX> [radiusZ] <x> <z>` - set border with the x and z coordinates of the center specified.
3. `/wb [worldname] radius <radiusX> [radiusZ]` - change border radius for this world. The world needs to have already had a border set, since the x and z values are not modified. If radiusZ isn't specified, the radiusX value will be used for both. A "+" or "-" can be added to the start of radiusX or radiusZ to have the current radius increased or decreased a specified amount, for example "+100" would increase the existing border radius by 100 blocks.
4. `/wb wshape [worldname] <elliptic|round|rectangular|square|default>` - Override the shape for this world only. The world needs to have already had a border set. The default shape used by other worlds (set via /wb shape) will not be changed by this. Note that "elliptic" and "round" are interchangeable here, as are "rectangular" and "square".

## Dynmap

由于使用第三方littleskin提供的服务, 需要配置configuration.txt

```
# Control loading of player faces (if set to false, skins are never fetched)
fetchskins: true

# Customize URL used for fetching player skins (%player% is macro for name)
skin-url: "https://littleskin.cn/skin/%player%.png"
```

注意事项:

1. DynmapBlockScan只能和Mod版的Dynmap搭配, 不然会崩.
2. 由于使用Mod版的Dynmap所以不支持LuckPerms, 只能`op <player>`, 然后进行操作.
3. 已知3.0-beta-10-forge搭配DynmapBlockScan会崩, **请使用3.0-beta-9-forge**
4. 一些有用的链接: [Dynmap Builds](http://mikeprimm.com/dynmap/builds/dynmap/), [Universal Renderer for mods](https://github.com/LolHens/DynmapBlockScan/tree/universal-renderer)
但是Universal Renderer版的DynmapBlockScan调用了客户端Side的类, 无法在服务器上运行
5. 似乎使用Mod版的Dynmap无法和WorldBorder插件互动, 只能使用插件版了
DynmapBlockScan可以自动添加一些Mod方块的支持(然而截止1-31-2020 IC2的机器依旧不渲染)

## ClearLag (已弃用)

由于CatServer目前表现良好, 服务器日常空载, 所以不需要定期清理实体和掉落物

## GriefPrevention

目前使用替代方案GriefDefender

## LuckPerms

这里记录我添加了哪些权限, 用于哪些命令

### default 组

### admin 组
