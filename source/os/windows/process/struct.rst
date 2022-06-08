数据结构
========================================

每个Windows进程都是由一个执行体进程(EPROCESS)块来表示，包含与进程有关的属性及其他的相关数据结构(一个或者多个线程)。

线程由执行体线程(ETHREAD) 块来表示。

EPROCESS 块和相关的大部分数据结构都位于系统地址空间中，不过PEB (Process Environment Block, 进程环境块)和TEB(Thread Environment Block, 线程环境块)则位于进程地址空间中

Windows 子系统也会创建一个和进程平行的 CSR_PROCESS 结构体。

需要对应查看具体的数据结构时，可以使用 ``dt nt!_eprocess`` 等调试命令。
