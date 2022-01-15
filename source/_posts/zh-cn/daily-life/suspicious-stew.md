---
title: suspicious-stew
toc: true
category: daily-life
lang: zh-cn
date: 2021-03-09 17:33:26
tags:
---

可疑的炖菜: 就是一些来路不明的未整合笔记

<!-- more -->

## 二次元萌萌人语录

(u1s1, 看着这些内容我都不自觉地脸红...)

呐呐呐，服务员欧内酱~~（超级肉麻）
诶多捏诶多捏，瓦塔西就是那个，二次元得斯~~（超级得意）
二次元の美好，米娜桑都知道的吧！（转身超级大声跟店里的顾客说了这句话）
 哒嘎啦，人家厚洗一对，那个二次元的徽章呐~偶捏该，瓦塔西斯够固sikisiki呆！！siki那个徽章呐~
纳尼?一定要那个口号吗….呜呜呜，哈子卡西….得莫，为了超级想要的二次元徽章….瓦塔西会干巴爹的！！！
异世相遇！！！！（华丽转圈圈）尽享美味！！！！！（转圈停下来然后跳起来对着服务员左手叉腰右手比着 ）
阿里嘎多欧内酱！！！呆siki了！​

米~娜~桑！新春佳节又来了desu哇~阿喏呐阿喏呐(｡>∀<｡)，首先呢新的一年呀，要-给-米-娜-桑拜个年desu！(*^ω^*)（姆Q）米娜桑新年おめでとうそしてそして在新的一年里，米~娜~桑的生活要摩多摩~~~多の西亚☆哇塞♡desu呦✧٩(ˊωˋ*)و✧，和往年一样呢，米~娜~桑的祝福呐！/

## 3B1B

向量是什么 https://www.bilibili.com/video/av5987715
导数的本质 https://www.bilibili.com/video/av24325548

## 构图

可应用于绘画, 摄影

0. 常用画面 1:1, 3:2, 4:3, 6:4, 按需裁剪
1. 中心构图法: 画面尽量对称; 将视觉主体和周围环境紧密结合, 但不能色彩撞衫导致主次不清
   对角线构图法
2. 第一眼让观众知道你拍的视觉主体是什么
3. 景物内容尽量完整
4. 人物视觉对象不能是显得堵的东西
5. 虚化对象要保留起码的特征

8. 尽量使主题出现在黄金分割线

需要避免的
1. 非刻意情况下画面要有层次感, 避免引起视觉错觉
2. 人物在视觉上不能在顶房梁
3. 脚不能被卡出画面外

附加
1. 飞向镜头的物品能增加视觉冲击力
2. 街头纪实摄影, 比如巷道口那样的一线天画内搭画框

## 字体

明日方舟标题英文字体是: NOVECENTOWIDE
明日方舟基建英文字体是 Bender
TNO字体: 主标题字体 Tannenberg, 旧版GUI字体 VT323, 新版GUI字体Aldrich


## Mechanism

一些零件规格, PCB 打样可以找嘉立创
DIY 准备工作:
防割板
电阻电容样品本(Pingcon 样品本 0603 封装, 0402 封装, 常用 IC 元件)
示波器推荐 DS213 开源示波器

### Screws

M3x10 3mm直径, 10mm螺纹端长度

M4x10r 4mm直径, 10mm螺纹端长度, 圆顶

### 螺母

M3nS 方形

### Pulley

GT2-16

### Motor

42步进电机
无刷伺服电机

### 同步带

聚氨酯 PU 同步带

### 驱动器

### 软件

1. multisim `模拟电路仿真`
2. Altium Designer 有开源替代 kicad
3. Autodesk Fusion 或 Rhino 6
4. Visual Studio Extension: Visual Micro


## Grandle

设置代理:

```conf gradle.properties
...
systemProp.http.proxyHost=hostname
systemProp.http.proxyPort=8080
systemProp.http.proxyUser=username
systemProp.http.proxyPassword=xxx
```

## 椭圆参数方程

