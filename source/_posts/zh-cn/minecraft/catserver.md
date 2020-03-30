---
title: catserver
category: minecraft
date: 2020-02-08 16:43:15
tags:
---

Catserver & Spigot 系Mod服务器配置

不含mod的配置改动(除非有MOD遇到Catserver水土不服)

<!-- more -->

## 启动参数

```bash catserver/start.sh
java -javaagent:authlib-injector-1.1.26-41a7a47.jar=https://littleskin.cn/api/yggdrasil -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1NewSizePercent=20 -XX:G1ReservePercent=20 -XX:MaxGCPauseMillis=50 -XX:G1HeapRegionSize=16M -XX:-UseAdaptiveSizePolicy -XX:-OmitStackTraceInFastThrow -Xmn128m -Xmx2048m -jar CatServer-a9501ab-async.jar nogui --tweakClass net.minecraftforge.fml.common.launcher.FMLServerTweaker
```

## 关闭数据分析&检查更新

1. plugins/bStats/config.yml: `enabled: false`
2. plugins/CoreProtect/config.yml: `check-updates: false`
3. plugins/PluginMetrics/config.yml: `opt-out: true`
4. plugins/Vault/config.yml: `update-check: false`

## Spigot配置

1. server.properties配置

   ```conf catserver/server.properties
   # 网络封包压缩阈值
   network-compression-threshold=256

   # 地图类型, 这里使用RTG地形
   level-type=RTG
   
   # 最大玩家
   max-players=64
   
   # 关闭watchdog
   max-tick-time=-1
   
   # 允许飞行
   allow-flight=true
   
   # 正版验证
   online-mode=true
   
   # 地图种子
   level-seed=981506332
   
   # 视距
   view-distance=8
   
   # bukkit系似乎不需要转码
   motd=Mojangの奇妙冒险
   ```

2. bukkit.yml配置

   ```yml catserver/bukkit.yml
   # 可适当降低怪物数量的上限, 此为默认值
   spawn-limits:
     monsters: 35
     animals: 10
     water-animals: 5
     ambient: 10
   ticks-per:
     monster-spawns: 3
   ```

3. spigot.yml

   ```yml catserver/spigot.yml
   settings:
     ...
     moved-wrongly-threshold: 0.0625
     moved-too-quickly-multiplier: 200.0 // 减少出现 xxx moved too quickly! 的几率
   world-settings:
     default:
       verbose: false // 关闭启动时打印世界配置信息
       hunger:
         jump-walk-exhaustion: 0.025
         jump-sprint-exhaustion: 0.1
         combat-exhaustion: 0.05
         regen-exhaustion: 3.0
         swim-multiplier: 0.005
         sprint-multiplier: 0.05
         other-multiplier: 0.0
       ...
       mob-spawn-range: 3 // 怪物生成范围
       ...
       entity-activation-range:
         animals: 16
         monsters: 24
         misc: 8
       ...
       merge-radius: // 更大合并掉落物范围
         item: 4.0
         exp: 6.0
       ...
       view-distance: 8 // 视距
       ...

   ```

   给我的提醒: 诸如seed-village之类的不需要管它, 默认值就是根据世界种子而配置

## Dynmap

由于使用第三方littleskin提供的服务, 需要配置configuration.txt

```yml catserver/dynmap/configuration.txt
# Control loading of player faces (if set to false, skins are never fetched)
fetchskins: true

# Customize URL used for fetching player skins (%player% is macro for name)
skin-url: "https://littleskin.cn/skin/%player%.png"
```

注意事项:

1. `Dynmap-forge` 不支持 `Luckperms-spigot`, 只能`op <player>`
2. `Dynmap-forge` 无法和 `WorldBorder-spigot` 插件联动
3. 已知 版本`3.0-beta-10-forge` 不能用, **请使用3.0-beta-9-forge**
4. `DynmapBlockScan` 和 **Dynmap-_spigot_**: 不能, 会崩.
5. `DynmapBlockScan-Universal-Renderer` 此版本调用了客户端Side的类, 无法在服务器上运行

DynmapBlockScan可以自动添加一些Mod方块的支持(然而截止1-31-2020 IC2的机器依旧不渲染)

有用的链接:

[DynmapBlockScan (Universal Renderer)](https://github.com/LolHens/DynmapBlockScan/tree/universal-renderer)

## WorldBorder

世界边界设置

```yml config/WorldBorder/config.yml
worlds:
  world:
    x: -263.5
    z: 250.5
    radiusX: 10000
    radiusZ: 10000
    wrapping: false
```
## LuckPerms 权限记录

这里记录我添加了哪些权限, 用于哪些命令

### default 组

### admin 组

## Mod兼容改动

治疗水土不服

### RandomPatches

```conf catserver/config/randompatches.cfg
  B:portalBucketReplacementFix=false
  B:patchNetHandlerPlayServer=true
```

## 清理配置残留

很多插件都会产生毫无意义的残余配置项, 需要时清理有助于治疗强迫症

### 多世界

#### Dynmap清理

清理 `dynmap/forgeworlds.yml` 中的相关条目, 然后 `/dynmap reload`
