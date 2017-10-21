Linux
================================


内存布局
---------------------------------

32位

::

    ----------------------------------
    |        Kernel Space            |    1GB
    ----------------------------------
    |                                |    Random Stack offset
    ----------------------------------
    |       Stack (growth down)      |    RLIMIT_STACK
    ----------------------------------
    |                                |    Random mmap offset
    ----------------------------------
    |       Memory Map Segement      |
    |  (including dynamic libraries) |
    |      e.g. /lib/libc.so         |  
    |         (growth down)          |    
    ----------------------------------
    |                                |  
    ----------------------------------
    |        Heap (growth up)        |   
    ----------------------------------
    |                                |  Random brk offset
    ----------------------------------
    |          BSS Segement          |   
    | Uninitalized static variables, | 
    |       filled with zeros.       | 
    |  e.g. static char *userName;   |   
    ----------------------------------
    |          Data Segement         |
    |  Static variables initialized  |
    |    by the programmer.          |
    ----------------------------------
    |          Text Segement         |
    ----------------------------------


设置aslr
-----------------------------------------
``cat /proc/sys/kernel/randomize_va_space``
``sudo sysctl -w kernel.randomize_va_space=0``

堆栈
-----------------------------------------
栈向下生长，其地址越来低，LIFO
堆向上生长，其地址越来越高，FIFO

syscall调用过程
-----------------------------------------

1. The specific syscall number is loaded into EAX.
2. Arguments to the syscall function are placed in other registers.
3. The instruction int 0x80 is executed.
4. The CPU switches to kernel mode.
5. The syscall function is executed.
   
每个syscall最多有六个参数
ebx, ecx, edx, esi, edi, epb
如果需要超过六个参数的时候，把所有的参数放到一个结构体然后结构体赋给ebx