\\(\begin{cases} x=a\cos t \\\ y=b\sin t \end{cases}(0\leqslant t\leqslant 2\pi)\\)

变形后可得椭圆方程:
\\(\begin{array}{l}\frac{x}{a}=\cos t \\\ \frac{y}{b}=\sin t\end{array}\rArr\begin{array}{l}\frac{x^2}{y^2}=\cos^2 t \\\ \frac{y^2}{b^2}=\sin^2 t\end{array}\rArr\frac{x^2}{a^2}+\frac{y^2}{b^2}=\cos^2 t+\sin^2 t=1\\)

## 因式分解

1. 整式乘法与整式除法
   整式乘法: 以 \\((x+1)(2x^2+3x-2)=2x^3+5x^2+x-2\\) 为例
   <div>
   $$
   \begin{array}{rrrrrrrr}
    & & & 2x^2 & + & 3x & - & 2 \\
    \times & & & & & x & + & 1 \\ \hline
    & & & 2x^2 & + & 3x & - & 2 \\
    & 2x^3 & + & 3x^2 & - & 2x & & \\ \hline
    & 2x^3 & + & 5x^2 & + & x & - & 2
   \end{array}
   $$
   </div>

   整式除法: 以 \\((2x^3+5x^2+x-2)\div(x+1)=(2x^2+3x-2)\\) 为例
   {% asset_img 1.png %}
2. 因式定理与余数定理:
   因式定理: 如果多项式 \\(f(a)=0\\) , 则多项式必含因式 \\((x-a)\\) ; 反之, 若多项式含有因式 \\((x-a)\\) , 则 \\(f(a)=0\\)
   余数定理: 用 \\((x-a)\\) 去除多项式 \\(f(x)\\) , 所得余式(相当于除法中的余数)是一个值为 \\(f(a)\\) 的常数
3. 试根法: 分解高次多项式时, 用<u>常数项因数</u>与<u>最高次项系数之因数</u>的比值(记为 \\(a\\))去试根, 若验证 \\(f(a)=0\\) 则 \\((x-a)\\) 可整除原多项式, 即 \\((x-a)\\) 为 \\(f(x)\\) 因式
   (试根法的本质是因式定理)
   如: \\(2x^3+5x^2+x-2=(x+1)(2x^2+3x-2)=(x+1)(2x-1)(x+2)\\)
   它的常数项因数(\\(\pm1\\)、\\(\pm2\\))和最高次项系数之因数(\\(\pm1\\)、\\(\pm2\\))的比值有 \\(\pm1\\)、\\(\pm2\\)、\\(\pm\frac{1}{2}\\) , 代入得其中 \\(-1\\)、\\(-2\\)、\\(\frac{1}{2}\\) 可整除原多项式

## IUPAC Organic

```
(1S,3R,4R,5R)-3-{[(2E)-3-(3,4-dihydroxyphenyl)prop-2-enoyl]oxy}-1,4,5-trihydroxycyclohexanecarboxylic acid

tri hydroxy cyclo hexane carboxylic
三羟基环己烷羧酸

tri- 三

meth- 甲
eth- 乙
prop- 丙
but- 丁
pent- 戊
hex- 己
hept- 庚
oct- 辛
non- 壬
dec- 癸

methylp 甲基

hydroxy 羟基
phenyl 苯基
cyclo- 环
-ane 烷(Alkanes)
carboxylic 羧酸
```

## 利用 Zerotier 白嫖校园网

"Zerotier 打洞, 永远滴神"

> 首先确定你的校园网有IPv6, 一个简单的方法是看看号称支持IPv6的手机支付宝, 能不能在只连接校园网且未登录Wifi的状态下正常使用.

1. 网关机配置转发和NAT:
   ```console
   sudo iptables -t filter -A FORWARD -i zt+ -s <你的Zerotier网络地址段> -d 0.0.0.0/0 -j ACCEPT
   sudo iptables -t filter -A FORWARD -i eth0 -s 0.0.0.0/0 -d <你的Zerotier网络地址段> -j ACCEPT
   sudo iptables -t nat -A POSTROUTING -o eth0 -s <你的Zerotier网络地址段> -j SNAT --to-source <你的网关机公网地址>
   ```
