---
title: blender
toc: true
category: computer
lang: zh-cn
date: 2021-07-28 18:56:39
tags:
---

开始入门 Blender, 契机是我的米家随手吸尘器的配件-前置滤网的 ABS 框架被我烤变形了,
淘宝搜寻一番后发现卖 50+ 实在太贵, 遂自己建模打印一个.

<!-- more -->

## 常规设置

1. 状态栏 (右下角) 显示内存、显存、点线面数量信息: Edit&rarr;<u>P</u>references...&rarr;Interface&rarr;Editors&rarr;Status Bar&rarr;[&check;]{Scene Statistics, System Memory, Video Memory}.
   <details>
     <summary><strong>&dagger; Example Photos &dagger;</strong></summary>
     {% asset_img 1-1.png %}
   </details>
2. (不推荐) 启用模拟三键鼠标(`<M-LeftMouse>` 旋转视角, 类似 MAYA):
   开启会导致选择循环边快捷键变成双击选择, 建议适应鼠标中键旋转视角 (优势在于一只手就能旋转视角): Edit&rarr;<u>P</u>references...&rarr;Input&rarr;Mouse&rarr;[&check;]Emulate 3 Button Mouse.
3. 启用围绕选择物体旋转和自动深度 (解决视角无法继续推近的问题): Edit&rarr;<u>P</u>references...&rarr;Navigation&rarr;Orbit & Pan&rarr;[&check;]{Orbit Around Selection, Depth}
4. 启用自动加载 Python 脚本 (一些插件需要开启, 不然可能会报错)
   Edit&rarr;<u>P</u>references...&rarr;Save & Load&rarr;[&check;]Auto Run Python Scripts.
5. 启用插件:
   Edit&rarr;<u>P</u>references...&rarr;Add-ons&rarr;
   Node: Node Wrangler (强大的节点工具)
   Rigging: Rigify (自带强大绑骨)
   Import-Export: Import Images as Planes (导入图像为平面)
   Import-Export: Export Autocad DXF Format (.dxf) (导出为 DXF 格式)
   Import-Export: Import AutoCAD DXF Format (.dxf) (导入为 DXF 格式)
   Add Curve: Extra Objects (额外曲线物体)
   Add Mesh: BoltFactory (螺栓)
   Add Mesh: Extra Objects (额外网格物体)
   Add Mesh: A.N.T.Landscape (简单地形制作)
   Interface: Modifier Tools (修改器工具)
   Interface: Copy Attributes Menu (复制属性菜单)
   Mesh: LoopTools (Loop 工具)
   Object: Bool Tool (布尔工具)
   Render: Auto Tile Size (自动 Tile 大小, 能自动帮你设置渲染块数量, 提供硬件利用率)
   UV: Magic UV (魔法 UV)
6. 最后别忘了保存设置  Edit&rarr;<u>P</u>references...&rarr;&equiv;&rarr;<u>S</u>ave Preferences

## 基础操作

### 观察物体:

{% asset_img 2-1.png %}

&#10112; 视角旋转 `<MiddleMouse>` (模拟三键鼠标 `<M-LeftMouse>`)
&#10113; 视角平移 `<S-MiddleMouse>` (模拟三键鼠标`<S-M-LeftMouse>`)
&#10114; 摄像机视图(切换) `<k0>`
&#10115; 透视/正交切换 `<k5>`
> 提示: 若没有小键盘, 可在首选项开启模拟数字键盘使普通数字键作为快捷键使用: Edit&rarr;<u>P</u>references...&rarr;Input&rarr;Keyboard&rarr;\[&check;\]Emulate Numpad

#### 摄像机

- 如何让摄像机跟随视角同步? (锁定相机到视图)
  `n` (开启 N 面板)&rarr;\[View\]&rarr;View Lock&rarr;\[&check;\]Camera to View
- 如何更改摄像机取景框的比例和大小?
  方法 1: &#x2780; 选中摄像机 (如 \[Outliner `<S-F9>`\]Scene Collection&rarr;Collection&rarr;\[&check;\]Camera) &#x2781; \[Properties `<S-F7>`\]&rarr;\[Output Properties\]&rarr;Format&rarr;{Resolution (X,Y,%),Aspect (X,Y)}
  方法 2: (关闭锁定相机到视图) 在相机视图下直接用鼠标滚轮和视角平移就可改变相机取景框在屏幕上的大小和位置.
