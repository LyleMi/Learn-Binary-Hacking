syscall
========================================

简介
----------------------------------------
syscall是内核提供用户空间程序与内核空间进行交互的一套标准接口，这些接口让用户态程序能受限访问硬件设备，比如申请系统资源、操作设备读写、创建新进程等。用户空间发起请求，内核空间负责执行，这些接口便是用户空间和内核空间共同识别的桥梁。

Syscall是通过中断方式实现的，用户程序通过软中断切入内核态，执行指令。Linux内核中，每个Syscall都有唯一的系统调用号对应。

系统调用列表可在 ``linux/arch/sh/include/uapi/asm/unistd_64.h`` 中找到。

ioctl
----------------------------------------
ioctl 是设备驱动程序中设备控制接口函数，一个字符设备驱动通常会实现设备打开、关闭、读、写等功能，在一些需要细分的情境下，如果需要扩展新的功能，通常以增设 ioctl 命令的方式实现。

.. code-block:: cpp

    #include <sys/ioctl.h> 
    int ioctl(int fd, int cmd, ...) ;

其中fd是文件描述符，cmd是交互协议，设备驱动将根据 cmd 执行对应操作，最后是可变参数，依赖 cmd 指定长度以及类型。ioctl 函数执行成功时返回 0，失败则返回 -1 并设置全局变量 errorno 值。

fcntl
----------------------------------------
fcntl用于根据文件描述词来操作文件的特性，函数原型如下：

.. code-block:: cpp

    #include <fcntl.h>; 
    int fcntl(int fd, int cmd); 
    int fcntl(int fd, int cmd, long arg); 
    int fcntl(int fd, int cmd, struct flock *lock); 

fcntl函数有5种功能： 

- 复制一个现有的描述符 ``cmd=F_DUPFD``
- 获得/设置文件描述符标记 ``cmd=F_GETFD或F_SETFD``
- 获得/设置文件状态标记 ``cmd=F_GETFL或F_SETFL``
- 获得/设置异步I/O所有权 ``cmd=F_GETOWN或F_SETOWN``
- 获得/设置记录锁 ``cmd=F_GETLK,F_SETLK或F_SETLKW``