2. 在 [ZeroTier Central](my.zerotier.com) 中添加路由: `0.0.0.0/0 via <你的网关机在Zerotier网络中的地址>`
3. 在想要白嫖的电脑上, 启用 Zerotier 的 Allow Default Route
   
## 最小二乘法求回归直线方程的推导过程

回归直线方程: \\(\hat{y}=a+bx\\)
其中: \\(\hat{b}=\frac{\sum_{i=1}^n x_iy_i-n\bar{x}\bar{y}}{\sum_{i=1}^n x_i^2-n\bar{x}^2}\\) , \\(\hat{a}=\bar{y}-\hat{b}\bar{x}\\) (\\(\bar{x}\\) 和 \\(\bar{y}\\) 为 \\(x_i\\) 和 \\(y_i\\) 的均值)

证明:
用所有离差(近似值 \\(\hat{y}\_i\\) 和观察值 \\(y\_i\\) 的差)的平方和来表示总离差: \\(\displaystyle Q=\sum\_{i=1}^n(y\_i-\hat{y}\_i)^2=\sum\_{i=1}^n(y\_i-a-bx\_i)^2\\)
(因为离差有正有负, 直接加可能相互抵消)
由于平方又叫二乘方, 所以这种使"离差平方和为最小的方法"称为**最小二乘法**

开始变形:
<div>
$$
\scriptsize
\begin{array}{l}
Q=\displaystyle\sum_{i=1}^n(y_i-a-bx_i)^2=(y_1-a-bx_1)^2+\dots+(y_n-a-bx_n)^2 \\
=(y_1^2+a^2+b^2x_1^2+2abx_1-2ay_1-2bx_1y_1)+\dots+(y_n^2+a^2+b^2x_n^2+2abx_n-2ay_n-2bx_ny_n) \\
=\displaystyle\sum_{i=1}^n y_i^2+na^2+b^2\sum_{i=1}^n x_i^2+2ab\sum_{i=1}^n x_i-2a\sum_{i=1}^n y_i-2b\sum_{i=1}^n x_iy_i \\
=\displaystyle\sum_{i=1}^n y_i^2+na^2+b^2\sum_{i=1}^n x_i^2+2ab\cdot n\bar{x}-2a\cdot n\bar{y}-2b\sum_{i=1}^n x_iy_i \\
=\displaystyle\sum_{i=1}^n y_i^2-2b\sum_{i=1}^n x_iy_i+b^2\sum_{i=1}^n x_i^2+na^2-2na(\bar{y}-b\bar{x}) \\
=\displaystyle\sum_{i=1}^n y_i^2-2b\sum_{i=1}^n x_iy_i+b^2\sum_{i=1}^n x_i^2+n(a^2-2a(\bar{y}-b\bar{x})) \\
=\displaystyle\sum_{i=1}^n y_i^2-2b\sum_{i=1}^n x_iy_i+b^2\sum_{i=1}^n x_i^2+n(a^2-2a(\bar{y}-b\bar{x})+(\bar{y}-b\bar{x})^2-(\bar{y}-b\bar{x})^2) \\
=\displaystyle\sum_{i=1}^n y_i^2-2b\sum_{i=1}^n x_iy_i+b^2\sum_{i=1}^n x_i^2+n(a-(\bar{y}-b\bar{x}))^2-n(\bar{y}-b\bar{x})^2 \\
=\displaystyle\sum_{i=1}^n y_i^2-2b\sum_{i=1}^n x_iy_i+b^2\sum_{i=1}^n x_i^2+n(a-(\bar{y}-b\bar{x}))^2-n\bar{y}^2+2nb\bar{x}\bar{y}-nb^2\bar{x}^2 \\
=\displaystyle(\sum_{i=1}^n y_i^2-n\bar{y}^2)-2b(\sum_{i=1}^n x_iy_i+n\bar{x}\bar{y})+b^2(\sum_{i=1}^n x_i^2-n\bar{x}^2)+n(a-(\bar{y}-b\bar{x}))^2
\end{array}
$$
</div>

