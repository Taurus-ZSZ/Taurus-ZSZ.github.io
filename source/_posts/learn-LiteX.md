---
title: 认识 LiteX
date: 2021-10-07 05:01:01
tags:
      - LiteX
      - SOC
categories:
      - FPGA
---

> 目录

[TOC]

# 认识 LiteX

LiteX [GitHub项目地址](https://github.com/enjoy-digital/litex)

LiteX [Wiki文档](https://github.com/enjoy-digital/litex/wiki)

LiteX [Community](https://github.com/litex-hub)







LiteX 框架为创建 FPGA 内核/SoC、探索各种数字设计架构和创建完整的基于 FPGA 的系统提供了一个方便高效的基础设施。

LiteX 提供了轻松创建 FPGA 内核/SoC 所需的所有通用组件：

- ✔️ Buses and Streams (Wishbone, AXI, Avalon-ST) and their  interconnect.
- ✔️ Simple cores: RAM, ROM, Timer, UART, JTAG, etc….
- ✔️ Complex cores through the ecosystem of cores: [LiteDRAM](https://github.com/enjoy-digital/litedram), [LitePCIe](https://github.com/enjoy-digital/litepcie), [LiteEth](https://github.com/enjoy-digital/liteeth), [LiteSATA](https://github.com/enjoy-digital/litesata), etc...
- ✔️ Various CPUs & ISAs: RISC-V, OpenRISC, LM32, Zynq, X86 (through a PCIe), etc...
- ✔️ Mixed languages support with VHDL/Verilog/(n)Migen/Spinal-HDL/etc... integration capabilities.
- ✔️ Powerful debug infractrusture through the various [bridges](https://github.com/enjoy-digital/litex/wiki/Use-Host-Bridge-to-control-debug-a-SoC) and [Litescope](https://github.com/enjoy-digital/litescope).
- ✔️ Direct/Fast simulation through [Verilator](https://www.veripool.org/verilator/).
- ✔️ Build backends for open-source and vendors toolchains.
- ✔️ And a lot more... :)

通过将 LiteX 与内核生态系统相结合，创建复杂的 SoC 变得比使用传统方法容易得多，同时提供更好的可移植性和灵活性：例如，基于  VexRiscv-SMP CPU、LiteDRAM、LiteSATA 构建的多核 Linux Capable SoC 和与 LiteX  集成，在廉价的 Acorn CLE215+ 挖矿板上运行：



## 安装 LiteX 环境


### ubuntu 安装 Litex
```shell
#安装环境：Python3.6+
#安装 python3-setuptools
apt-get install python3-setuptools
#安装python  pip
  ##使用脚本安装pip
      ### 下载get-pip.py 脚本
      wget  https://bootstrap.pypa.io/get-pip.py
      ### 改变执行权限
      chmod +x ./get-pip.py
      ### 运行安装pip
      python get-pip.py
  ## 使用包管理器安装
     apt-get install python3-pip
#安装LiteX 

#1、Install Python 3.6+ and FPGA vendor's development tools and/or Verilator.
#2、Install Migen/LiteX and the LiteX's cores:

https://raw.githubusercontent.com/enjoy-digital/litex/master/litex_setup.py
chmod +x litex_setup.py
./litex_setup.py init install --user (--user to install to user directory)

##Later, if you need to update all repositories:
./litex_setup.py update

#3、Install a RISC-V toolchain (Only if you want to test/create a SoC with a CPU):
pip3 install meson
./litex_setup.py gcc

添加环境变量：
vim  ~/.bashrc
将下面的这一行加到最后
export PATH=$PATH:/opt/python_fpga_tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin

如果出现下面的这个错误
Could not detect Ninja v1.8.2 or newer
安装 ninja
pip3 install ninja



#4、Build the target of your board...:

	Go to litex-boards/litex_boards/targets and execute the target you want to build.


#5、    ... and/or install Verilator and test LiteX directly on your computer without any FPGA board:
##On Linux (Ubuntu):
sudo apt install libevent-dev libjson-c-dev verilator
lxsim --cpu-type=vexriscv
```

### Archlinux 安装 Litex
```shell
#安装环境：Python3.6+
#安装 python3-setuptools
pacman -S python-setuptools
#这条命令安装后，执行./litex_setup.py init install --user 时出错。 又试了一遍又可以了！！！！
#也可以通过安装pip使用pip install setuptools 后成功安装。
#安装python  pip
  ##使用脚本安装pip
      ### 下载get-pip.py 脚本
      wget  https://bootstrap.pypa.io/get-pip.py
      ### 改变执行权限
      chmod +x ./get-pip.py
      ### 运行安装pip
      python get-pip.py
  ## 使用包管理器安装
  pacman -S python-pip
#安装LiteX 

#1、Install Python 3.6+ and FPGA vendor's development tools and/or Verilator.
#2、Install Migen/LiteX and the LiteX's cores:

https://raw.githubusercontent.com/enjoy-digital/litex/master/litex_setup.py
chmod +x litex_setup.py
./litex_setup.py init install --user (--user to install to user directory)

##Later, if you need to update all repositories:
./litex_setup.py update


#4、Build the target of your board...:

	Go to litex-boards/litex_boards/targets and execute the target you want to build.

#3、Install a RISC-V toolchain (Only if you want to test/create a SoC with a CPU):
pip3 install meson
./litex_setup.py gcc

#5、    ... and/or install Verilator and test LiteX directly on your computer without any FPGA board:
##On Linux (Ubuntu):
sudo apt install libevent-dev libjson-c-dev verilator
lxsim --cpu-type=vexriscv
```

未完待续~~






## 教程资源

资源链接：https://github.com/enjoy-digital/litex/wiki/Tutorials-Resources

### LiteX教程

#### [FPGA 101](https://github.com/litex-hub/fpga_101)

为学生提供的课程/实验室通过动手方法发现 FPGA 并学习 Migen/LiteX。这些教程通过常见的 SoC 内核涵盖了 Migen  的基础知识（语法/模拟）：时钟生成、7 段显示等......然后通过在 SoC 中集成这些内核来介绍 LiteX  的基础知识。它演示并解释了如何从主机 PC 通过桥接器 (UART) 控制这些内核，然后直接从 FPGA 中实现的 RISC-V 软 CPU  控制这些内核。  这些教程应帮助用户了解 Migen/LiteX 的可能性，并提供创建自己的 Migen 内核/LiteX SoC 的基础知识。      

注意：这些实验室基于 Nexys4DDR（现命名为 NexysA7），但可以轻松适应其他板。由于编写了这些教程，新的较便宜的开源  FPGA 板也与开源 FPGA 工具链兼容，广泛可用。这些教程可能会在未来进行调整以支持此类板。







Xilinx Vivado

吐槽vivado之LiteX vs. Vivado: First Impressions :https://www.bunniestudios.com/blog/?p=5018

Vivado 之所以强大，是因为我不必在 Verilog 中编写复杂的 SoC，而是可以使用他们的伪 GUI/TCL  接口来创建框图，从而在很大程度上自动化构建 AXI 路由结构的任务。此外，我可以访问 Xilinx 广泛的 IP 库，其中包括一个非常灵活的  DDR 内存控制器和一个经过严格审查的 PCI-express 控制器。由于这种设计自动化水平和可用 IP，仅使用 Verilog  可能需要数月才能完成的任务在 Vivado 的帮助下可以在几天内完成。

Vivado 的缺点是它不是开源的（可以免费下载，但不能免费修改），而且它的效率和速度不是特别好。除了对 Vivado  的封闭源代码性质的意识形态上的反对之外，缺乏源代码访问还有一些实际的、务实的影响。在高层次上，赛灵思通过销售  FPGA——硅芯片来赚钱。然而，为了吸引设计成功，他们必须提供设计工具和 IP 生态系统。该软件的开发由芯片的销售直接补贴。

当涉及到工具的效率时，这会产生有趣的利益冲突——也就是说，它们在优化设计以消耗尽可能少的硅方面有多好。花钱创建面积高效的工具会减少收入，因为这会鼓励客户购买更便宜的芯片。

因此，Vivado 工具在优化面积设计方面非常糟糕。例如，即使您不使用该接口，PCI express  内核（虽然可配置性极强且经过严格审查）也无法关闭 AXI  从桥。即使输入未连接或接地，逻辑优化器也不会删除未使用的门。不幸的是，这块死逻辑消耗了我的目标 FPGA 容量的 20%  左右。我只能通过手动编辑机器生成的 VHDL  来注释掉从桥来回收该空间。这是一件足够简单的事情，并且对内核的功能没有负面影响。但赛灵思没有动力添加 GUI  开关来禁用逻辑，因为如果您的设计使用 PCI express 内核，额外的门会鼓励您“升级”一个 FPGA 尺寸。同样，DDR3 内存核心将其  70% 的大量占用空间用于“校准”块。校准通常只在启动时运行一次，因此逻辑在正常操作期间处于空闲状态。使用 FPGA  时，明智的做法是运行校准、存储值，然后将预先测量的值塞入应用程序设计中，从而消除校准块的开销。但是，我无法实现此优化，因为 DDR3  块是作为不透明网表提供的。最后，AXI 结构自动化（虽然很神奇）在端口数量上的扩展性很差。在我最近使用 Vivado 完成的基准设计中，50%  的芯片用于路由结构，25% 用于 DDR3 块，其余用于我的实际应用逻辑。

Tim 提到他认为在使用 LiteX 时相同的设计将适合更小的 FPGA。他一直在使用 LiteX 生成  FPGA“门件”（比特流）以支持他在各种平台上的 HDMI2USB 视频处理管道，从 Numato-Opsis 到 Atlys，他甚至为  NeTV2 启动了一个端口。出于好奇，我决定将我的 Vivado 设计之一移植到 LiteX，以便我可以对两种设计流程进行逐个比较。

LiteX 是 Migen/MiSoC 的软分叉——一个基于 Python 的框架，用于管理硬件 IP 和自动生成 HDL。 LiteX 中的  IP 模块是完全开源的，因此可以针对多个 FPGA  架构。然而，对于低级综合、布局布线和比特流生成，它仍然依赖于专有的芯片特定供应商工具，例如面向 Artix FPGA 时的  Vivado。它有点像一个吐出汇编的开源 C 编译器，因此它仍然需要特定于供应商的汇编器、链接器和  binutils。虽然在汇编程序之前打开编译器似乎有些落后，但请记住，对于软件而言，汇编程序的工作范围很简单——主要是在定义良好的 32  位左右的操作码中。然而，对于 FPGA  而言，“汇编器”（布局布线工具）的任务是确定在实际上有几百万位长的“操作码”中放置一位原语的位置，每一位之间都有潜在的交叉依赖性。抽象层虽然是并行的，但不能直接比较。

让我用我与 Python 爱恨交加的声明来开始我的体验。我曾多次将 Python 用于“娱乐”项目和小工具，以及驱动一些自动化框架。但我发现  Python  非常令人沮丧。如果您可以从头开始使用他们的框架，那将是直观、有趣，甚至是强大的。但是如果你的应用程序天生就不是“Pythonic”，那你就惨了。而且我有很多需要进行位碰撞、操作二进制文件或处理低级硬件寄存器，这些活动绝对不是 Pythonic。我也花了很多时间与 Python 类型系统和语法的“可爱”作斗争：我更像是一个 Rust  人。我喜欢严格类型的语言。我不喜欢使用“-1”作为最后一个元素数组索引和使用魔术方法重载二元运算符之类的新奇事物。

令人惊讶的是，我能够在一天内启动并运行 LiteX。这在很大程度上要归功于 Tim 努力创建一个真正全面的引导脚本，该脚本检查 git  存储库、所有子模块（谢谢！），并管理您的构建环境。它刚刚奏效；我遇到的唯一问题是关于安装 Xilinx 工具链的一些不一致的文档（对于  Artix 构建，您需要获取 Vivado；而 Spartan 则需要获取 ISE）。整个过程占用了大约 19GiB 的硬盘空间，其中  18GiB 是 Vivado 工具链。

我得到了一个用于定义 SoC 的令人惊讶的强大和成熟的框架。由于 MiSoC 和 LiteX 人群的大量工作，已经有用于 DRAM、PCI  express、以太网、视频、软核 CPU（您选择 or1k 或 lm32）等的 IP  核。公平地说，我无法将它们加载到真实硬件上并验证它们的规范合规性或功能，但它们似乎可以编译为正确的原语，因此它们具有正确的形状和大小。他们没有使用 AXI，而是将 Wishbone 用于面料。我还不清楚 MiSoC 结构生成器的带宽效率如何，但它已经用于将 4x HDMI 连接路由到  Numato-Opsis 上的 DRAM 的事实表明它为我的应用程序提供了足够的马力（这只需要3 个 HDMI 连接）。

作为一个高级框架，它非常神奇。大型 IP 实例和相应的总线端口是按需分配的，基于 Python  中非常高级的描述。我感觉有点像一个蹒跚学步的孩子，他在安全关闭的情况下收到了一把上膛的枪。我祈祷底层正在做出理智的推论。但是，至少在 LiteX 的情况下，如果我不同意这些决定，它已经足够开源，我可以尝试修复问题，假设我有时间和勇气这样做。

对于我的工具流程比较，我在 Vivado 和 LiteX 中实现了一个简单的 2x HDMI-in 到 DDR3 到 1x HDMI-out  设计。创建设计在两个流程上的工作量大致相同——一旦您拥有基本的 IP  块，在每种情况下，实例化总线结构和寻址分配在很大程度上都是自动化的。由于其图形规划工具，Vivado  在引脚/封装布局方面更胜一筹（我发现封装布局的图示比球栅坐标的文本列表直观得多），而 LiteX 的设计创建速度要快一些，尽管我对 Python 的常见挫折（取决于读者的偏见，决定是我有不同的看待事物的方式，还是我的智力不足以完全理解 Python 的优点）。

但从那里开始，两者之间的经验迅速分歧。让我对 LiteX 感到兴奋的主要事情是其高级综合的速度和效率。**LiteX 产生的设计使用了大约 20% 的 XC7A50 FPGA，运行时间约为 10 分钟，而 Vivado 产生的设计消耗了相同 FPGA 的 85%，运行时间约为 30-45  分钟。**

值得注意的是，LiteX 倾向于“快速失败”，因此语法错误或配置的小问题在几秒钟内（如果不是几分钟）就会变得明显。然而，Vivado  往往会“迟到失败”——一个小的配置问题可能要到运行大约 20  分钟后才会出现，因为它管理脱离上下文的块综合和构建依赖项的方式很笨拙。这意味着，尽管我对 Python  语法感到沮丧，但在时间方面为小错误付出的代价要少得多——所以总的来说，我的工作效率更高。

但真正引人注目的是效率。 LiteX 生成更高效的 HDL 的事实意味着我可以通过使用更小的 FPGA  来从设计中节省大量成本。请记住，LiteX 和 Vivado  都使用相同的后端进行低级综合以及布局和布线。不同之处完全在于高级设计自动化——我认为这是一个非常适合基于 Python  的框架的级别。你并不是真正用 Python 设计硬件（最终它都变成了 Verilog），而是管理和配置 IP 库，这是 Python  非常适合的东西。也就是说，我在 MiSoC 库中挖掘了一些，似乎有一些使用这种 Python  语法的严肃逻辑设计。我不确定我是否想围绕这种编码风格，但好消息是我仍然可以在 Verilog 中编写叶单元并从高级 Python  集成框架中调用它们。

因此，我谨慎地继续使用 LiteX 作为 NeTV2  未来的主要设计流程。一旦我的下一代硬件可用，我们将看到比特流如何在时序和功能方面得到证明，但我很乐观。我对调试的工作方式有一些担忧——我发现  Xilinx ILA 内核是非常强大的工具，并且能够自动将任何复杂设计逆向工程到原理图（Vivado  中内置的一项功能）有助于查找时序和逻辑错误。但是有了内置的软 CPU 内核、“LiteScope”逻辑分析器（即将支持  sigrok）和快速的构建时间，我觉得有足够的机会在 LiteX 中开发新的、甚至更强大的方法来跟踪解决棘手的错误。

我的最后一个想法是  LiteX，在目前的状态下，可能最适合那些受过编写软件培训并想要设计硬件的人，而不是那些受过经典电路设计培训但想要工具升级的人。 LiteX  中内置的设计习惯用法和直觉强烈地借鉴了软件设计师的实践，这意味着许多“显而易见”的事情没有记录在案，这会让局外人（例如像我这样的硬件设计师）陷入困境。毫无疑问，设计流程的功能和实用性——因此，随着工具链的成熟和文档的改进，我乐观地认为这可能成为各种规模硬件项目的流行设计流程。



Interested? Tim has suggested the following links for further reading:

- [Getting started](https://github.com/timvideos/HDMI2USB-litex-firmware/blob/master/getting-started.md) with LiteX
- Tim ‘mithro’ Ansell’s [talk at Linux.conf.au 2017 about why he finds LiteX / MiSoC so powerful](http://j.mp/pyhw-pycon2017).
- [Migen Manual](https://m-labs.hk/migen/manual/)
- Tim ‘mithro’ Ansell (and friends) are running [a day long tutorial on using LiteX and Linux together at Linux.conf.au 2018](https://linux.conf.au/programme/miniconfs/fpga/) – If you are in Australia, I recommend checking it out. I’ll be speaking at the full conference too!
- [Cr1901’s tutorial for adding a new FPGA board to Migen](https://www.wdj-consulting.com/blog/migen-port.html)
- [M-Lab’s the creators of Migen + MiSoC](http://m-labs.hk/gateware.html) & [Enjoy Digital, creators of LiteX and the other Lite cores](http://enjoy-digital.fr/cores.html)
- [Implementing a simple SoC using Migen and Project IceStorm (the fully open source FPGA toolchain for the Ice40)](https://lab.whitequark.org/notes/2016-10-19/implementing-a-simple-soc-in-migen/)

Tags: [fpga](https://www.bunniestudios.com/blog/?tag=fpga), [netv2](https://www.bunniestudios.com/blog/?tag=netv2), [open source](https://www.bunniestudios.com/blog/?tag=open-source)



[HDMI2USB video processing pipelines](https://github.com/mithro/HDMI2USB-litex-firmware/)



https://blog.mithis.net/

 [really comprehensive bootstrapping script](https://github.com/mithro/HDMI2USB-litex-firmware/blob/master/scripts/bootstrap.sh)





## 相关链接

[liteX](https://github.com/enjoy-digital/litex)

[litex-hub](https://github.com/litex-hub)	

[litex-lesson](https://github.com/litex-hub/fpga_101)	

[litex Wiki](https://github.com/enjoy-digital/litex/wiki)

https://osda.gitlab.io/

