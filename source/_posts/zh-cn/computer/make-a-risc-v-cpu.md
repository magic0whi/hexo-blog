---
title: make-a-risc-v-cpu
lang: zh-cn
category: computer
date: 2020-08-17 13:28:09
tags:
toc: true
---

如何造一个RISC-V架构的CPU

<!-- more -->

## 二进制

可按4位二进制对应1位16进制, 从右往左转换为16进制数
0对应0V (地线), 1对应5V

## 逻辑门

MOSFET, 全称 金属-氧化物半导体场效应晶体管, 分为N型和P型, 是组成逻辑门的基本元件
P型 输入0导通 输入1则断开, (有个小圈圈的是P型)
N型 输入1导通 输入0则断开
(在Logisim就叫Transistor)

8种逻辑门实现:
{% asset_img Logic_gate_implemented_by_mosfet.png %}
附文件: {% asset_link Logic_gate_implemented_by_mosfet.circ "Logic_gate_implemented_by_mosfet.circ" %}

## CPU 组成

### ALU

全称 Arithmetic & Logic Unit 算术和逻辑单元

CPU中用来计算加减乘除的单元
由加法器, 减法器, 乘法器, 除法器, 多路复用器组合而成

{% asset_img ALU.png %}

#### 加法器

元件符号: {% asset_img Adder.png %}

半加法器无法处理前者输出的进位
所以在半加法器基础上再加一个加法器, 以处理上一个加法器输出的进位
即前一个加法器处理 A+B=Sum , 后一个半加法器再处理 Sum+Carry

具体实现:
{% asset_img Half_adder_and_full_adder.png %}
附文件: {% asset_link Half_adder_and_full_adder.circ "Half_adder_and_full_adder.circ" %}

#### 减法器

减法器可由一个加法器加上另一个加法器和一个非门得到
{% asset_img Subtractor.png %}

原理:
(假设加法器只有4位)
5-4=5+(-4)=1=0B0001
将 -4 视为正数 4 然后按位取反再加一(称为二补数): 4 -> 0100 , 取反得 1011 , 1011+1=1100
将 5 和 0B1100 相加: 0B1100+0B0101=0B0001 (最高位溢出)
此时可以发现, 5+4的二补数 的结果正好等于 5-4 的结果

有符号数的最高位被用来表示符号位.
如4位的系统中,
0000~0111 代表0~7 这8个正数;
1000~1111代表 -8~-1 这8个负数

有符号数(Signed number): 能表示正数和负数
无符号数(Unsigned number): 只能表示负数

#### 多路复用器

Multiplexer 多路复用器, 简称 Mux
通过控制 Sel 以输出 A, B, C, D 之中其中一路的值

{% asset_img Mux.png %}

具体实现:
{% asset_img Four_way_multiplexer.png %}
附文件: {% asset_link Four_way_multiplexer.circ "Four_way_multiplexer.circ" %}

### 寄存器

(别去管它一开始的状态是怎样的)
寄存器由 8 个 D 触发器构成
{% asset_img A_register.png %}

1. S-R 锁存器
   S-R 代表 Set-Reset
   由2个或非门组成
2. D 锁存器
   D-Data, E-Enable
   由4个与非门组成
3. D 触发器
   D 触发器要求只能在时钟信号的上升沿的一小段时间内进行数据的修改
   原理是给 D 锁存器的 E 端接入一个时钟信号的上升沿触发器
   {% asset_img Clock_signal_and_Rising_edge.png %}

具体实现:
{% asset_img Registers.png %}
附文件: {% asset_link Four_way_multiplexer.circ "Four_way_multiplexer.circ" %}

### 时钟

### RAM

RAM分为SRAM和DRAM
内存单元

### 程序计数器

### 解码器

## RISC-V 架构

指令抓取 -> 指令解码 -> 执行 -> 内存操作 -> 回写

### 结构

### 指令集