到此, 需要两个关键变形公式以继续变形:
1. \\(\displaystyle\sum_{i=1}^n(x_i-\bar{x})^2=\sum_{i=1}^nx_i^2-n\bar{x}^2\\)
   证明:
   <div>
   $$
   \scriptsize
   \begin{array}{ll}
   \displaystyle\sum_{i=1}^n(x_i-\bar{x})^2 & =(x_1-\bar{x})^2+\dots+(x_n-\bar{x})^2 \\
   & =(x_1^2-2x_1\bar{x}+\bar{x}^2)+\dots+(x_n^2-2x_n\bar{x}+\bar{x}^2) \\
   & =(x_1^2+\dots+x_n^2)+n\bar{x}^2-2\bar{x}(x_1+\dots+x_n) \\
   & \displaystyle=\sum_{i=1}^n x_i^2+n\bar{x}^2-2n\bar{x}\frac{(x_1+\dots+x_n)}{n} \\
   & \displaystyle=\sum_{i=1}^n x_i^2+n\bar{x}^2-2n\bar{x}^2 \\
   & \displaystyle=\sum_{i=1}^n x_i^2-n\bar{x}^2
   \end{array}
   $$
   </div>
2. \\(\displaystyle\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})=\sum_{i=1}^n x_iy_i-n\bar{x}\bar{y}\\)
   证明:
   <div>
   $$
   \scriptsize
   \begin{array}{ll}
   \displaystyle\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y}) & =(x_1-\bar{x})(y_1-\bar{y})+\dots+(x_n-\bar{x})(y_n-\bar{y}) \\
   & =(x_1y_1+\bar{x}\bar{y}-x_1\bar{y}-y_1\bar{x})+\dots+(x_ny_n+\bar{x}\bar{y}-x_n\bar{y}-y_n\bar{x}) \\
   & =(x_1y_1+\dots+x_ny_n)+n\bar{x}\bar{y}-\bar{y}(x_1+\dots+x_n)-\bar{x}(y_1+\dots+y_n) \\
   & \displaystyle=\sum_{i=1}^n x_iy_i+n\bar{x}\bar{y}-n\bar{y}\frac{x_1+\dots+x_n}{n}-n\bar{x}\frac{y_1+\dots+y_n}{n} \\
   & \displaystyle=\sum_{i=1}^n x_iy_i+n\bar{x}\bar{y}-n\bar{y}\bar{x}-n\bar{x}\bar{y} \\
   & \displaystyle=\sum_{i=1}^n x_iy_i-n\bar{x}\bar{y}
   \end{array}
   $$
   </div>
   
接上面:
<div>
$$
\scriptsize
\begin{array}{rl}
Q= & \displaystyle(\sum_{i=1}^n y_i^2-n\bar{y}^2)-2b(\sum_{i=1}^n x_iy_i+n\bar{x}\bar{y})+b^2(\sum_{i=1}^n x_i^2-n\bar{x}^2)+n(a-(\bar{y}-b\bar{x}))^2 \\
= & \displaystyle\sum_{i=1}^n(y_i-\bar{y})^2-2b\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})+b^2\sum_{i=1}^n(x_i-\bar{x})^2+n(a-(\bar{y}-b\bar{x}))^2 \\
= & \displaystyle\sum_{i=1}^n(y_i-\bar{y})^2+\sum_{i=1}^n(x_i-\bar{x})^2(b^2-2b\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2})+n(a-(\bar{y}-b\bar{x}))^2 \\
= & \displaystyle\sum_{i=1}^n(y_i-\bar{y})^2+\sum_{i=1}^n(x_i-\bar{x})^2(b^2-2b\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2}+(\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2})^2 \\
& \displaystyle-(\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2})^2)+n(a-(\bar{y}-b\bar{x}))^2 \\
= & \displaystyle\sum_{i=1}^n(y_i-\bar{y})^2+\sum_{i=1}^n(x_i-\bar{x})^2(b-\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2})^2 \\
& \displaystyle-\sum_{i=1}^n(x_i-\bar{x})^2(\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2})^2+n(a-(\bar{y}-b\bar{x}))^2 \\
= & \displaystyle\sum_{i=1}^n(y_i-\bar{y})^2+\sum_{i=1}^n(x_i-\bar{x})^2(b-\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2})^2 \\
& \displaystyle-\frac{[\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})]^2}{\sum_{i=1}^n(x_i-\bar{x})^2}+n(a-(\bar{y}-b\bar{x}))^2 \\
\end{array}
$$
</div>

