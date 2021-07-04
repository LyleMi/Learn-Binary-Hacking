brk与sbrk
========================================
.. code-block:: cpp

    #include<unistd.h>
    int brk(void *end_data_segment); 
    void * sbrk(intptr_t increment);

``brk`` 将数据段的结束地址设置为传入的参数 ``end_data_segment`` 的值，成功返回0，失败返回-1并且设置errno值为ENOMEM。

brk从内核获取内存是通过增加 ``brk`` (program break) 地址的方式，开始于 ``start_brk`` 、结束于 ``brk`` ，初始的时候两者都指向的是同一个位置。
当ASLR关闭的时候， ``start_brk`` 和 ``brk`` 都是指向bss段的尾部的，当ASLR开启的时候， ``start_brk`` 和 ``brk`` 初始位置是bss段的尾部加一个随机的偏移。

sbrk会根据传入的 ``increment`` 调整空间，increment为0时返回当前堆的结束地址，为正数时增加堆的空间，为负数时减小堆的空间。
