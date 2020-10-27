基础
========================================

简介
----------------------------------------
syscall是内核提供用户空间程序与内核空间进行交互的一套标准接口，这些接口让用户态程序能受限访问硬件设备，比如申请系统资源、操作设备读写、创建新进程等。用户空间发起请求，内核空间负责执行，这些接口便是用户空间和内核空间共同识别的桥梁。

Syscall是通过中断方式实现的，用户程序通过软中断让程序陷入内核态，执行相应的操作。Linux内核中，每个Syscall都有唯一的系统调用号对应。

基于Syscall机制，内核驻留在受保护的地址空间，用户空间程序无法直接执行内核代码，也无法访问内核数据，通过系统调用

系统调用列表可在 ``linux/arch/sh/include/uapi/asm/unistd_64.h`` 中找到，以arm为例，其他Syscall相关的源码包括：

- arch/arm/include/Uapi/asm/unistd.h
- arch/arm/kernel/calls.S
- arch/arm/kernel/entry-armv.S
- arch/arm/kernel/entry-common.S
- include/linux/syscalls.h
- include/uapi/asm-generic/unistd.h
- kernel/signal.c

Syscall流程
----------------------------------------

x86/64
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在x86/64架构上， ``arch/x86/syscalls/syscall_64.tbl`` 映射了系统调用号到函数定义的关系。同目录的syscalltbl.sh脚本，根据syscall_64.tbl生成一个头文件arch/x86/include/generated/asm/syscalls_64.h，并用__SYSCALL_COMMON将系统调用号和系统函数地址做映射，最终保存到sys_call_table数组。

进程用户态执行syscall指令后进入到内核入口函数system_call()，此时寄存器状态：

- rax -> 系统调用号
- rdi -> 参数1
- rsi -> 参数2
- rdx -> 参数3
- r10 -> 参数4
- r8  -> 参数5
- r9  -> 参数6

system_call()的地址在内核初始化时被syscall_init()写入进MSR_LSTAR寄存器，该寄存器的作用是保存syscall指令的处理函数地址。

ARM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在ARM架构上，通过swi中断来实现系统调用，实现从用户态切换到内核态，发送软中断swi时，从中断向量表中查看跳转代码，其中异常向量表定义在文件 ``arch/arm/kernel/entry-armv.S`` 。当执行系统调用时会根据系统调用号从系统调用表中来查看目标函数的入口地址，在calls.S文件中声明了入口地址信息。