至此, 公式变形结束.
观察公式, 其中 \\(\scriptsize-\frac{[\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})]^2}{\sum_{i=1}^n(x_i-\bar{x})^2}\\) , \\(\scriptsize\sum_{i=1}^n(y_i-\bar{y})^2\\) 为常数项与 \\(a\\) , \\(b\\) 无关.
因此只需使 \\(b=\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2}\\) , \\(a=\bar{y}-b\bar{x}\\) 即可得到最小 \\(Q\\) 值

## 咖啡的种植

1. 种子培育
   1. 用洗净的中细河沙做催芽基质
   2. 种子剥去羊皮纸外壳, 以60度以内温水泡种催芽2-7天\[根据地方气候决定催芽时间长短\], 夏季2-4天即可.
      每日换水一次, 直到白色胚芽凸起. 可以正式育苗.
   3. 种子均匀铺在苗床上, 种子不可重叠, 点种是种子裂口处朝上覆盖一层稻草或其他保湿物. 或以珍珠棉和泡沫箱盖住, 留有部分空间不可盖严.
      2-3日浇一次水, 如气温高水分蒸发过快需每日浇水. (用喷雾)
   4. 可在上面搭建塑料透明薄膜棚子, 但待苗出土后幺注意遮阴.
   5. 20-40天出苗, 但各地气温不同, 出苗会有前后.
   6. 催芽床可用600倍多菌灵喷雾喷催芽床四周, 催芽过程中, 适时喷药.
      预防小苗猝倒病, 如有猝倒及时隔离.
   7. 预防蚂蚁蟋蟀地老虎等害虫.
2. 育苗期
   1. 搭建遮阴棚遮阴, 咖啡喜阴不可在太阳下直晒.
   2. 将20公分左右的咖啡幼苗移植到装有营养土的营养袋.
   3. 适度浇水, 保持土壤抓起来可成团, 搓, 可散开.
   4. 移植幼苗成活后, 30天左右可少量施水肥.
3. 咖啡盆栽管理
   1. 咖啡习性L咖啡喜阴怕晒, 耐热怕冷, 怕旱怕涝, 喜肥怕贫瘠.
   2. 咖啡苗移盆时尽量不要弄散原培养土, 所换花盆盆地加碎石子提高沥水性. 土表见干即可浇水, 如出现短暂脱水叶片会蔫掉浇水后6小时即可回转.
   3. 施肥: 咖啡施肥每个月一次调水浇灌, 最好使用专用肥. 用量少效果好, 没有也可用普通氮磷钾肥代替效果可能稍差. 施肥量专用肥一年苗不超过20粒每次\[调水浇\]. 三年苗不超过80粒每次\[调水浇\]. 施肥后一个星期不可出现脱水现象.
   4. 防虫: 咖啡主要有蚜虫, 钻心虫, 蚧壳虫, 毛毛虫. 使用石灰水刷咖啡主干可有效防止. 如出现害虫用吡虫啉喷雾可有效杀死害虫.
   5. 病害: 炭疽病, 锈病. 本人也只是听说从未见过. 我地多年来从未出现过咖啡病害, 本人也无计可施.
