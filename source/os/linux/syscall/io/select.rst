select
========================================
select是IO多种复用的一种实现，它将需要监控的fd分为读、写、异常三类，其返回时是读、写、异常事件发生或者超时。

``select`` 系统调用的原型如下：

.. code-block:: cpp

    #include <unistd.h>
    #include <sys/types.h>

    int select(
        int __nfds,
        fd_set *__restrict __readfds,
        fd_set *__restrict __writefds,
        fd_set *__restrict __exceptfds,
        struct timeval *__restrict __timeout
    );

相关的宏定义有：

.. code-block:: cpp

    FD_ZERO (fd_set *set);          // 清空文件描述符
    FD_CLR (int fd, fd_set *set);   // 在指定集合中删除文件描述符
    FD_SET (int fd, fd_set *set);   // 向指定集合中添加文件描述符
    FD_ISSET (int fd, fd_set *set); // 检查文件描述符是否在集合中

select 的主要缺陷在于效率较低，并不能直接遍历 readfds ，而需要遍历所有传入的 fd ，判断是否存在于 readfds 中。在只有少量 fd 有数据的情况下，遍历会消耗较多的资源。
