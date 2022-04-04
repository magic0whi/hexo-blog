---
title: Altiumdesigner Notes
toc: true
category: computer
date: 2021-11-02 21:52:02
tags: electronic-and-information-engineering
---

进度 Pass 1/2, Stage 16/38, 10:41

<!-- more -->

自定义快捷键: 按住 `Ctrl` 点击想要修改快捷键的命令

### 原理图库创建

原件在放置时按 `Tab` 键可暂停 (暂停后调整属性), 按 `Enter` 键继续

设置格点<u>V</u>iew-><u>G</u>rids->Set <u>S</u>nap Grid... (快捷键`VGS`)

在Name属性中, 字符后面加上 `\` 可给字符加上 Overline, 如 `V\I\N` 会显示为 <span style="text-decoration:overline">VIN</span>

### 原理图绘制步骤

1. 先不连线, 把各个模块对应的元件大致摆放好.
2. 通过绘制工具中的线 (<u>P</u>lace-><u>D</u>rawing Tools-><u>L</u>ine) 跨分模块区域.
   绘制时, `Shift+Space` 可以切换正交模式、钝角模式、任意角模式.
3. 连好导线, 在一些管脚添加需要的 NetLabel
4. 核对元件 Value 值 (如元件位号 R1、R2, 电阻的阻值等)
   > 可通过位号编辑器自动编号编号, 它的位置在 <u>T</u>ools-><u>A</u>nnotation-><u>A</u>nnotate Schematics...
   > 修改了参数后, 已经套用的需要重置 (如点击 Reset All), 然后再应用 (点击 Update Changes List)
   > 最后点击接受变更 (Accept Changes (Create ECO)), 再在弹出的窗口点击执行变更 (Execute Changes) 即可
5. 给元件添加封装 (Footprint), 即 PCB 上元件的 3D 模型.
   由于一个一个在元件的属性里添加封装太麻烦且容易出错, 建议使用封装管理器 (<u>T</u>ools->Footprint Mana<u>g</u>er...)
6. 原理图的编译设置及检查.
   在工程选项 (Proje<u>c</u>t->Project <u>O</u>ptions...) 可配置错误报告 (Error Reporting) 的类型.
   常用设置:
   ```plaintext
   Violations Associated with Components:
     Duplicate Part Designators = Fatal Error // 相同的元件位号
   Violations Associated with Nets:
     Floating net labels = Fatal Error // 网络标号悬空 (没有连接元件)
     Floating power objects = Fatal Error // 电源对象悬空
     Nets with only one pin = Fatal Error // 单端网络
   ```
   > 在原理图中, 可以设置网络颜色 (<u>V</u>iew->Set Net Colors), 以便于查找单端网络.
   > 对于没有网络标号的引脚, 如果连接了悬空的导线, 可以加上通用 No ERC 标号 (<u>P</u>lace->Directi<u>v</u>es->Generic <u>N</u>o ERC) 来禁用这里的 ERC 检查; 也可以直接删除连接的悬空导线.
7. 常见 CHIP 封装的创建
   > 阻焊的作用是防止绿油覆盖
   > 在 `.PcbDoc` 中按 `Ctrl+D` 可以调出视图配置, 可以切换 3D (快捷键 `3`) 等操作, 在 3D 中, 按住 `Shift+鼠标右键` 可以调整角度.

#### 快捷键

* `A` 调出对齐菜单 (<u>E</u>dit->Ali<u>g</u>n)
* `M` 调出移动菜单 (<u>E</u>dit-><u>M</u>ove)
  `MS` 移动选择的元件 (->Move <u>S</u>election)
  `Shift+鼠标拖动` 快速复制元件
  在移动过程中, 按 `X`、`Y` 可按对应的轴镜像.
* `Ctrl+W` 放置导线 (<u>P</u>lace-><u>W</u>ire).
  退格键可撤回放置的点, 导线和绘制线都可用.

  <u>P</u>lace-><u>N</u>et Label 放置网络标签

#### 技巧

* 在原理图中添加大量管脚时可使用阵列式粘贴 (编辑器此时打开 `.SchLib` 文件, <u>E</u>dit->Paste Arra<u>y</u>...), 其中主增量 (Primary Increment) 对应管脚号 (Designator), 次增量 (Secondary Increment) 对应管脚名称 (Name), 记得先 `Ctrl+C` 复制一个管脚到剪切板.
* 可以从原理图 (`.SchDoc`) 反向生成原理图库 (.SchLib): (编辑器此时 `.SchDoc` 文件) <u>D</u>esign-><u>M</u>ake Schematic Library
  在弹出的 Component Grouping 窗口可以指定将特定参数相同的归类到一起.
* 原理图 (`.SchDoc`) 双击边缘可以快速弹出 Properties 菜单.

## 元件知识

1. 零欧姆电阻=毫欧电阻, 一般为 20&Omega;-50&Omega;
   作用: 作保险丝, 功能切换, 跳线, 作高频电感、电容, 作磁珠等