4. 咖啡品种分辨
   1. 铁皮卡即蓝山咖啡, 铁皮卡是阿拉比卡系列里血统最纯品质最高的品种. 树系高大枝叶稀疏形似野生, 新叶呈古铜色, 叶质轻薄柳长. 籽粒椭圆略长于其他品种, 颜色略微泛黄.
   2. 卡蒂姆(Catimor): 1959年, 葡萄牙人将巴西卡杜拉与提摩混血, 培育出抗病能力强的卡蒂姆/卡提摩, 目前是商用豆的重要品种.
   3. 波邦分黄波邦和波邦, 黄波邦果实呈黄色. 树形与卡蒂姆难于区分...卡蒂姆产量高于波邦. 唯有挂果期好分辨...

## 尺码对照表

<table>
<thead>
  <tr>
    <th colspan="7">上衣(女)</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>标准</td>
    <td>国际</td>
    <td>中国</td>
    <td>胸围(cm)</td>
    <td>腰围(cm)</td>
    <td>肩宽(cm)</td>
    <td>适合身高(cm)</td>
  </tr>
  <tr>
    <td rowspan="8">尺码明细</td>
    <td>XXXS</td>
    <td>145/73A</td>
    <td>74~76</td>
    <td>58~60</td>
    <td>34</td>
    <td>147~150</td>
  </tr>
  <tr>
    <td>XXS</td>
    <td>150/76A</td>
    <td>76~78</td>
    <td>60~62</td>
    <td>35</td>
    <td>150~153</td>
  </tr>
  <tr>
    <td>XS</td>
    <td>155/80A</td>
    <td>78~81</td>
    <td>62~66</td>
    <td>36</td>
    <td>153~157</td>
  </tr>
  <tr>
    <td>S</td>
    <td>160/84A</td>
    <td>82~85</td>
    <td>67~70</td>
    <td>38</td>
    <td>158~162</td>
  </tr>
  <tr>
    <td>M</td>
    <td>165/88A</td>
    <td>86~89</td>
    <td>71~74</td>
    <td>40</td>
    <td>163~167</td>
  </tr>
  <tr>
    <td>L</td>
    <td>170/92A</td>
    <td>90~93</td>
    <td>75~79</td>
    <td>42</td>
    <td>168~172</td>
  </tr>
  <tr>
    <td>XL</td>
    <td>175/96A</td>
    <td>94~97</td>
    <td>80~84</td>
    <td>44</td>
    <td>173~177</td>
  </tr>
  <tr>
    <td>XXL</td>
    <td>180/100A</td>
    <td>98~102</td>
    <td>85~89</td>
    <td>46</td>
    <td>177~180</td>
  </tr>
</tbody>
</table>

<table>
<thead>
  <tr>
    <th colspan="5">裤子(女)</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>标准</td>
    <td>国际</td>
    <td>中国</td>
    <td>腰围(cm)</td>
    <td>臀围(cm)</td>
  </tr>
  <tr>
    <td rowspan="9">尺码明细</td>
    <td>XXXS</td>
    <td>23</td>
    <td>55~57</td>
    <td>77~80</td>
  </tr>
  <tr>
    <td>XXS</td>
    <td>24</td>
    <td>57~60</td>
    <td>80~83</td>
  </tr>
  <tr>
    <td>XS</td>
    <td>25</td>
    <td>60</td>
    <td>83</td>
  </tr>
  <tr>
    <td>S</td>
    <td>26</td>
    <td>63</td>
    <td>87</td>
  </tr>
  <tr>
    <td>M</td>
    <td>27</td>
    <td>67</td>
    <td>90</td>
  </tr>
  <tr>
    <td>L</td>
    <td>28</td>
    <td>70</td>
    <td>93</td>
  </tr>
  <tr>
    <td>XL</td>
    <td>29</td>
    <td>73</td>
    <td>97</td>
  </tr>
  <tr>
    <td>XXL</td>
    <td>30</td>
    <td>77</td>
    <td>100</td>
  </tr>
  <tr>
    <td>XXXL</td>
    <td>31</td>
    <td>80</td>
    <td>103</td>
  </tr>
</tbody>
</table>

