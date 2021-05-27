---
title: suspicious-stew
toc: true
category: learn
lang: zh-cn
date: 2021-03-09 17:33:26
tags:
---

可疑的炖菜

就是一些未整合的笔记

<!-- more -->

## 萌萌人语录

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