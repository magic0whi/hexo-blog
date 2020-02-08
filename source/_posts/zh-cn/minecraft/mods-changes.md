---
title: mods-changes
category: minecraft
date: 2020-02-08 16:43:28
tags:
---

我的世界Mod配置文件的改动记录

也包括Sponge的配置

<!-- more -->

## 关闭MOD更新检查

1. 应用能源2: config/AppliedEnergistics2/VersionChecker.cfg
2. 砍树 config/bspkrscore.cfg
3. 史诗攻城config/epicsiegemod.cfg

## 应用能源2

1. (在其他id世界也生成陨石) config/AppliedEnergistics2.cfg:

```cfg config/AppliedEnergistics2.cfg:
worldgen {
    ...
    I:  <
        0
        3 // 一般都是这个, 从level.dat可以看nbt数据
     >
```

## 工业2

1. (将高炉炼钢时间缩短为一分钟) config/ic2/blast_furnace.ini :

```ini config/ic2/blast_furnace.ini
; Iron Ingot
minecraft:iron_ingot = ic2:ingot#steel ic2:misc_resource#slag @fluid:1 @duration:600
; Crushed Iron Ore
OreDict:crushedIron = ic2:ingot#steel ic2:misc_resource#slag @fluid:1 @duration:600
; Iron Ore
minecraft:iron_ore = ic2:ingot#steel ic2:misc_resource#slag @fluid:1 @duration:600
; Purified Crushed Iron Ore
OreDict:crushedPurifiedIron = ic2:ingot#steel ic2:misc_resource#slag @fluid:1 @duration:600
; Iron Dust
OreDict:dustIron = ic2:ingot#steel ic2:misc_resource#slag @fluid:1 @duration:600
```

2. config/IC2.ini :

```ini
; 移动储电箱无电量损耗
energyRetainedInStorageBlockDrops = 1
; 通过传送器时，玩家的背包重量不会增加能量消耗
teleporterUseInventoryWeight = false
; (未应用)修改矿物生成让工业2的矿脉更大更集中, 缓解烦人的挖矿
[worldgen / copper]
count = 7
size = 20
[worldgen / lead]
count = 4
size = 8
[worldgen / tin]
count = 12
size = 12
[worldgen / uranium]
count = 10
size = 6
```

3. 配合AE2让打粉机可以打石英粉 config/ic2/macerator.ini:

```ini
; certus quartz dust
OreDict:crystalCertusQuartz = appliedenergistics2:material@2

; nether quartz dust
OreDict:gemQuartz = appliedenergistics2:material@3

; fluix dust
OreDict:crystalFluix = appliedenergistics2:material@8
```

## SCP: Lockdown

1. (关闭和地形格格不入的废弃设施生成, 设成很大的数即可) config/Secure. Contain. Protect. v2.1.cfg:

```cfg
I:"Abandoned facility rarity"=2147483647
```

## (已移除)Tiquality
1. 让mod增加的流体能正常流动 config/Tiquality.cfg :

```ini
(在后面追加)
S:NATURAL_BLOCKS <
            ic2:distilled_water
            ic2:uu_matter
            biomesoplenty:sand
            buildcraftenergy:fluid_block_oil_heat_0
            buildcraftenergy:fluid_block_fuel_gaseous_heat_1
         >
```


## EpicBanItem

1. (检查当工业2的采矿镭射枪nbt数据中充能大于0时触发ban物品) config/epicbanitem/banitem.conf :

```ini
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



## (已弃用)Flexiblelogin:

1. (汉化) config/flexiblelogin/locale.conf


## Nucleus:

1. 世界需要玩家有"nucleus.worlds.<worldname>"权限才能进入 config/nucleus/main.conf :
```ini
separate-permissions=true
```

2. fjk(first join kit) 新手礼包 nucleus/kits.json

```txt
具体见nucleus fjk配置
```


## (已移除)环境污染
1.adpother.cfg关闭污染源

```cfg
# 生物死亡排碳
B:AnimalDeath=false
# 生物喂食排碳
B:AnimalFeeding=false
# 火焰排放
B:Fire=false
# 玩家死亡排放
B:PlayerDeath=false
# 玩家吃食排放
B:PlayerEating=false
# 原版熔炉排放
B:VanillaFurnace=false
```

2.torch.cfg关闭火把排放:

```cfg
# 零排碳
S:carbon=0.0
# 零排硫
S:sulfur=0.0
```


## SpongeForge
1.禁用异步光照(因为Phosphor): 
```
        async-lighting {
            # If 'true', lighting updates are run asynchronously.
            enabled=false
        }