<table>
<thead>
  <tr>
    <th colspan="7">上衣(男)</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>标准</td>
    <td>国际</td>
    <td>中国</td>
    <td>胸围(cm)</td>
    <td>腰围(cm)</td>
    <td>肩宽(cm)</td>
    <td>适合身高(cm)</td>
  </tr>
  <tr>
    <td rowspan="5">尺码明细</td>
    <td>S</td>
    <td>165/80A</td>
    <td>82~85</td>
    <td>72~75</td>
    <td>42</td>
    <td>163~167</td>
  </tr>
  <tr>
    <td>M</td>
    <td>170/84A</td>
    <td>86~89</td>
    <td>76~79</td>
    <td>44</td>
    <td>168~172</td>
  </tr>
  <tr>
    <td>L</td>
    <td>175/88A</td>
    <td>90~93</td>
    <td>80~84</td>
    <td>46</td>
    <td>173~177</td>
  </tr>
  <tr>
    <td>XL</td>
    <td>180/92A</td>
    <td>94~97</td>
    <td>85~88</td>
    <td>48</td>
    <td>178~182</td>
  </tr>
  <tr>
    <td>XXL</td>
    <td>185/96A</td>
    <td>98~102</td>
    <td>89~92</td>
    <td>50</td>
    <td>182~187</td>
  </tr>
  <tr>
    <td></td>
    <td>XXXL</td>
    <td>190/100A</td>
    <td>103~107</td>
    <td>93~96</td>
    <td>52</td>
    <td>187~190</td>
  </tr>
</tbody>
</table>

<table>
<thead>
  <tr>
    <th>裤子(男)</th>
    <th></th>
    <th></th>
    <th></th>
    <th></th>
    <th></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>标准</td>
    <td>国际</td>
    <td>中国</td>
    <td>身高</td>
    <td>腰围(cm)</td>
    <td>臀围(cm)</td>
  </tr>
  <tr>
    <td rowspan="9">尺码明细</td>
    <td>XXXS</td>
    <td>28</td>
    <td></td>
    <td>70</td>
    <td>93</td>
  </tr>
  <tr>
    <td>XXS</td>
    <td>29</td>
    <td>160/66A</td>
    <td>73</td>
    <td>97</td>
  </tr>
  <tr>
    <td>XS</td>
    <td>30</td>
    <td>165/70A</td>
    <td>77</td>
    <td>100</td>
  </tr>
  <tr>
    <td>S</td>
    <td>31</td>
    <td>170/74A</td>
    <td>80</td>
    <td>103</td>
  </tr>
  <tr>
    <td>M</td>
    <td>32</td>
    <td>175/78A</td>
    <td>83</td>
    <td>107</td>
  </tr>
  <tr>
    <td>L</td>
    <td>33</td>
    <td>180/82A</td>
    <td>87</td>
    <td>110</td>
  </tr>
  <tr>
    <td>XL</td>
    <td>34</td>
    <td>185/86A</td>
    <td>90</td>
    <td>113</td>
  </tr>
  <tr>
    <td>XXL</td>
    <td>36</td>
    <td>185/86A</td>
    <td>93</td>
    <td>117</td>
  </tr>
  <tr>
    <td>XXXL</td>
    <td>38</td>
    <td>190/90A</td>
    <td>97</td>
    <td>123~127</td>
  </tr>
</tbody>
</table>

**上述腰围指实际腰围, 并不是裤子的尺码**

## 读书摘录

"在写作当中运用别人的语句并不就意味着模仿或抄袭, 写出来的东西并非毫无价值可言, 因为它至少
能够说明我已经能够灵活地驾驭这些优美的文字, 能够表达我对那些优美的、富有诗意的思想的欣赏" -- *假如给我三天光明*

"I tell you I must go!" I retorted, roused to something like passion. "Do you think I can stay to become nothing to you? Do you think I am an automaton?--a machine without feelings? and can bear to have my morsel of bread snatched from my lips, and my drop of living water dashed from my cup? Do you think, because I am poor, obscure, plain, and little, I am soulless and heartless? You think wrong!--I have as much soul as you,--and full as much heart! And if God had gifted me with some beauty and much wealth, I should have made it as hard for you to leave me, as it is now for me to leave you. I am not talking to you now through the medium of custom, conventionalities, nor even of mortal flesh;--it is my spirit that addresses your spirit; just as if both had passed through the grave, and we stood at God's feet, equal,--as we are!" -- *Jane Eyre*

