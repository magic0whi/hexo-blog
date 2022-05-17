---
title: Altiumdesigner Notes
toc: true
category: computer
date: 2021-11-02 21:52:02
tags: electronic-and-information-engineering
---

进度 Pass 1/2, Stage 18/38

<!-- more -->

### 原理图库元件绘制

- `Comment` is the display name of component in Schmetic Document, leave it empty or '`*`' will keep its value same as `Design Item ID`.
- 放置原件时 `<Tab>` 可暂停 (暂停后调整属性), 按 `<CR>` 继续
- 设置格点 <u>V</u>iew&rarr;<u>G</u>rids&rarr;Set <u>S</u>nap Grid...

### 原理图绘制步骤

1. 先不连线, 把各个模块对应的元件大致摆放好.
2. 用"绘制线" (<u>P</u>lace&rarr;<u>D</u>rawing Tools&rarr;<u>L</u>ine) 跨分模块区域.
   绘制时 `<S-Space>` 切换正交模式、钝角模式、任意角模式.
3. 连好导线, 给管脚添加 NetLabel (<u>P</u>lace&rarr;<u>Net</u> Label).
4. 核对元件 Value 值 (如元件位号 R1、R2, 电阻阻值等)
   > 位号编辑器能批量编号: <u>T</u>ools&rarr;<u>A</u>nnotation&rarr;<u>A</u>nnotate Schematics...
   > 修改参数后需要重置来抹除之前更改: Reset All&rarr;Update Changes List
   > 最后点击 Accept Changes (Create ECO)&rarr;Execute Changes
5. 给元件添加封装 (Footprint). (即元件 3D 模型)
   建议使用封装管理器 <u>T</u>ools&rarr;Footprint Mana<u>g</u>er...
6. 原理图的编译设置及检查.
   在Proje<u>c</u>t&rarr;Project <u>O</u>ptions... 配置 Error Reporting 的类型.
   常用设置:
   ```plaintext
   Violations Associated with Components:
     Duplicate Part Designators = Fatal Error // 相同的元件位号
   Violations Associated with Nets:
     Floating net labels = Fatal Error // 网络标号悬空 (没有连接元件)
     Floating power objects = Fatal Error // 电源对象悬空
     Nets with only one pin = Fatal Error // 单端网络
   ```
7. 常见 CHIP 封装的创建
   (`.PcbDoc`) `<C-d>` 调出视图配置, 可切换 3D (`3`). 3D 视图下 `<S-RightMouse>`(Drag) 可调整角度.
   如何相对某个悍盘进行一定, 首先将他们重叠在一起, 然后 `M`&rarr;Move Selection by <u>X</u>, Y...
   `<C-m>` 测量工具可测量两个顶点间的距离. `<S-c>` 可清楚测量标签.
   <u>E</u>dit&rarr;Set Re<u>f</u>erence&rarr;<u>C</u>enter 可以设置原点 (根据目前悍盘位置计算)
   文字和线的丝印要在丝印层 (Top Overlay) 画, 可在别的层画好辅助线, 丝印画好后, 切到辅助线所在层按 `<S-s>` 切换单层显示然后删掉辅助线.
   `<S-e>` 切换抓取模式, 在最底下的信息栏可以看到当前 Snap 模式.
   如果丝印画在悍盘上, 可通过 <u>E</u>dit&rarr;Slice Trac<u>k</u>s 来割断线条然后删除它.
8. 利用 IPC 封装创建向导快速创建封装: (`.PcbLib`) <u>T</u>ools&rarr;<u>I</u>PC Compliant Footprint Wizard...
   输入参数时, 底下勾选 Generate STEP Model Preview 可获得更逼真的模型预览.
   对于 Solder Fillets 的 Board density Level 设置时, 如果板子上的元件比较密集可以设置 Level C - High density (悍盘会更小), 稀疏则可以选择 Level A - Low density, 默认的 B 适用于大多数情况.
   

#### 快捷键

自定义快捷键: 按住 `<C>` 点击想要修改快捷键的命令

- `A` 对齐菜单 (<u>E</u>dit&rarr;Ali<u>g</u>n)
- 移动
  `M` 移动菜单 (<u>E</u>dit&rarr;<u>M</u>ove)
  `MS` 移动已选元件 (&rarr;Move <u>S</u>election)
  在移动过程中, 按 `X`、`Y` 可镜像.
- `<S-MouseLeft>`(Drag) 快速复制元件
- `<C-w>` 放置导线 (<u>P</u>lace&rarr;<u>W</u>ire).
  `<BS>` 可撤回放置的点, 导线和绘制线都可用.

  <u>P</u>lace&rarr;<u>N</u>et Label 放置网络标签
  <u>P</u>lace&rarr;<u>P</u>ort 放置电源端口
- `<C-m>` 测量工具, `<S-c>` 清除测量标签

#### 技巧

- 原理图库
  - 对于元件 Name 属性, `\` 可给字加上上划线, 如 `V\I\N` 会显示为 <span style="text-decoration:overline">VIN</span>
  - 添加大量管脚时可使用阵列式粘贴:
    <u>E</u>dit&rarr;Paste Arra<u>y</u>...), Primary Increment 对应 Designator, Secondary Increment 对应 Name, 记得先复制一个管脚到剪切板.
  - 可从原理图反向生成原理图库: (`.SchDoc`) <u>D</u>esign&rarr;<u>M</u>ake Schematic Library
    弹出的 Component Grouping 可将特定参数相同的归类到一起.
- 原理图
  - 双击原理图边缘可快速弹出 Properties 菜单.
  - 在原理图中, 可设置网络颜色 (<u>V</u>iew&rarr;Set Net Colors), 以便于查找单端网络.
  - 对于没有网络标号的引脚, 如连接了悬空导线, 可加上 Generic No ERC 标号 (<u>P</u>lace&rarr;Directi<u>v</u>es&rarr;Generic <u>N</u>o ERC) 来禁用这里的 ERC 检查; 也可直接删除悬空导线.
  - To update components modified in Schematic Library, use <u>T</u>ools&rarr;Update From <u>L</u>ibraries...
  - Click components/net labels with `<M>` can highlight this component/connetecd net labels.
- PCB封装库
  - 通孔(过孔)悍盘的设置是 Properties&rarr;Layer = Top Layer, 还可在 Properties&rarr;Solder Mask Expansion&rarr;Manual 中勾选 `Tented` 来使阻悍绿油覆盖通孔.
  - 如果无法抓取原点, 按 `<S-e>` 来切换抓取模式(Snapping: All Layers, Current Layer, Off).
  - 添加多个悍盘同样可以使用阵列粘贴:
    <u>E</u>dit&rarr;Paste Arra<u>y</u>...)
  - 同样的, 可从 PCB 图反向生成 PCB 封装库: (`.SchDoc`) <u>D</u>esign&rarr;Make <u>P</u>CB Library


## 元件知识

1. 零欧姆电阻=毫欧电阻, 一般为 20&Omega;-50&Omega;
   作用: 作保险丝, 功能切换, 跳线, 作高频电感、电容, 作磁珠等
2. 阻焊的作用是防止绿油覆盖.
