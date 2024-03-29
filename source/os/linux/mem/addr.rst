地址
========================================

32位内核内存分布
----------------------------------------
::

    +------------------------+
    | Kernel Space           | 1GB
    +------------------------+
    |                        |
    +------------------------+
    | Stack                  |
    | (向下增长)             |
    +------------------------+
    |                        |
    +------------------------+
    | Memory Mapping Segment |
    | (文件映射或匿名映射)   |
    | (向下增长)             |
    +------------------------+
    |                        |
    +------------------------+ program break (brk)
    | Heap                   |
    | (向上增长)             |
    +------------------------+ start_brk
    |                        |
    +------------------------+
    | Bss 段                 |
    +------------------------+ end_data
    | Data 段                |
    +------------------------+ start_data
    +------------------------+ end_code
    | Text 段                |
    +------------------------+ 0x08048000
    |                        |
    +------------------------+ 0

地址定位方式
----------------------------------------
- 固定定位方式
    - 汇编和机器代码一一对应
    - 容易覆盖操作系统
    - 只支持单道程序
- 静态重定位方式
    - 源程序经编译和连接后生产的目标代码的地址是以0为起始地址的相对地址
    - 当需要执行时，由装入程序运行重定位程序模块，根据作业在本次分配到的内存起始地址将可执行代码装如指定内存地址，并修改有关地址部分的值
    - 修改的方式是对每一个逻辑地址的值加上内存区首地址
    - 需要整块的地址
- 动态重定位
    - 在装入内存的时，不修改逻辑地址，访问逻辑地址时，动态的讲逻辑地址变成物理地址
    - 在执行指令时，若设计到逻辑地址，则将该地址送入虚地址寄存器VR，再将BR和VR相加送入地址寄存器MR

虚拟地址转换
----------------------------------------
地址转换负责将虚拟地址转换成物理地址，有多个应用场景。

应用在虚拟内存中，可以给应用程序一种独占整个计算机内存的假象，可以使用超过实际物理大小的内存，应用程序之间互不干扰。

用于进程隔离，可以构建沙盒技术，避免操作系统内核和应用程序受到病毒或者恶意代码的攻击。

进程间通信时，地址转换可以将不同进程空间的地址映射到同一段物理内存，从而实现进程间通信。

加载动态链接库时，可以在多个程序实例之间进行共享。
