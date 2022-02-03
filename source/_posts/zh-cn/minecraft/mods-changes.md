---
title: mods-changes
category: minecraft
date: 2020-02-08 16:43:28
tags:
toc: true
---

Legacy MOD changes
Will be merged when the first release IC2 1.18 was published.

<!-- more -->

## 工业2 (已移除)

1. 主配置文件改动

   ```ini config/IC2.ini
   [balance]
   ; 移动储电箱无电量损耗
   energyRetainedInStorageBlockDrops = 1
   ; 通过传送器时，玩家的背包重量不会增加能量消耗
   teleporterUseInventoryWeight = false
   ; 关闭扳手拆机器log
   wrenchLogging = false
   ```

2. 让打粉机可以打AE2的东西

   ```ini config/ic2/macerator.ini
   ; certus quartz dust
   OreDict:crystalCertusQuartz = appliedenergistics2:material@2

   ; nether quartz dust
   OreDict:gemQuartz = appliedenergistics2:material@3

   ; fluix dust
   OreDict:crystalFluix = appliedenergistics2:material@8

   ; sky stone dust
   appliedenergistics2:sky_stone_block = appliedenergistics2:material@45

   ; ender dust
   minecraft:ender_pearl = appliedenergistics2:material@46
   ```

3. 将高炉炼钢时间缩短为一分钟

   ```ini config/ic2/blast_furnace.ini
   ; Iron Ingot
   minecraft:iron_ingot = ic2:ingot#steel ic2:misc_resource#slag    @fluid:1 @duration:600
   ; Crushed Iron Ore
   OreDict:crushedIron = ic2:ingot#steel ic2:misc_resource#slag    @fluid:1 @duration:600
   ; Iron Ore
   minecraft:iron_ore = ic2:ingot#steel ic2:misc_resource#slag    @fluid:1 @duration:600
   ; Purified Crushed Iron Ore
   OreDict:crushedPurifiedIron = ic2:ingot#steel    ic2:misc_resource#slag @fluid:1 @duration:600
   ; Iron Dust
   OreDict:dustIron = ic2:ingot#steel ic2:misc_resource#slag @fluid:1    @duration:600
   ```

## 砍树

与工业2橡胶树的兼容

```conf config/treecapitator.cfg
        ic2_rubber_tree {
            S:logs=IC2:rubber_wood
            S:leaves=IC2:leaves
        }
```

## EpicBanItem

1. (检查当工业2的采矿镭射枪nbt数据, 充能大于0时触发ban物品)

   ```conf config/epicbanitem/banitem.conf
       "ic2:mining_laser"=[
           {
               name=ban-mining-laser-in-ic2
               priority=5
               query {
                   id="ic2:mining_laser"
                   "tag.charge" {
                       "$gt"="0.0d"
                   }
               }
               update {
                   "$set" {
                       Damage=0
                       id="minecraft:air"
                   }
               }
           }
       ]
   ```