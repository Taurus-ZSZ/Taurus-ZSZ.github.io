---
title: migen-notes
date: 2021-10-06 01:40:18
tags:
      - python
      - soc
categories:
      - FPGA
---

> 目录

[TOC]

# Migen 学习笔记

> Migen是什么？？？

Migen 是一种基于 Python 的工具，可进一步自动化 VLSI 设计过程。

Migen 是 MiSoC 的基础。



## 介绍

&ensp;&ensp;Migen是一个基于Python的工具，目的是实现自动化VLSI设计过程。

**VLSI: 超大规模集成电路**（英语：very-large-scale integration，[缩写](https://zh.wikipedia.org/wiki/縮寫)：VLSI

Migen使面向对象和元编程设计硬件的现代编程模式成为可能！这个结果使设计更加简单与优雅，并且可以减少人为错误的发生。

### 背景

虽然Milkymist 片上系统有 [mm](http://m-labs.hk)取得了许多成功，但由于在手动编写的 Verilog HDL 中的实现，它仍存在一些限制：

……



### 安装

github 地址：https://github.com/m-labs/migen

下载项目：

```text
Either run the setup.py installation script or simply set PYTHONPATH to the root of the source directory.

If you wish to contribute patches, the suggest way to install is;

        Clone from the git repository at http://github.com/m-labs/migen
        Install using python3 ./setup.py develop --user
        Edit the code in your git checkout.


```

解压到一个文件夹，

使用powershell在该文件夹下打开，运行：要求使用python3.6+

```powershell
pyhton ./setup.py develop --user

#这样在windows下就安装好了
```

## 特定于FHDL域的语言

这个Fragmented Hardware Description Language (FHDL)是Migen的基础。

它由一个描述信号的正式系统，以及对它们进行操作的组合语句和同步语句组成。



**MyHDL**

MyHDL遵循传统的HDL的事件驱动；而FHDL将代码分成组合语句、同步语句和重置语句。

FHDL 由几个元素组成，下面将对其进行简要说明。它们都可以直接从 migen 模块导入。

### 表达式

#### 常量 constants

这个`Constant`对象被解释为常量。他可以指定整型和布尔型，它也支持切片和指定位宽，并且可以表明符号。

明确的支持负数，跟MyHDL一样，算数操作返回==自然==结果

```python
True 	#被解释为1
False 	#被解释为0
```

为了简化语法，赋值和运算符会自动将 Python 整数和布尔值包装到 Constant 中。此外，Constant 是 C 的别名。以下是有效的 Migen 语句：

```python
a.eq(0), a.eq(a + 1), a.eq(C(42)[0:1])
```

#### 信号 Signal

signal 对象表示电路中预期会发生变化的值。他完全符合Verilog中的wire 和reg 和VHDL中的signal的作用。

信号对象的要点是它由它的 Python ID（由 id() 函数返回）标识，没有别的。

**信号对象的属性是：**

1. 一个整数或一个（integer，boolean）对，用于定义位数以及信号的较高索引位是否为符号位（即信号有符号）。默认值为一位且无符号。或者，可以指定 min 和 max 参数来定义信号的范围并确定其位宽和符号。与 Python 范围一样，min 是包含的，默认为 0，max 是不包含的，默认为 2
2. name，用作 V*HDL 后端名称管理器的提示。
3. 信号的复位值。它必须是一个整数，默认为0。当用同步语句修改信号的值时，复位值是关联寄存器的初始化值。当在条件组合语句（If 或  Case）中分配信号时，重置值是当没有验证导致信号被驱动的条件时信号具有的值。这强制设计中没有锁存器。如果使用组合语句永久驱动信号，则重置值无效。

name 属性的唯一目的是使生成的 V*HDL 代码更易于理解和调试。从纯粹的功能角度来看，拥有多个具有相同名称属性的信号是完全可以的。后端将为每个对象生成一个唯一的名称。如果未指定 name 属性，Migen 将分析创建信号对象的代码，并尝试从中提取变量或成员名称。例如，以下语句将创建一个或多个名为“bar”的信号：

```python
bar = Signal()
self.bar = Signal()
self.baz.bar = Signal()
bar = [Signal() for x in range(42)]
```

如果发生冲突，Migen 首先尝试通过使用创建它们的类和模块层次结构中的名称作为标识符前缀来解决这种情况。如果冲突仍然存在（如果在同一上下文中创建了两个具有相同名称的信号对象，就会出现这种情况），它最终会添加数字后缀。

#### 运算符 Operators

运算符由 **_Operator** 对象表示，一般不应直接使用。相反，大多数 FHDL 对象重载了通常的 Python 逻辑和算术运算符，这允许使用更轻的语法。例如，表达式：

```python
a * b + c
#上式等价于
_Operator("+",[_Operator("*",[a,b]),c])
```



#### 切片 Slices

同样的，切片由***`_Slice`***对象表示,同样，通常不应该使用它来支持 Python 切片操作 [x:y]。支持使用 [x]、[x:] 和 [:y]  形式的隐式索引。谨防！切片的工作方式类似于 Python 切片，而不是 VHDL 或 Verilog 切片。第一个界限是 LSB  的索引并且是包含的。第二个界限是 MSB 的索引，是唯一的。在 V*HDL 中，界限是 MSB:LSB 并且两者都包含在内。



#### 连接 Concatenations

连接是使用 Cat 对象完成的。为了使语法更简洁，它的构造函数采用可变数量的参数，这些参数是要连接在一起的信号（您可以使用 Python  的“*”运算符来代替传递列表）。为了与切片一致，第一个信号连接到结果中具有最低索引的位。这与 Verilog 中“{}”构造的工作方式相反。

==跟Verilog中的{}拼接语法功能一致==

#### 复制 Replications

Replicate 对象表示==相当于 Verilog 中的 {count{expression}}==。例如，表达式：



```python
Replicate(0, 4)
#上式等价于
cat(0,0,0,0)
```



### 声明 Statements

#### 分配 Assignment

分配由 ***_Assign*** 对象表示。由于直接使用它会导致语法混乱，因此首选的赋值技术是使用可以为其分配值的对象提供的 ==eq() 方法==。它们是信号，以及它们与切片和连接运算符的组合。例如，声明：

```python 
a[0].eq(b)
#上式等价于
_Assign(_Slice(a,0,1),b)

```

#### If

If 对象需要第一个参数，该参数必须是表示条件的表达式（Constant、Signal、_Operator、_Slice 等对象的组合），然后是表示语句（_Assign、If、Case 等）的可变数量的参数。对象）在条件得到验证时执行。

If 对象定义了一个 Else() 方法，该方法在调用时定义了在条件不为真时要执行的语句。这些语句作为参数传递给可变参数方法。

为方便起见，还有一个 Elif() 方法。

Example:

```python
If(tx_count16 == 0,
  tx_bitcount.eq(tx_bitcount + 1),
  If(tx_bitcount == 8,
    self.tx.eq(1)
  ).Elif(tx_bitcount == 9,
        self.tx.eq(1),
        tx.busy.eq(0)
  ).Else(
  	  self.tx.eq(tx_reg[0]),
      tx_reg.eq(Cat(tx_reg[1:],0))
  )
)
```

#### Case

Case 对象构造函数将要测试的表达式和一个字典作为第一个参数，其键是要匹配的值，以及在匹配的情况下要执行的语句的值。特殊值“default”可以用作匹配值，这意味着只要没有其他匹配项就应该执行语句。
```python
cases = {
            0x1: abcdefg.eq(0b0000110),
            0x2: abcdefg.eq(0b1011011),
            0x3: abcdefg.eq(0b1001111),
            0x4: abcdefg.eq(0b1100110),
            0x5: abcdefg.eq(0b1101101),
            0x6: abcdefg.eq(0b1111101),
            0x7: abcdefg.eq(0b0000111),
            0x8: abcdefg.eq(0b1111111),
            0x9: abcdefg.eq(0b1101111),
            0xa: abcdefg.eq(0b1110111),
            0xb: abcdefg.eq(0b1111100),
            0xc: abcdefg.eq(0b0111001),
            0xd: abcdefg.eq(0b1011110),
            0xe: abcdefg.eq(0b1111001),
            0xf: abcdefg.eq(0b1110001),
        }

        # Combinatorial assignement
        self.comb += Case(value, cases)
```



#### 数组 Arrays

Array 对象表示可以由 FHDL 表达式索引的其他对象的列表。明确可以：

- 嵌套 Array 对象以创建多维表。
- 列出 Array 中的任何 Python 对象，只要出现在模块中的每个表达式最终都对索引的所有可能值计算为 Signal。这允许创建结构化数据列表。
- 在两个方向（赋值和读取）使用涉及 Array 对象的表达式。

For example, this creates a 4x4 matrix of 1-bit signals:

```python
my_2d_array = Array(Array(Signal() for a in range(4)) for b in range(4))
```

然后，您可以使用（x 和 y 是 2 位信号）读取矩阵：

```python
读取矩阵：
out.eq(my_2d_array[x][y])
#and write it with:
my_2d_array[x][y].eq(inp)
```

由于它们在 Verilog 中没有直接等价物，因此在实际转换发生之前，Array 对象被降低到多路复用器和条件语句中。这种降低自动发生，无需任何用户干预。  

对 Array 对象执行的任何越界访问都将引用最后一个元素。



### Specials

#### 三态IO Tri-state I/O

定义三态 I/O 端口的单向信号的三元组 (O, OE, I) 由 TSTriple 对象表示。此类对象只是用于稍后连接到三态 I/O 缓冲区的信号的容器，==不能用作模块专用。==但是，此类对象应尽可能长时间地保留在设计中，因为它们允许以明确的方式处理单个单向信号。

可以用作特殊模块的对象是 Tristate，它的行为与三态 I/O 缓冲区的实例完全一样，定义如下：

```python
Instance("Tristate",
        io_target=target,
        i_o=o,
        i_oe=oe,
        o_i=i
        )
```

信号目标，<font color=red>o </font>和 <font color=red>i  </font>可以有任何宽度，而<font color=red>oe </font>是 1 位宽。目标信号应该进入一个端口，而不是在设计的其他地方使用。与现代 FPGA 架构一样，Migen 不支持内部三态。

可以通过调用 get_tristate 方法从 TSTriple 对象创建 Tristate 对象。

默认情况下，Migen 为三态缓冲区发出与技术无关的行为代码。如果需要特定代码，可以使用 V*HDL 转换函数的适当参数覆盖三态处理程序。

#### Instances 实例

实例对象代表 V*HDL 模块的参数化实例，以及其端口与 FHDL 信号的连接。它们在许多情况下很有用：

- 重用旧版或第三方 V*HDL 代码。
- 使用特殊的 FPGA 功能（DCM、ICAP 等）
-  实现无法用 FHDL 表达的逻辑（例如锁存器）
-  将 Migen 系统分解为多个子系统。

实例对象构造函数采用实例的类型（即实例化模块的名称），然后是描述如何连接和参数化实例的多个参数。

**这些参数可以是：**

- <font color=red>Instance.Input</font>、<font color=red>Instance.Output</font> 或 <font color=red>Instance.InOut</font> 来描述与实例的信号连接。参数是实例中端口的名称，以及它应该连接到的 FHDL 表达式。 
- <font color=red>Instance.Parameter</font> 设置实例的参数（带有名称和值）。
-  <font color=red>Instance.ClockPort </font>和 <font color=red>Instance.ResetPort </font>用于将时钟和复位信号连接到实例。唯一的强制参数是实例端口的名称。或者，可以指定时钟域名，并且可以使用<font color=red> invert </font>选项连接到那些需要 180 度时钟或低电平有效复位的模块。

#### Memories 存储器

被支持使用类似于实例的机制支持Memories（on chip SRAM）。

Memory对象有以下参数：

- 宽度，即每个字中的位数。
- 深度，表示内存中的单词数。
- 用于初始化内存的可选整数列表。

要访问硬件中的内存，可以通过调用 get_port 方法获取端口。一个端口总是有一个地址信号a和一个数据读取信号dat_r。根据端口的配置，其他信号可能可用。

**get_port 的选项是：**

- write_capable（默认值：False）：如果端口可用于写入内存。这会产生一个额外的 we 信号。 
- async_read（默认值：False）：读取是异步（组合）还是同步（注册）。 
- has_re（默认值：False）：添加读取时钟使能信号 re（异步端口忽略）。 
- we_granularity（默认值：0）：如果非零，则可能发生小于内存字的写入。 
- we 信号的宽度增加以用作子字的选择信号。 
- 模式（默认值：WRITE_FIRST，异步端口忽略）。有可能：
  - READ_FIRST：在写入期间，读取前一个值。   
  - WRITE_FIRST：返回写入的值。     
  - NO_CHANGE：数据读取信号在写入时保持其先前值。  
- clock_domain（默认值：“sys”）：用于从此端口读取和写入的时钟域。

Migen 生成的行为 V*HDL 代码应该与所有模拟器兼容，如果端口数 <= 2，则大多数 FPGA 合成器。如果需要特定代码，可以使用 V*HDL 转换函数的适当参数覆盖内存处理程序。

### 模块 Modules

模块的作用与 Verilog 模块和 VHDL 实体相同。同样，它们以树状结构组织。 FHDL 模块是派生自 Module 类的 Python 对象。这个类定义了派生类使用的特殊属性来描述它们的逻辑。它们在下面解释。

#### 组合语句 Combinatorial statements

组合语句是在其输入之一发生变化时执行的语句。  组合语句通过使用 comb 特殊属性添加到模块中。像大多数模块特殊属性一样，它必须使用 += 递增运算符访问，并且单个语句、语句元组或语句列表可以出现在右侧。  例如，下面的模块实现了一个或门：

```python
class ORGate(Module):
  def __init__(self):
    self.a = Signal()
    self.b = Signal()
    self.x = Signal()

    ###

    self.comb += self.x.eq(self.a | self.b)
```

为了提高代码可读性，建议将模块的接口放在 __init__ 函数的开头，并使用三个井号将其与实现分开。

#### 时序逻辑 Synchronous statements

同步语句是在某个时钟信号的每个边沿执行的语句。

在一个模块中通过使用<font color=red>sync</font> 指定同步属性，他跟组合逻辑一样，组合逻辑通过使用<font color=red>com</font>来指定组合逻辑的属性。

<font color=red>sync</font> 同步属性可以指定使用的时钟域，比如：向foo时钟域添加一条语句，self.sync.foo += statement。默认的时钟域是==sys==下面的两条语句是等价的：

```python
self.sync += statement
#等价于
self.sync.sys +=statement
```

#### 子模块和特殊项 Submodules and specials

可以分别使用 submodules 和 specials 属性添加子模块和特殊项。这可以通过两种方式完成：

1. 匿名，通过直接在特殊属性上使用 += 运算符，例如self.submodules += some_other_module。与 comb 和 sync 属性一样，可以指定单个模块/特殊或元组或列表。
2. 通过使用子模块或特殊属性的子属性命名子模块/特殊，例如self.submodules.foo =  module_foo。然后可以将子模块/特殊作为对象的属性访问，例如self.foo（而不是  self.submodules.foo）。使用此表单一次只能添加一个子模块/特殊项。

#### 时钟域 Clock domains

指定时钟域的实现是使用<font color=red> ClockDomain </font>对象完成的。它包含时钟域的名称、可以像设计中的任何其他信号一样驱动的时钟信号（例如，使用 PLL 实例），以及可选的复位信号。没有复位信号的时钟域使用例如复位。 Verilog 中的初始语句，在许多 FPGA  系列中，它在配置期间初始化寄存器。

如果可以从变量名称中提取名称，则可以省略名称。使用此自动命名功能时，会删除前缀 _、cd_ 和 _cd_。

然后使用 <font color=red>clock_domains </font>特殊属性将时钟域添加到模块中，该属性的行为与<font color=red>Submodules </font>和<font color=red>specials</font>完全相同。



#### Summary of special attributes

| Syntax                                                       | Action                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| self.comb += stmt                                            | Add combinatorial statement to current module.               |
| self.comb += stmtA, stmtB self.comb += [stmtA, stmtB]        | Add combinatorial statements A and B to current module.      |
| self.sync += stmt                                            | Add synchronous statement to current module, in default clock domain sys. |
| self.sync.foo += stmt                                        | Add synchronous statement to current module, in clock domain foo. |
| self.sync.foo += stmtA, stmtB self.sync.foo += [stmtA, stmtB] | Add synchronous statements A and B to current module, in clock domain foo. |
| self.submodules += mod                                       | Add anonymous submodule to current module.                   |
| self.submodules += modA, modB self.submodules += [modA, modB] | Add anonymous submodules A and B to current module.          |
| self.submodules.bar = mod                                    | Add submodule named bar to current module. The submodule can then be accessed using self.bar. |
| self.specials += spe                                         | Add anonymous special to current module.                     |
| self.specials += speA, speB self.specials += [speA, speB]    | Add anonymous specials A and B to current module.            |
| self.specials.bar = spe                                      | Add special named bar to current module. The special can then be accessed using self.bar. |
| self.clock_domains += cd                                     | Add clock domain to current module.                          |
| self.clock_domains += cdA, cdB self.clock_domains += [cdA, cdB] | Add clock domains A and B to current module.                 |
| self.clock_domains.pix = ClockDomain() self.clock_domains._pix = ClockDomain() self.clock_domains.cd_pix = ClockDomain() self.clock_domains._cd_pix = ClockDomain() | Create and add clock domain pix to current module. The clock domain name is pix in all cases. It can be accessed using self.pix, self._pix, self.cd_pix and self._cd_pix, respectively. |

#### 时钟域管理 Clock domain management

当一个模块命名了定义一个或多个同名时钟域的子模块时，这些时钟域名称的前缀是每个子模块的名称加上下划线。

此功能的一个示例用例是具有两个独立视频输出的系统。每个视频输出模块由定义时钟域pix并驱动时钟信号的时钟发生器模块，加上具有同步语句和时钟域pix中的其他元素的驱动模块组成。视频输出模块的设计者可以简单地使用该模块中的时钟域名pix。在顶层系统模块中，视频输出子模块分别命名为video0和video1。然后 Migen 自动将每个模块的 pix 时钟域重命名为 video0_pix 和  video1_pix。请注意，发生这种情况只是因为时钟域是定义的（使用 ClockDomain  对象），而不是在视频输出模块中简单地引用（使用例如同步语句）。  

当定义时钟域的任何子模块是匿名的时，时钟域名称重叠是一种错误情况。



#### 定稿机制 Finalization mechanism

有时，希望仅在用户完成操作该模块后才创建某些模块逻辑。例如，FSM  模块支持动态定义状态，状态信号的宽度只有在添加所有状态后才能知道。一种解决方案是在 FSM  构造函数中声明最终的状态数，但这对用户不友好。更好的解决方案是在 FSM 模块转换为 V*HDL 之前自动创建状态信号。 Migen  使用所谓的终结机制来支持这一点。

模块可以重载可以创建逻辑并使用以下算法调用的 do_finalize 方法：

1. 当前模块的完成开始。 
2. 如果模块已经完成（例如手动），则程序在此处停止。 
3. 当前模块的子模块被递归确定。 
4. 为当前模块调用 do_finalize。 
5. 由当前模块的 do_finalize 创建的任何新子模块都将递归完成。

在 V*HDL 转换和仿真时自动调用终结。可以通过调用其 finalize 方法为任何模块手动调用它。

上面解释的时钟域管理机制发生在最终确定期间。

### 转换合成 Conversion for synthesis

任何 FHDL 模块都可以转换为可综合的 Verilog HDL。这是通过使用 migen.fhdl.verilog 模块中的 convert 函数来完成的：

```python
# define FHDL module MyDesign here

if __name__ == "__main__":
  from migen.fhdl.verilog import convert
  convert(MyDesign()).write("my_design.v")
```

migen.build 组件提供将第三方 FPGA 工具（来自 Xilinx、Altera 和 Lattice）连接到 Migen 的脚本，以及用于轻松部署设计的电路板数据库。



## 仿真Migen 设计 Simulating a Migen design

Migen 允许您轻松模拟 FHDL 设计并将其与任意 Python 代码连接。该模拟器是用纯 Python 编写的，无需使用外部 Verilog 模拟器即可直接解释 FHDL 结构。

Migen 允许您使用 Python 的生成器函数编写测试平台。此类测试平台与 FHDL 仿真器同时执行，并使用 <font color=red> yield </font> 语句与其通信。有四种基本模式：

1. Reads：可以使用(yield signal)查询当前时间信号的状态；
2.  写入：可以使用 (yield signal.eq(value)) 设置下一个时钟周期后的信号状态；
3.  时钟：模拟可以使用yield提前一个时钟周期；
4.  组成：可以使用`yield from run_other()`将控制权转移到另一个测试平台函数。



可以使用 migen.sim 中的 run_simulation 函数运行测试平台； run_simulation(dut, bench) 根据 FHDL 模块 dut 中定义的逻辑运行生成器函数工作台。

将 vcd_name="file.vcd" 参数传递给 run_simulation 将使其将 dut 内的信号的 VCD 转储写入 file.vcd。

#### Examples

For example, consider this module:

```python
class ORGate(Module):
  def __init__(self):
    self.a = Signal()
    self.b = Signal()
    self.x = Signal()

    ###

    self.comb += self.x.eq(self.a | self.b)
```

It could be simulated together with the following testbench:

```python
dut = ORGate()

def testbench():
  yield dut.a.eq(0)
  yield dut.b.eq(0)
  yield
  assert (yield dut.x) == 0

  yield dut.a.eq(0)
  yield dut.b.eq(1)
  yield
  assert (yield dut.x) == 1

run_simulation(dut, testbench())
```

当然，这非常冗长，可以将各个步骤分解为单独的函数：

```python
dut = ORGate()

def check_case(a, b, x):
  yield dut.a.eq(a)
  yield dut.b.eq(b)
  yield
  assert (yield dut.x) == x

def testbench():
  yield from check_case(0, 0, 0)
  yield from check_case(0, 1, 1)
  yield from check_case(1, 0, 1)
  yield from check_case(1, 1, 1)

run_simulation(dut, testbench())
```

#### 陷阱 Pitfalls

不幸的是，有一些基本错误会产生非常令人费解的结果。  

在调用其他测试平台时，重要的是不要忘记 yield from。如果省略，则调用将无声地执行任何操作。  

写入信号时，重要的是没有其他任何东西应该同时驱动信号。如果不是这种情况，写入将无声地做任何事情。



## 小结

只是大概的了解了Migen,可以使用python开发FPGA，还没有实际使用，处于基础的了解阶段。

# Python FPGA

> 使用python开发FPGA的方法：



1. [Migen](https://m-labs.hk/gateware/migen/) 
2. [MyHDL](http://www.myhdl.org)
3. [Pyverilog](https://github.com/PyHDI/Pyverilog) 



YouTube上的视频资料：

MyHDL:

​		https://www.youtube.com/watch?v=aTi35MMAVLs&t=22s



# litex

关于litex的相内容请参考我的learn-LiteX

[liteX](https://github.com/enjoy-digital/litex)

[litex-hub](https://github.com/litex-hub)	

[litex-lesson](https://github.com/litex-hub/fpga_101)	

[litex Wiki](https://github.com/enjoy-digital/litex/wiki)





































