---
title: altiumdesigner-notes
toc: true
category: computer
date: 2021-11-02 21:52:02
tags: electronic-and-information-engineering
---

进度 Pass 1/2, Stage 11/38

<!-- more -->

原件在放置时按 `Tab` 键可暂停(暂停后调整属性), 按 `Enter` 键继续

设置格点<ins>V</ins>iew-><ins>G</ins>rids->Set <ins>S</ins>nap Grid... (快捷键`VGS`)

在Name属性中, 字符后面加上 `\` 可给字符加上Overline, 如 `V\I\N` 会显示为 <span style="text-decoration:overline">VIN</span>

自定义快捷键: 按住 `Ctrl` 点击想要修改快捷键的命令

学到の里技:
* 在原理图中添加大量管脚时可使用阵列式粘贴 (With `.SchLib` file focused in Editor, <u>E</u>dit->Paste Arra<u>y</u>...), 其中主增量 (Primary Increment) 对应管脚号 (Designator), 次增量 (Secondary Increment) 对应管脚名称 (Name), 记得先 `Ctrl+C` 复制一个管脚到剪切板.
* 可以从原理图 (.SchDoc) 反向生成原理图库 (.SchLib): (With `.SchDoc` file focused in Editor) <u>D</u>esign-><u>M</u>ake Schematic Library
  弹出的 Component Grouping 可以指定特定参数相同的归类到一起.
* 原理图 (.SchDoc) 双击边缘可以快速弹出 Properties 菜单.

快捷键:
* `A` 调出对齐菜单 (<u>E</u>dit->Ali<u>g</u>n)
* `M` 调出移动菜单 (<u>E</u>dit-><u>M</u>ove)
  `MS` 移动选择的元件 (->Move <u>S</u>election)
`Shift+Drag` 快速复制元件
  在移动过程中, 按 `X`、`Y` 可按对应的轴镜像.
* `Ctrl+W` 放置导线 (<u>P</u>lace-><u>W</u>ire).
  退格键可撤回确定的点, 导线和绘制线都可用.

1. 原理图绘制步骤:
   1. 先不连线, 把各个模块对应的元件大致摆放好.
   2. 通过绘制工具中的线 (<u>P</u>lace-><u>D</u>rawing Tools-><u>L</u>ine) 跨分模块区域.
      绘制时, `Shift+Space` 可以切换正交模式、钝角模式、任意角模式.