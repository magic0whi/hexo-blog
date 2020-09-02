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
有栅极 G, 漏极 D, 源极 S 三个端口
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

ALUSel:
0000 加法(add)
0001 and
0010 or
0011 xor
0100 shift logical right(srl)
0101 shift arithmetic right(sra)
0110 shift logical left(sll)
0111 compare less than(slt)
1000 除法(div)
1001 求余(rem)
1010 正数乘法(mul)
1011 正数乘法(后32位)(mulh)
1100 减法(sub)
1101 直接输出B(bsel)
1110 我还不知道
1111 我还不知道

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
时钟每上沿一次, 计数+4(内存中一条字线管理32位数据, 也就是4个字节)

具体实现:
{% asset_img Program_Counter.png %}
{% asset_link Program_Counter.circ "Program_Counter.circ" %}

## RISC-V 架构

指令抓取 -> 指令解码 -> 执行 -> 内存操作 -> 回写

### 结构

RISC-V架构(按先后顺序排列:
PC寄存器: 存储在指令内存中, CPU当前执行的位置
指令内存(IMEM): 储存需要运行的指令, 程序运行时, 数据不能够被更改
32个寄存器(reg0~reg31): 储存程序运行时的一些临时变量
ALU: 逻辑与数学运算
数据内存(DMEM): 负责存储较大的数据, 程序运行时能够被读写

以上模块构成了RICV-V的数据管线(Data Pipeline)

控制单元(Control Unit): 充当"阀门", 负责协调以上模块
指令集(Instruction Set): 特定架构下CPU实现的一套操作, 如加减法, 

### 指令集

#### 核心指令格式

规定了所有指令集需要参照的格式(见表格):
{% asset_img Clock_signal_and_Rising_edge %}
RISC-V中, 单条指令长度固定32位, 有六个基本类型(R I S SB U UJ)

opcode: 规定指令内容(类似于编程中的函数符号, 如func(var1, var2) ), 7位.

rd: 目标寄存器(Destination reg), 指定指令结果存储到哪个寄存器中, 5位(对应32个寄存器). 而 S, SB 这两种没有规定 rd, 说明这两种指令操作不会产生新的值(类似于编程中没有返回值的函数).
rs1 和 rs2: 来源寄存器(source reg), 类似于函数的参数
如 `a+b=c` 中, `rs1` 对应 `a`, `rs2` 对应 `b`, `rd` 对应 `c`

funct3和funct7: 函数(function), 3代表3位, 7代表7位, 作用是定义函数的实现(如规定`a?b`是加法 `a+b` 还是减法 `a-b`)

imm: 立即数(immediate value), 类似于常数(如 a+4=c 的 4)

#### RV64I 基础指令

{% asset_img RV61I_Base_Integer_Instructions.png %}

注意: RISC-V 虽然有32个寄存器, 但寄存器 x0 的值恒为 0, 这是因为 0 这个数经常被用到, 如将 x1 初始化为 0 可用 `addi x1, x0, 0`

图中用的是Verilog, 一种逻辑描述语言, 若你懂些英文, 建议看一看再读下面的内容
{% asset_link Verilog_for_61C.pdf "Verilog_for_61C.pdf" %}

汇编语言的格式:
`add x1, x2, x3` , x1 代表 rd, x2 代表 rs1, x3 代表 rs2
`addi x1, x2, 5` , 5 是立即数(或者叫常数)

一些指令的解释:
1. add 和 addw 这样的指令实现的是相同的操作, 区别在于带 'w' 后缀表示与32位兼容(只使用64位寄存器的前32位)
2. jal: 将指令指针从当前位置往后跳 imm/2 行(为什么除2而不是4, 因为有短指令, 具体看下面).
   从表中看到, 它的定义是 R[rd]=PC+4; PC=PC + {imm,1b'0}, 由分号分隔, 说明这个指令有两个步骤.
   第一步是将(本来要执行的)指令地址储存到 rd 中(方便那边的指令执行完毕后跳转回来)
   第二步是修改PC寄存器, 同时赋值的 imm 左移补一个 0 (至于为什么要补一个0而不是两个0, 因为RISC-V虽然一般的指令长度是32位(4字节), 但它也支持16位的短指令, 所以跳转的最小值实际上是2. 你会发现, imm每次+1, 因为后面补了一个0, 实际上+1就变成+2了, 补两个0就是+4)
3. 有些时候, 若某个程序比较大, 以至于需要跳转的行数特别大(jal->属于UJ指令->imm最大20位->最大跳转长度 2^20),
   无法直接跳转, 就需要组合技 auipc + jalr
   auipc 后面的 imm 补了12个0(换算成十进制是4096 -> 往后跳 imm * 1024行), 可以表达很大的跳转行数, 但不会实际执行, auipc 只是将计算后的地址储存到 rd 中.
   jalr 是从寄存器取得指令内存地址, 并加上 imm/4 行(jalr 的 imm 竟然没有补0)后赋值到PC, 同样的, 将(本来要执行的)指令地址储存到 rd 中以方便跳转回来.
   示例(我要将PC地址跳1024+5行, 并将回跳地址保存在寄存器 x2):
   auipc x1, 1
   jalr x2, x1, 20

#### 从指令集到机器代码

机器代码是指令实际储存在内存中的样子

举个例子:
addi x1, x0, 1
要将他转化为机器码, 需要查表(本章开局那里)得到addi的参数:
type: I
opcode: 0010011
funct3: 000
因为addi是I型, 根据Core Instruction Formats的定义将这句汇编代码翻译成机器码:
31        20 19   15 14  12 11    7 6     0
000000000001  00000    000   00001  0010011

为了方便可以转成16进制: 0x00100093

## CPU实现

附上一个花了我三天的CPU实现, 参考了 [T-K-233](https://space.bilibili.com/14120234/) 的设计, 一些地方略有不同

{% asset_img risv_v_simple.png %}

{% asset_link risv_v_simple.circ "risv_v_simple.circ" %}