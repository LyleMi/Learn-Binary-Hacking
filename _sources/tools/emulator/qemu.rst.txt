QEMU
========================================

背景
----------------------------------------
QEMU是一款开源的仿真执行工具，可以模拟运行多种CPU架构的程序或系统。在执行时，QEMU会先以基本块（Basic Block）为单位，将目标指令经由TCG这一层翻译成宿主机的代码，得到TB(Translation Block)，最终在主机上执行。

和虚拟化不同的是，仿真基本都是软件实现，宿主机和虚拟机可以是不同的架构。

架构
----------------------------------------
QEMU 支持两种操作模式：用户模式仿真和系统模式仿真。用户模式仿真 允许一个 CPU 构建的进程在另一个 CPU 上执行（执行主机 CPU 指令的动态翻译并相应地转换 Linux 系统调用）。系统模式仿真 允许对整个系统进行仿真，包括处理器和配套的外围设备。

在 x86 主机系统上仿真 x86 代码时，使用 QEMU 加速器 可以实现近似本地的性能。这让我们能够直接在主机 CPU 上执行仿真代码（在 Linux 上通过 kernel 模块执行）。

但是从技术角度看，QEMU 的有趣之处在于其快速、可移植的动态翻译程序。动态翻译程序 允许在运行时将用于目标（来宾）CPU 的指令转换为用于主机 CPU，从而实现仿真。这可以通过一种强制方法实现（将指令从一个 CPU 映射到另一个 CPU），但是情况并非总是这样简单，在某些情况下，根据所翻译的架构，可能需要使用多个指令或行为更改。

QEMU 实现动态翻译的方法是，首先将目标指令转换为微操作。这些微操作是一些编译成对象的 C 代码。然后构建核心翻译程序。它将目标指令映射到微操作以进行动态翻译。这不仅可产生高效率，而且还可以移植。

QEMU 的动态翻译程序还缓存了翻译后的代码块，使翻译程序的内存开销最小化。当初次使用目标代码块时，翻译该块并将其存储为翻译后的代码块。 QEMU 将最近使用的翻译后的代码块缓存在一个 16 MB 的块中。 QEMU 甚至可以通过在缓存中将翻译后的代码块变为无效来支持代码的自我修改。

外围设备
----------------------------------------
将 QEMU 作为 PC 系统仿真器使用可提供各种外围设备。需要的标准外围设备包括硬件 Video Graphics Array (VGA) 仿真器、PS/2 鼠标和键盘、电子集成驱动器（Integrated Drive Electronics）硬盘和 CD-ROM 接口，以及软盘仿真。另外，QEMU 包括对 NE2000 Peripheral Controller Interconnect (PCI) 网络适配器、串行端口、大量的声卡和 PCI Universal Host Controller Interface (UHCI) Universal Serial Bus (USB) 控制器（带虚拟 USB 集线器）的仿真。Processor symmetric multiprocessing (SMP) 支持也得到了对 255 个 CPU 的支持。

除了仿真标准 PC 或 ISA PC（不带 PCI 总线）外，QEMU 还可以仿真其他非 PC 硬件，如 ARM Versatile 基线板（使用 926E）和 Malta million instructions per second (MIPS) 板。对于各种其他平台，包括 Power Macintosh G3 (Blue &amp; White) 和 Sun-4u 平台，都能正常工作。

源码结构
----------------------------------------
主要文件

- softmmu/vl.c
    - 最主要的模拟循环，虚拟机环境初始化，和 CPU 的执行。
- target/<arch>/translate.c
    - 将 guest 代码翻译成不同架构的 TCG 操作码。
- tcg/tcg.c
    - 主要的 TCG 代码。
- tcg/<arch>/tcg-target.c
    - 将 TCG 代码转化生成主机代码。
- accel/tcg/cpu-exec.c
    - 寻找下一个二进制翻译代码块，如果没有找到就请求得到下一个代码块，并且操作生成的代码块。

参考链接
----------------------------------------
- `qemu <https://www.qemu.org/>`_
- `qemu wiki <https://wiki.qemu.org/>`_
- `qemu source code on github <https://github.com/qemu/qemu>`_
- `QEMU, a Fast and Portable Dynamic Translator <https://static.usenix.org/event/usenix05/tech/freenix/full_papers/bellard/bellard.pdf>`_
- `使用 QEMU 进行系统仿真 <https://www.ibm.com/developerworks/cn/linux/l-qemu/index.html>`_
- `QEMU 信息泄露漏洞 CVE-2015-5165 分析及利用 <https://programlife.net/2020/06/30/cve-2015-5165-qemu-rtl8139-vulnerability-analysis/>`_