- 找到一个不错的角度, 如何将当前的视角快速设为摄像机视图?
  &#x2780; 选中摄像机 &#x2781; `<C-M-k0>`

#### 切换四视图

四视图分割(切换) `<C-M>-q`
顶视图 `<k7>`; 底视图 `<C-k7>`
前视图 `<k1>`; 后视图 `<C-k1>`
右视图 `<k3>`; 左视图 `<C-k3>`
翻转 `<k9>` (若当前非正交视图则绕Z轴翻转)

#### 聚焦物体

- 选中物体&rarr;\[3D Viewpont `<S-F5>`\] `<kPoint>`
- 聚焦并隐藏其他物体(切换): 选中物体&rarr;\[3D Viewport `<S-F5>`\] `<kDivide>`
- 在四视图界面同步聚焦: `<C-kPoint>`
- 显示整个场景 `<Home>`

#### 物体变换

- 在物体上的实时显示移动、旋转、调整工具:
  \[3D Viewport `<S-F5>`\](Top bar)\[V\] Gizmos&rarr;Object Gizmos&rarr;\[&check;\]{Move,Rotate,Scale}
  > Gizmo 快速开关 ``<C-`>``
- 移动物体 `g`&rarr;{\[`<S>`-\](`x`,`y`,`z`),\[0-9\]+} 使其只在{轴,除去该轴的平面}上移动; 输入数字精确移动
  移动归零 `<M>-g`
- 旋转物体 `r`\[`r`\] (默认基于当前视角旋转, 再按切换视角深度旋转)&rarr;{`x`,`y`,`z`,\[0-9\]+} 使其只在对应轴上旋转; 输入数字精确旋转(角度制).
  旋转归零 `<M>-r`
- 缩放物体 `s`&rarr;{\[`<S>`-(`x`,`y`,`z`),\[0-9\]+} 使其只在{轴,除去该轴的平面}上缩放; 输入数字精确缩放.
  缩放归零 `<M>-s`

> 也可使用N面板调整物体: `n`&rarr;\[Item\]&rarr;Transform

#### 选择物体

- 切换选择工具(移动/框选/刷选/套索) \[3D Viewport `<S-F5>`\] `w`, 在 \[3D Viewport\] 顶栏还可设置工具属性 (如交集选择)
- 全选 `a`; 取消全选 `<M>-a` 或 `aa` (快速)

#### 新建/删除/复制物体

- 物体添加 `<S>-a`, 物品添加时的参数仅在新建时可调
- 删除物体 `<Del>` 或 `x` 
- 复制物体 选中物体&rarr;`<C-c> <C-v>`; 或者 `<S-d>`

## 坐标系与轴心点

### 物体坐标系

> 设置默认坐标系: \[3D Viewport\](Top bar)\[V\]Transform Orientations
> 快捷键 `,`

- 局部 (Local) 坐标系: 基于物体本身方向
  显示: 选中物体&rarr;\[Properties `<S-F7>`\]&rarr;\[Object Properties\]&rarr;Viewport Display&rarr;\[&check;\]Axis.
  > 在物体变换操作时, 多次按下轴字母可切换全局/默认坐标系, 
- 法向 (Normal) 坐标系
  实践: 切到法向坐标系&rarr;选中物体&rarr;编辑模式 `<Tab>`&rarr;启用面选择模式: \[3D Viewport\](Top bar) Face Selection Mode &rarr;点击一个平面, 尝试在 Z 轴上移动, 会发现这个平面的 Z 轴垂直于该平面 (法向)
  > 如果没有启用模拟小键盘的话, 在编辑模式可用快捷键 `1`、`2`、`3` 启用点、线、面选择模式);
  > 同样的, 进入变换模式后, 多次按下轴字母即可快速切换全局/默认坐标系.
- 万向 (Gimbal) 坐标系
  对于万向轴来说, 它可通过固定两个轴 (旋转轴和转向轴) 的方式让第三轴绕旋转轴旋转,
  如将圆球设置为旋转轴=它的局部 X 轴; 转向轴=它的局部 Z 轴:
  切到万向坐标系&rarr;选中该圆球&rarr;\[Properties `<S-F7>`\]&rarr;[Object Properties]&rarr;Transform&rarr;Mode='YXZ Eular' (第二个字母为旋转轴, 第三个字母为转向轴)
  当我们尝试将该圆球绕 X 轴旋转, Y 轴会发生变化, 但无论如何旋转, 万向坐标系的 Z 轴永远保持和全局坐标 Z 轴一个方向, 且万向 Z 轴与万向 X 轴始终保持正交 (即万向 X 轴永远在 XY 平面).
- 视图 (View) 坐标系
  坐标轴 XY 与视窗锁定, 上下为 Y, 左右为 X, 垂直屏幕方向为 Z

#### 自定义坐标系/游标坐标系

将默认坐标系设为某个物体的局部坐标系
选中物体&rarr;\[3D Viewport\](Top bar)\[V\]Transform Orientations&rarr;点击 '+' 号, 会生成一个与该物体名称相同的坐标系

游标 (Cursor) 坐标系: 和自定义坐标系相似, 不过是游标的局部坐标系. 要改变游标方向, 可使用游标工具或者 \[3D Viewport\] `<S-RightMouse>` 拖动, 移动后游标会朝向屏幕方向.

### 轴心点

- 原点: 选中单个物体后中心的橘黄色的小点称为原点. 变换原点: \[3D Viewport\](Upper Right Corner)[V]Options&rarr;Affect Only [&check;]Origins, 然后可以对原点进行移动旋转缩放
  恢复原点位置: 选中物体&rarr;\[3D Viewport\]`<RightMouse>`&rarr;Set <u>O</u>rigin&rarr;{<u>G</u>eometry to Origin,<u>O</u>rigin to Geometry,Origin <u>t</u>o 3D Cursor, Origin to <u>C</u>enter of Mass (Surface),Origin to Center of <u>M</u>ass (Volume)} 几何中心到原点, 原点到几何中心, 原点到3D游标, 原点到重心(表面), 原点到重心(体积)
- 变换轴心点: 选择多个物体后出现的一个变换位置用的轴心点, 要设置变换轴心点, 可使用句号键 `.` 调出浮动菜单或 \[3D Viewport\](Top bar)[V]Transform Pivot Point&rarr;{<u>B</u>ounting Box Center,3<u>D</u> Cursor,<u>I</u>ndividual Origins,<u>M</u>edian Point,<u>A</u>ctive Element} 边界框中心、3D游标、各自的原点、质心点、活动元素
- 质心点 (Median Point): 为变换轴心点计算的默认方式, 是基于原点计算产生的均值, 在只有一个物体选择的情况下, **质心等于原点**.
- 边界框中心 (Bounding Box Center): 选中多个物体的变换轴心点根据它们的边界框来计算.
  > 显示边界框: 选中物体&rarr;\[Properties\]&rarr;Object Properties&rarr;Viewport Display&rarr;[&check;]Bounds
- 3D游标 (3D Cursor): 变换轴心点是游标位置. 配合下一章的吸附.
- 各自的原点 (Individual Origins): 比较特别, 此模式下对选中的多个物体进行缩放和旋转都会对以各自的原点进行变换, 而不是一个统一的变换轴心点.
  实践: 选中多个物体然后旋转变换试试.
- 活动元素 (Active Element): 变换轴心点的位置是活动元素 (最后选择的物体, [Scene Collection] 中高亮的那个) 的原点

> 当使用句号键 `.` 在浮动菜单勾选 Only Locations (`9`), 选择的多个物体的缩放和旋转只会相对于变换轴心点进行相对的距离和角度的变换而不会改变物体本身的缩放和旋转.
> 如旋转操作不旋转物体本身, 只是相对于变换轴心点的角度进行位置上的"旋转"

## 吸附与衰减编辑工具

1. 吸附功能全解:
   开启/关闭吸附 `<S-Tab>` 或 (Top bar - Center) 3D Viewport&rarr;Snap, 也可在变换操作时按住 `<C>` 来临时开启/关闭吸附.
   (Top bar - center) 3D Viewpont&rarr;Snap 旁的下拉菜单可调整吸附内容 (Snap To), 被吸附对象吸附基准点 (Snap With), 影响的变换 (Move, Rotate, Scale):
   
   > - 为了方便观察, 切换线框模式 `<S>-z` 或 (Top bar - Right) 3D Viewport&rarr;Viewport Shading=Wireframe.
   > - 对于吸附基准点 (Snap With), **Closest=被吸附对象离吸附目标最近距离的顶点; Center=变换轴心点 (约等于质心, 但由变换轴心点的设置决定)**.
   > - `<S>-s` 呼出游标吸附浮动菜单, 可让游标吸附到各种位置上.
   >   如将游标吸附到某个选择的面上: `<Tab>` 进入编辑模式&rarr;`3` 进入面选择模式&rarr; 选中某个平面&rarr;`<S>-s` 呼出游标吸附菜单&rarr;Cursor to Selected.

   - 增量模式 (Increment): 物体变换操作时会吸附到背景网格上, 可利用不同缩放下网络间距不同来达到选择移动 0.1M 或 1.0M 的效果.
     默认是相对于网格的移动, 若在下拉菜单中勾选绝对栅格对齐 (Absolute Grid Snap), 则单个/多个物体的质心会被严格吸附到网格点上.
     如图所示 (俯视图), 移动后, 两个方块的质心点被吸附到网格上:
     {% asset_img 4-4.png %}
   - 顶点模式 (Vertex): 物体变换操作时会吸附到其他物体的顶点上.
     - 下拉菜单中勾选背面剔除 (Backface Culling) 后, 将不会吸附当前视角下处在物体背面的顶点.
     - 下拉菜单中勾选旋转对齐目标 (Align Rotation to Target) 后, 被吸附物体局部坐标轴的Z轴会与顶点法向对齐, 如图:
       {% asset_img 4-8.png %}
   - 其他的功能, 自己尝试吧~
     - 这里说一下面吸附模式的妙用, 可以很方便地实现让一个物体摆放在另一个物体之上:
       将要放置物体的原点置于底面的中心 (配合四视图比如正视图+吸附功能)&rarr;(Top bar - Center) 3D Viewport&rarr;\[V\] Snap&rarr;Snap to=face, Snap with=Median, 然后就可将这个物体完美放在另一个物体的平面上啦~
       {% asset_img 4-9.png %})
       {% asset_img 4-10.png %}
     - 面吸附模式下, 下拉菜单勾选项目独立元素 (Project Individual Elements) 后, 会使选择的多个物体使用各自的原点进行吸附.
       {% asset_img 4-12.png %}

       - 此外, 若在编辑模式下进行面吸附, 可将网格物体贴合到另一个物体的面上, 目前版本贴合似乎有bug, 建议多开一个窗口在正交视图下 (四视图) 进行贴合.
         {% asset_img 4-13.png %}
     - 垂直交线 (Edge Perpendicular) 模式: 物体会被吸附到吸附基准点与被吸附物体的边的垂直交线的交点上, 也适用于编辑模式下的顶点模式.
       {% asset_img 4-14.png %}
2. 衰减编辑: 快捷键 `O`. 启用衰减编辑后, 对其中一个物体进行变换会带动周围的物体一起进行变换.
   如图, 启用衰减编辑后, 变换物体时会出现一个灰色的圈, 圈内的物体会被带动, **鼠标滚轮可改变圈的大小**
   {% asset_img 4-15.png %}

   衰减模式有许多种, 自己尝试吧: (Top bar - Center) \[V\] Proportional Editing&rarr;{Smooth,Sphere,Root,Inverse Square,Sharp,Linear,Constant,Random}
   
   提一下衰减编辑配合网格 (Grid) 物体在编辑模式下的运用:
   {% asset_img 4-17.png %}
   {% asset_img 4-18.png %}
   勾选从视角投影 (Projected from View) 可做到根据视角的投影方向进行统一的操作:
   {% asset_img 4-19.png %}

## 常用基础操作 (游标、物体合并分立、操作重复执行、第一人称观察、物体隐藏、物体父子级、物体镜像变换)

1. 游标操作:
   游标移动相关浮动菜单 `<S>-s`
   游标快速移动 `<S-RightMouse(Drag)>`
   游标回到世界原点 `<S>-c`
2. 物体合并分离:
   合并: 选择物体&rarr;`<C>-j`
   分离: 选择物体&rarr;`<Tab>` 编辑模式&rarr;选择分离部分 (快捷键 `l` 选择相邻元素) 然后 `p` 或 `<M>-m`
   > 合并后原点为活动项原点, 分离后原点位置在分离物体原点上
3. 操作重复执行: 复制/变换等常规操作完成后&rarr;`<S>-r`
4. 第一人称观察和拍摄: ``<S>-` ``
   进入后, \[`<`{`S`,`M`}`>-`\]{`w`,`a`,`s`,`d`,`e`,`q`} \[加速,减速\]前后左右上下, `<Space>` 快速前进到准星物体处
   > 配合相机的锁定到视图可实现相机的自由移动
   > 记录K帧: 选中相机&rarr;\[Properties\] Object Properties&rarr;Transform&rarr;将鼠标指针停留在各项参数上&rarr;按 `i` 键启动记录 (图中记录位置旋转缩放的信息)&rarr;\[Time<u>l</u>ine\]\[&check;\] Auto Keying 开启自动记录关键帧&rarr;`<Space>` 启动播放&rarr;``<S>-` `` 进入第一人称模式对摄像机进行移动
   > {% asset_img 5-1.png %}
   > 可以选中后按 `x` 或 `<Del>` 来删除一些不想要的关键帧
   > 切到关键帧曲线编辑器 \[Graph <u>E</u>ditor\], 可对关键帧进行一些平滑处理:
   > {% asset_img 5-3.png %}
   > \[Graph <u>E</u>ditor\] `a` 键选择所有曲线&rarr; (Top bar - Left) \[Graph <u>E</u>ditor\] Key&rarr;Sm<u>o</u>oth Keys (快捷键 `<M>-o`) 来平滑关键帧. 平衡一次的效果可能不够明显, 可通过按住 `<M>-O` 平滑曲线 n 次:
   > {% asset_img 5-4.png %}
5. 物体隐藏、显示、禁用
   - 隐藏: 选中物体然后&rarr; `h` 或 \[Outliner\]&rarr;Scene Collection&rarr;点击物体旁边的"小眼睛".
   - 显示: 重新显示所有隐藏对象 `<M>-h`
   - 视图/渲染中禁用:
     \[Outliner\] 右上角过滤器&rarr;Restriction Toggles&rarr;{\[&check;\] Disable in Viewports (显示器图标),\[&check;\] Disable in Renders (相机图标)}
     然后就可对物体设置视图中禁用和渲染中禁用了:
     {% asset_img 5-7.png %}
6. 物体父子级: 子物体与父物体变换操作时同步
   * 建立父子级: 选中多个物体&rarr;`<C>-p` (活动项会成为父级)
   * 清空父级: `<M>-p`
  
   > 也可在 \[Outliner\] 按住 `<S>` 拖动物体来建立或取消父子级关系:
7. 物体镜像变换
   (复制一个保留在原位的物体方便参考, 选中物体&rarr; `<S>-d`&rarr;`<RightMouse>` 取消位移)
   选中物体&rarr;`<C>-m` 进入交互式镜像变换模式&rarr;{`x`,`y`,`z`} 或者 `<MiddleMouse(Drag)>` 可选择镜像轴

## 建模功能篇

### 初识编辑模式点线面与统计信息拓展

1. 点、线、面选择模式切换: `1`, `2`, `3`
   模式加减选: `<S>-`{`1`,`2`,`3`}
2. 可开启信息HUD来显示电线面等数据: \[3D Viewport\] (Top bar - Right)&rarr;Viewport Overlays&rarr;Guides&rarr;\[&check;\]Statistics
3. 物体模式 (Object Mode) 统计数据计算的所有物体数据总和
   编辑模式 (Edit Mode) 统计数据计算当前进入编辑物体数据总和
   三角形 (Triangles) 为网格等效为三角面之后的预估数量, 面是可以三角化的: 编辑模式下选择一个面&rarr;\[3D Viewport\] (Edit Mode - Top bar) Face&rarr;Triangulate Faces (`<C>-t`)

### 挤出类工具全解 (挤出、沿法向挤出、挤出各个面、挤出至游标)

1. 初识T面板
   左侧工具栏可通过 `T` 键开启/隐藏
   鼠标移动到工具栏上后按 `<kPlus>`, `<kMinus>` 可实现UI缩放.
   向右拖拽工具栏可以切换显示并排工具栏、名称工具栏 (推荐切换到下图的并排工具栏)
   {% asset_img 7-2.png %}
2. 挤出并移动
   进入编辑模式, T面板会出现编辑模式下的工具.
   选择挤出到选区工具 (Extrude Region, 快捷键 `e`), 单个面默认沿面法向、多个面默认沿质心法向, 按下 \[`<S>-`\]{`x`,`y`,`z`} \[锁定\]对应轴向方向的移动, 也可以进入对应的正交视图规避未显示轴向的数据干扰
   选中T面板挤出到选区工具&rarr;面选择模式选择一个面, 此时可看到一个黄色的加号&rarr;拖拽这个加号, 沿面法向挤出
   > **翻车点: 挤出到选区工具的操作是挤出并移动, 按右键取消的只是移动, 但依旧会进行挤出从而产生一个重叠面. 解决方案是: 开启X射线模式 \[3D Viewport\] (Top bar - Right) [&check;] Toggle X-Ray (`<M>-z`)&rarr;(Edit Mode - Vertex Selection Mode) 框选顶点&rarr;`<RightMouse>`&rarr;Mirror Vertices&rarr;By Distance (`m`).**
   > {% asset_img 7-6.png %}
   > 对于边法向, 由于不是很直观, 开启法向线的显示: \[3D Viewport\] (Top bar - Right) \[V\] Overlays&rarr;Viewport Overlays&rarr;Normals&rarr;Display Split Normals; Size=0.45.
   > 对于面法向, 除了在视图叠加层中开启法向线的显示外, 还可以启用面朝向, 蓝色为正面, 红色为反面: \[3D Viewport\] (Top bar - Right) \[V\] Overlays&rarr;Viewport Overlays&rarr;Geometry&rarr;\[&check;\] Face Orientation.
3. 沿法向挤出 (Extrude Along Normals)
   新建两个平面, 将其中一个面翻转 (选中这个面, 然后 `<M>-n` 调出分割菜单, 选择 Flip 翻转):
   {% asset_img 7-9.png %}
   可看到, 选中两个平面后, 挤出并移动工具会默认使用两物体质心点的法向:
   {% asset_img 7-10.png %}
   要让选择的平面独立使用自己的法向, 应使用沿法向挤出 (Extrude Along Normals) 工具, 此时控制柄会变化:
   {% asset_img 7-11.png %}
   此时拖动平面会沿着各自的法向进行挤出:
   {% asset_img 7-12.png %}
4. 挤出各个面 (Extrude Individual)
   让选择的每个面都独自的进行挤出; 在选取对象为多个相连面时, 沿法向挤出是相接的, 而挤出各个面是分开的.
   {% asset_img 7-15.png %}
   > 在沿法向挤出模式下, 选择正方形的两个面进行挤出, 会沿着这两个面的夹角作为法向进行挤出:
   > {% asset_img 7-13.png %}
   > 如果要让挤出变得平均, 需要在挤出操作的历史记录里勾选 Offset Even (均等偏移):
   > {% asset_img 7-14.png %}
5. 挤出至光标 (Extrude to Cursor)
   挤出到鼠标所在区域, 建议结合正交视图使用. 快捷键 (Edit Mode) `<C-RightMouse>`.
6. 挤出流形 (Extrude Manifold)
   使用其他挤出工具进行穿插挤压, 会出现多余面的问题:
   {% asset_img 7-16.png %}
   使用挤出流形则可很好地解决这个问题:
   {% asset_img 7-17.png %}
7. 快捷菜单
   编辑模式下选中目标&rarr;`<M>-e` 可弹出全部和挤出相关的功能
   > Extrude Repeat 重复挤出 (也可使用 `<S>-r` 重复上次变换), 在弹出的历史操作消息框里可进行重复挤出的设置:
   > {% asset_img 7-18.png %}
   > 绕视图旋转 (Spin): 可让轮廓沿着视图旋转成为一个立体模型:
   > 首先通过顶点挤出创建一个轮廓
   > {% asset_img 7-19.png %}
   > 然后**在俯视图下**, `<M>-e`&rarr;Spin 使其进行绕Z轴旋转:
   > {% asset_img 7-20.png %}
   > 当然, 可在历史操作里进行参数上的设置
   > {% asset_img 7-21.png %}

### 内插面工具全解(激活内插、深度、外插、分离、边界与其他相关参数)

激活内插面 `i`, 激活后:
- 深度 (Hold) `<C>`
- 外插 `o`
- 边界 `b`
- 分离 `i` (分离开启会影响到边界和外插属性)

面边界: 只被一个面包含的边

### 倒角工具全解(全参数解析、额外拓展、解决倒角不均匀)

1. 基础操作
   快捷键 `<C>-b`
   进入后可参考状态栏快捷键提示进行操作:
   切换段数 `<ScrollWheel`{`Up`,`Down`}`>` 或 (Drag) `s` 
   `p` 控制形状轮廓 (内凹还是外凸)
2. 宽度类型
   为了更好的演示, 将宽度、段数、形状的值调整至下图所示:
   {% asset_img 9-1.png %}
   针对宽度类型, 分别有偏移量 (Offset), 宽度 (Width), 深度 (Depth), 百分比 (Percent), 绝对值 (Absolute)
   对于前三个类型, 下图可以很好地解释:

   {% asset_img 9-2.png %}
   偏移量宽度类型 Offset=平行边, 间距=垂线距离
   百分比宽度类型 Percent 会根据倒角原始边的邻边长度决定, 需要注意邻边不同会产生倒角不均匀的问题.
   绝对值宽度类型 Absolute , 绝对值=平行线端点连线斜边长度>=垂线距离:
   > - 如何测两点距离:
   >   举个例子, 倒角参数设置 Width Type 为 Width, 段数 Segments 为2, Shape 为1:
   >   {% asset_img 9-3.png %}
   >   吸附模式为顶点吸附, 选中T面板的测量工具 Measure , (Hold) `<C>` (临时启用吸附) 拖动连接两个顶点即可:
   >   {% asset_img 9-4.png %}
   > - 如何测量 Width Type 为 Depth 的距离:
   >   做一条辅助线.
   >   选中两顶点按 `j` 在两点间作一辅助线, 吸附模式为顶点吸附和垂直交线吸附 Edge Perpendicular (按住 `<S>` 可同时启用多个吸附) 然后测量. 也可继续作一条垂直辅助线: 选中这条线段&rarr;`<RightMouse>`&rarr;Subdivide 将其一分为二. 平分后出现原线段的中点.
   >   {% asset_img 9-5.png %}
   >   {% asset_img 9-6.png %}
3. 其他参数全解
   - 材质编号 (Material Index)
     给物体上材质:
     选中物体&rarr;\[Properties\]&rarr;Material Properties&rarr;点击加号建立三个材质槽&rarr;将三个材质槽更名&rarr;\[V\]Surface&rarr;Base Color 设置基础色:
     {% asset_img 9-8.png %}
     然后开启材质显示: \[3D Viewport\] (Top bar - Right) Viewport Shading=Material Preview
     默认情况下物体会使用第一个材质, 可进入编辑模式指定该面的材质:
     编辑模式选择面&rarr;\[Properties\]&rarr;Material Properties&rarr;选择材质槽&rarr;Assign
     
     此时进行倒角操作, 会发现默认的 -1 材质编号会自动套用临边的材质. 更改这个参数来选择倒角边的材质:
     {% asset_img 9-10.png %}
   - 平衡着色与硬化法向
     想让倒角变得平滑, 最笨的方法是增加段数, 但面数增加会导致运算量增加.
     - 更好的方法是使用 `<RightMouse>` Object Context Menu&rarr;Shade Smooth 平滑着色
       默认平滑着色套用到全局, 可在 \[Properties\] Object Data Properties&rarr;\[V\]Normals&rarr;\[&check;\]Auto Smooth 限制自动平滑角度
     - 另一方法是硬化法向, 不影响原始法向, 仅平滑倒角.
       为了让效果更明显, 将视图着色方式改为快照材质: \[3D Viewport\] (Top bar - Right) Viewport Shading=Solid; \[3D Viewport\] (Top bar - Right) \[V\]Shading&rarr;Lighting&rarr;MatCap&rarr;Studio Light=ceramic_dark.exr.
       在倒角历史操作&rarr;\[&check;\]Harden Normals.
       {% asset_img 9-13.png %}
       {% asset_img 9-14.png %}
   - 钳制重叠 (Clamp Overlap) 可避免宽度增大到一定程度, 出现穿插的情况.
   - 外斜接 (Miter Outer): 锐边 (Sharp)、补块 (Patch)、圆弧 (Arc)
     {% asset_img 9-16.png %}
   - 内斜接 (Inner): 锐边(Sharp)、圆弧(Arc)
     {% asset_img 9-17.png %}
   - 相交类型 (Intersection Type): 栅格填充 (Grid Fill)、截止 (Cutoff)
     {% asset_img 9-18.png %}
   - 面强度 (Face Strength): 根据面权重限制法向平滑, 结合以后讲到的加权法向修改器可根据条件标记面权重强弱 (如面的面积、倒角区域), 实现不同强度区域不同程度平滑效果.
   - 标记 (Mark)
     - 缝合边 (Seams): 对一个带缝合边的边进行倒角时会出现缝合边断开, 历史操作菜单 (倒角)&rarr;Mark&rarr;\[&check;\]Seams 启用缝合边
       {% asset_img 9-22.png %}
       > 什么是缝合边? 有什么用?
       > 选中物体, `<Tab>` 进入编辑模式, \[Top Bar\] UV Editing 打开UV编辑器.
       > 删除原有UV映射: (选中物体) \[Properties\] Object Data Properties&rarr;\[V\]UV Maps&rarr;点击减号删除
       > 想象剪一个纸盒, 要剪开的边就是缝合边
       > {% asset_img 9-20.png %}
       > 按 `a` 全选, 进行UV展开: \[3D Viewport\] (Top bar - Right) UV&rarr;Unwrap
     
     - 锐边 (Sharp) 同理, 历史操作菜单&rarr;Mark&rarr;\[&check;\]Sharp 可自动补全断离锐边.
       > 什么是锐边? 有什么用?
       > 勾选了锐边后, 90&deg; 以下的法向自动平滑将不会对标记为锐边的边进行平滑.
       > {% asset_img 9-23.png %}
   - 自定义倒角轮廓:
     历史操作菜单&rarr;Profile Type=Custom
     图表里的顶点可拖移、删除:
     {% asset_img 9-25.png %}
     更改预设的参数后点击应用预设 Apply Preset 来刷新, 比如楼梯预设的段数:
     {% asset_img 9-24.png %}
4. 倒角不均匀与顶点倒角
   - 倒角不均匀: 注意物体是否进行过变形, `<C>-a` 应用菜单&rarr;All Transforms 全部变换.
   - 顶点倒角: 新建一平面, 选中&rarr;`<Tab>` Edit Mode&rarr;`<RightMouse>`&rarr;Subdivide
     `<C-S>-B` 进入顶点倒角模式, 参数的设置可参考边线倒角:
     {% asset_img 9-27.png %}

### 环切与偏移环切边工具全解(边线循环原理+全解析环切与偏移环切边工具)

1. 循环边传递原理
2. 环切工具全解
3. 偏移循环边工具全解

