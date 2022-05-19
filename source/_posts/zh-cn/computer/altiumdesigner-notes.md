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
9. 3D 模型的创建和导入
   默认 3D 模型的创建和导入都放在机械一层 (Mechanical 1).
   导入: <u>P</u>lace&rarr;3D B<u>o</u>dy
   手动绘制: <u>P</u>lace&rarr;Extruded 3D <u>B</u>ody
   默认绘制的是挤出类型 (Properties&rarr;3D Model Type&rarr;Extruded; Overall Height 是挤出高度设置, Standoff Height 悬浮高度)
   
### PCB 网表导入和布线

1. 网表导入及长久报错处理
   (`PcbDoc`) <u>D</u>esign&rarr;<u>I</u>mport Changes From XXX.PrjPcb
   或者 (`SchDoc`) <u>D</u>esign&rarr;<u>U</u>pdate PCB Document XXX.PcbdDoc
   在弹出的 Engineering Change Order 中, 如果只有一张原理图 Add Rooms 可以不勾
   报错处理: 根据错误信息, 分析是没有封装还是管脚缺失, 还是管脚号和封装没对应.
2. 常见绿色报错的消除
   <u>T</u>ools&rarr;<u>D</u>esign Rule Check... 可以修改设计规则检查
   如果遇到提示悍盘直接短路的情况, 检查封装中悍盘的 Jumper (跳线) 参数是否设为 0 (禁用), 否则 AD 会连接 Jumper 参数相同的悍盘并导致短路. 
3. 元器件摆放&板框评估&叠层设置
   器件阵列排步: <u>T</u>&rarr;C<u>o</u>mponent Placement&rarr;Arrange Within Rectang<u>l</u>e.
   在机械一层用线画出矩形的两条边 <u>P</u>lace&rarr;<u>L</u>ine, 然后设置这两条线的长度为整数, 然后完成矩形.
   使用线性尺寸工具标注长度: <u>P</u>lace&rarr;<u>D</u>imension&rarr;<u>L</u>inear.
   然后设置原点 <u>E</u>dit&rarr;<u>O</u>rigin&rarr;<u>S</u>et.
   选中画好的板框 (选中其中一边后可按 `<Tab>` 快速选中相连的线), 然后 <u>D</u>esign&rarr;Board <u>S</u>hape&rarr;<u>D</u>efine Board Shape from Selected Objects

   打开叠层管理器: <u>D</u>esign&rarr;Layer Stac<u>k</u> Manager..., 默认是 2 层, Overlay 后缀表示丝印层, Solder 表示阻悍层. 只有橙色的才是信号层如 Top Layer 和 Bottom Layer.
   要添加新层, 可右键已有层然后 Insert layer {above,below}&rarr;{Signal,Plane,Core,Prepreg}, Signal 信号层, Plane 负片层(默认导通, 线起绝缘作用; 电源层), Core 心板(屏蔽用), Prepreg PP片(基材)

### 快捷键

自定义快捷键: 按住 `<C>` 点击想要修改快捷键的命令

- `j` 搜索菜单
  (`PcbLib`) `JC` 根据元件位号查找元件
- `a` 对齐菜单 (<u>E</u>dit&rarr;Ali<u>g</u>n)
- 移动
  `m` 移动菜单 (<u>E</u>dit&rarr;<u>M</u>ove)
  `ms` 移动已选元件 (&rarr;Move <u>S</u>election)
  在移动过程中, 按 `X`、`Y` 可镜像.
- `<S-MouseLeft>`(Drag) 快速复制元件
- `<C-w>` 放置导线 (<u>P</u>lace&rarr;<u>W</u>ire).
  `<BS>` 可撤回放置的点, 导线和绘制线都可用.

  <u>P</u>lace&rarr;<u>N</u>et Label 放置网络标签
  <u>P</u>lace&rarr;<u>P</u>ort 放置电源端口
- `<C-m>` 测量工具, `<S-c>` 清除测量标签
- `<C-h>` 或 `<Tab>` 连接全选
- `s` 选择菜单
  `sl` 线选
  `si` 框选
- `<S-r>` 改变走线方式: Ignore Obstacles (忽略障碍), Walkaround Obstacles (绕开障碍), Push Obstacles (推挤障碍), HugNPush Obstacles (紧贴并推挤障碍), Stop At First Obstacle (遇到第一个障碍就停止), AutoRoute Current Layer (当前层自动布线), AutoRoute MultiLayer (多层自动布线).
- `<S-s>` 切换单层/多层显示.
- `q` 单位切换mil/mm
- `n` 网络/走线显示和隐藏菜单

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
- PCB 封装库
  - 通孔(过孔)悍盘的设置是 Properties&rarr;Layer = Top Layer, 还可在 Properties&rarr;Solder Mask Expansion&rarr;Manual 中勾选 `Tented` 来使阻悍绿油覆盖通孔.
  - 如果无法抓取原点, 按 `<S-e>` 来切换抓取模式(Snapping: All Layers, Current Layer, Off).
  - 添加多个悍盘同样可以使用阵列粘贴:
    <u>E</u>dit&rarr;Paste Arra<u>y</u>...)
  - 同样的, 可从 PCB 图反向生成 PCB 封装库: (`.SchDoc`) <u>D</u>esign&rarr;Make <u>P</u>CB Library
  - 更改完封装后别忘了在 PCB Library 选中该封装然后右键 Update PCB with XXX
- PCB 布局
  分割显示, 打开配合交叉选择模式(`<S-C-x>` <u>T</u>ools&rarr;Cross Select Mode
  然后框选原理图的元件时对于 PCB 板上的元件也会被选择. 此时就可以配合矩形排列 (`TOL`) 分离模块.
- PCB 布线
  首先需要先把电源跳线隐藏掉, <u>D</u>esign&rarr;<u>C</u>lass... 打开对象类编辑器, 在 Net Classes 下新建一个 "PWR" 类, 然后把电源相关的网络都加进这个类.
  要隐藏这个 "PWR", 首先确保右下角 Panel 里的 PCB 面板启用, 然后可以在 PCB 面板里将 "PWR" 右键设置为隐藏.
  Ro<u>u</u>te&rarr;<u>U</u>n-Route&rarr;<u>C</u>onnection 可以一次性删除整根线(也可以点击某段线然后按 `<Tab>` 选中整根线删掉)
  Place 中的 Line 和 Track 的区别: Line 默认不设置网络, Track 会自动设置走线网络为起点悍盘的网络.
 

## 元件知识

1. 零欧姆电阻=毫欧电阻, 一般为 20&Omega;-50&Omega;
   作用: 作保险丝, 功能切换, 跳线, 作高频电感、电容, 作磁珠等
2. 阻焊的作用是防止绿油覆盖.
3. 固定孔要接地
