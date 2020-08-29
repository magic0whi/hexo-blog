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

> Bit Extender 和 Splitter 都可以用来扩充地址以使信号兼容高位宽线路, 
> 此外, Splitter 可裁剪数据以使电路兼容低位宽线路, 如程序计数器

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

触发器=锁存器+时钟信号的调控

1. S-R 触发器
   S-R 代表 Set-Reset
2. D 触发器
   D 代表 Data

   {% asset_img Clock_signal_and_Rising_edge.png %}

   将时钟上沿信号直接接到Enable端口上, 数据的更改受到时钟信号调控,
   然而不能长期保存数据
3. 寄存器
   在时钟信号这边另加一个与门和端口, 这样就可以决定是否放行时钟信号,
   从而防止数据被时钟信号刷掉

具体实现:
{% asset_img Registers.png %}
附文件: {% asset_link Four_way_multiplexer.circ "Four_way_multiplexer.circ" %}

### RAM

然后是底层原理:
Logisim中无法直接模拟DRAM, SRAM我试过也不行

{% asset_img RAM.png %}
横着的线称为字线(wordline), 用于启用指定地址下的8个内存单元
竖着的线称为位线(bitline), 负责数据的输入和输出
解码器(Decoder)负责根据输入的内存地址开启对应的字线

附上解码器(Decoder)的具体实现
{% asset_img 2_bit_Decoder.png %}
附文件: {% asset_link 2_bit_Decoder.circ "2_bit_Decoder.circ" %}

附上一个RAM的使用示例:
{% asset_img RAM_example.png %}
文件: {% asset_link RAM_example.circ "RAM_example.circ" %}

#### 程序计数器

{% asset_img _Program_Counter.png %}
程序计数器用于指向当前指令(IMEN)内存中的地址
时钟每上沿一次, 计数+4(内存中一条字线管理32位的数据, 也就是4个字节)

具体实现:
{% asset_img Program_Counter.png %}
{% asset_link Program_Counter.circ "Program_Counter.circ" %}

## RISC-V 架构

指令抓取 -> 指令解码 -> 执行 -> 内存操作 -> 回写

### 结构

### 指令集