"I am no bird; and no net ensuares me: I am a free human being with an independent will, which I now exert to leave you." -- *Jane Eyre*

"但在绝大部分的明清通俗小说中, 尽管对于科举制度的不平、愤激、斥责俯首皆是, 但叙述时字里行间却仍然包含了对于科举的依赖和眷念, 这尤其体现为**男主人公获得进士科名往往是小说团圆大结局结局不可或缺的元素**." -- *儒林外史-导读* 叶楚炎

"而与之相比, 吴敬梓对于科举社会的种种情状却有着更深的洞察力和表现力: 无论是对于科举社会中士人生存困境的呈现, 还是对于诸多弊端的反思, 以及对于儒林中人出路的探寻, 《儒林外史》都远远地超过了同题材的这些作品." -- *儒林外史-导读* 叶楚炎

"消除胆怯为当务之急, 对此, 英国大作家汤玛士&#183;卡莱尔曾说: "要想成为一个真正的人, 第一就要征服恐惧不安." 为了达到征服的目的, 第一步骤就是 "行动". 也就是说要积极向前迈进. 如此, 恐惧不安必能被消除. 勇敢采取行动向前迈进吧. 希尔多&#183;罗斯福就是因这种积极的行动, 结果很成功地消除了恐惧不安. 他说: "我经常被 '不安' 所困扰, 可是我从不向他低头, 也从不担心未来的任何事, 所以, '不安' 就逐渐消失了. "" --如何消除胆怯(ISBN7-5048-2005-9/Z&#183;290)

"如果碰到你不懂的事又不敢或不肯问, 那么你就真的注定要笨到底了. 因为怕显得笨而更笨的人是无可救药的." --如何消除胆怯(ISBN7-5048-2005-9/Z&#183;290)

"我们随别人喜而喜, 随别人忧而忧. 如是说来, 你我果真成了一张扑克牌, 一张任人揉捏、任别人摆弄的扑克牌? 没有自己的主张, 没有自己的愿望, 更没有自己的自尊. 就这样一生都摆脱不了别人的支配与选择?
果真是这样吗?
不! 我们绝不是一张扑克牌!
扑克牌毕竟没有思维, 而我们, 却是一群有着高级思维能力的活生生的人啊! 我们本该拥有与扑克牌截然不同的人生! 自己的历史靠自己书写, 自己的青春靠自己去创造, 自己的世界靠自己去闯! 而不是像牌那样在冥冥中失去自我!" --如何消除胆怯(ISBN7-5048-2005-9/Z&#183;290)

"感到畏惧的时候, 你就去做你畏惧的事, 不久你就不用再畏惧它了.
如果你害怕某一件事, 你试着不要让自己沉溺于这事的想象中, 首要的责任是征服恐惧, 为此需要积极的、勇往直前的行动, 去攻击恐惧, 攻击的力量越大. 畏惧消失得也就越快."--如何消除胆怯(ISBN7-5048-2005-9/Z&#183;290)

"罪恶之源无关紧要, 人们关心的是对抗它和战胜它, 既不要乐观, 也不要悲观, 要把不懈努力使事情变得更美好作为唯一指南. " -- (西)伊巴涅斯

"每天务必要做一点你所不愿意做的事情. 这是一条最宝贵的准则, 它可以使你养成认真尽责而不以为善的习惯. " -- (美)马克&#183;吐温

"理想的(绝大多数是内在的)学习动机着重于战略和战术, 而并不那么强调立竿见影的效果, 因此, 需有极强的忍受挫折的耐力才行, 因为解决问题的方法只有在重重困难中方可产生. 因此可以判断, 程序教学的学习计划和形式把任何微不足道的思维活动都当作成功的做法, 是与创造力敌对的" -- <创造力> (德)海纳特