```

2. 使用新的红石算法
```
        eigen-redstone {
            enabled=true
        }
```
3. 取消碰撞箱限制防止冰与火之歌的5级龙被清理
```
        # Maximum size of an entity's bounding box before removing it. Set to 0 to disable
        max-bounding-box-size=0
```
4. 最大视距为8 :
```
view-distance=8
```

## (已移除) MyCrayfish's Gun mod:
1. (客户端配置文件) 解决右键冲突 config/cgm.cfg

```
        controls {
            # If true, uses the old controls in order to aim and shoot
            B:"Use Old Controls"=true
        }
```


## BuildCraft

1. (未应用)与RTG沙漠海洋生态环境的兼容问题(禁用沙漠海洋油田生态) config/buildcraft/main.cfg:

```
        B:oil_desert_biome=true
        B:oil_ocean_biome=true
```

2. 允许石油生成维度为白名单模式 config/buildcraft/main.cfg:

```
B:excludedDimensionsIsBlacklist=true
I:excludedDimensions <
    0
    3
 >
```

## BiomesOPlenty

关闭主界面全景图替换 config/biomesoplenty/misc.cfg:

```
B:"Enable Biomes O' Plenty Main Menu Panorama"=false
```

## SereneSeasons

季节是白名单only, 给第二个世界也添加季节影响 config/sereneseasons/seasons.cfg:

S:"Whitelisted Dimensions" <
    0
    3
 >

## Cuisine

config/cuisine.cfg

添加竹子, 庄稼, 农作物的维度生成白名单

```
    I:BamboosGenDimensions <
        0
        3
     >

    I:CropsGenDimensions <
        0
        3
     >

    I:FruitTreesGenDimensions <
        0
        3
     >
```

## Epic Siege

苦力怕不隔墙爆

B:Breaching=false

怪物不隔墙感知

I:"Xray Mobs"=0

反转黑名单, 可破坏方块为白名单模式

B:"Invert Digging Blacklist"=true

蜘蛛只有1%的概率吐丝

I:"Webbing Chance"=1

可破坏方块白名单

```
    S:"Digging Blacklist" <
        minecraft:crafting_table
        minecraft:furnace
        minecraft:wool
        minecraft:snow_layer
        minecraft:snow
        minecraft:ice
        minecraft:netherrack
        minecraft:soul_sand
        minecraft:grass
        minecraft:grass_path
        minecraft:farmland
        minecraft:dirt
        minecraft:stone
        minecraft:cobblestone
        minecraft:sand
        minecraft:sandstone
        minecraft:planks
        minecraft:log
        minecraft:trapdoor
        minecraft:wooden_door
        minecraft:spruce_door
        minecraft:birch_door
        minecraft:jungle_door
        minecraft:acacia_door
        minecraft:dark_oak_door
        minecraft:fence
        minecraft:nether_brick_fence
        minecraft:spruce_fence
        minecraft:birch_fence
        minecraft:jungle_fence
        minecraft:dark_oak_fence
        minecraft:acacia_fence
        minecraft:fence_gate
        minecraft:spruce_fence_gate
        minecraft:birch_fence_gate
        minecraft:jungle_fence_gate
        minecraft:dark_oak_fence_gate
        minecraft:acacia_fence_gate
        minecraft:glass
        minecraft:stained_glass
        minecraft:glass_pane
        minecraft:stained_glass_pane
     >
```

## 砍树TreeCapitator

config/treecapitator:
```
        ic2_rubber_tree {
            S:logs=IC2:rubber_wood
            S:leaves=IC2:leaves
        }
```

## 林业

设置难度(他说不设置服务器可能会有影响)

```
difficulty {
    S:game.mode=NORMAL
```

(未应用)关闭背包补给, 提高服务器性能

```
performance {
    B:backpacks.resupply=true
}
```

关闭铜/锡的生成, 以IC2的取代

```
B:copper=false
B:tin=false
```

## 服务器主配置文件server.properties:
```
#防止服务器卡顿触发自动关闭
max-tick-time=-1

# RTG世界类型
level-type=RTG

# 压缩封包阈值
network-compression-threshold=512

# 服务器端口
server-port=23232

# 允许飞行
allow-flight=true

# 离线玩家不可加入
online-mode=true

# 世界生成种子
level-seed=981506332

# 服务器motd
motd=\u897F\u4EAC\u6469\u767B - The modern of Shikyo
```


## 以下是需要保护的配置文件&数据

config/griefprevention/GlobalPlayerData/*
config/griefprevention/worlds/minecraft/overworld/shikyo/ClaimData/*
config/flexiblelogin/database.mv.db
luckperms/luckperms-h2.mv.db