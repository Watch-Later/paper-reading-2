# CPU Logic Design and HCL

Created: 2022-05-03 16:30

## 数字系统 & 逻辑综合

数字系统的 3 个主要组成部分：

1. 计算 bit 的函数的组合逻辑
2. 存储 bit 的存储器
3. 时钟信号，控制存储器的元素更新

逻辑综合（logic synthesis），根据 HDL 描述生成有效的电路设计。约等于，编译器把高级语言转成汇编语言。

2 个语言：
- Verilog
- VHDL

## bit 级组合电路

a, b

```verilog
bool eq = { a && b } || { !a && !b };
bool xor = { !a && b } || { a && !b };

```

多路复用，3  个输入，其中 s 是控制位

```verilog
bool out = { s && a } || { s && b};
```

## 字（word）级组合

把 bit 级的并连起来。

ALU 算数/逻辑单元，就是一种组合电路

## 存储器 & 时钟控制

### 时钟控制

组合电路，从逻辑上讲，不存储任何信息。

为了产生时序电路 （sequential circuit），也就是有状态并能在这个状态上进行计算，必须引入按位存储信息的设备。

- 时钟寄存器（简称寄存器），存储单个的 bit 或 word。时钟信号控制寄存器加载输入值。
- 随机访问存储器（简称存储器），存储多个 word。例子包括：虚拟存储器系统，寄存器堆。

讲硬件和机器级编程时，寄存器的语义有差异：
1. 硬件寄存器。直接将他的输入和输出线，连接到电路的其他部分。PC（程序计数器）和 CC（条件码寄存器）
2. 机器级编程。寄存器堆的少量可寻址的 word。在 IA32（x86）处理器中，有 8 个（%eax，%ecx 等）

硬件有时可以直接将一个字从一个指令传送到另一个指令。避免先写寄存器堆再读出的延迟。

### 硬件寄存器

硬件寄存器，电路不同部分中的组合逻辑之间的屏障。只有在时钟上升沿时，值才会从寄存器的输入传送到输出。

### 程序寄存器

寄存器堆有 2 个读端口，1 个写端口。

多端口随机访问存储器允许同时进程多个读和写操作。即，读 2 个寄存器，写第 3 个寄存器。（应该是需要流水线 CPU 才能做到）

时钟信号按照类似时钟寄存器一样的方式控制向寄存器堆写入字。

## References

1.