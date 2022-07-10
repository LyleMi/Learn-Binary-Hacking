epoll
========================================
``epoll`` 系统调用的接口如下：

.. code-block:: cpp

    #include <sys/epoll.h>

    struct epoll_event {
        _u32 events; 
        union {
            void *ptr;
            int fd;
            _32 u32;
            _u64 u64;
        } data;
    }

    int epoll_create(int size);
    int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
    int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);

epoll 在 Linux 2.6 之后提出，是基于事件驱动的I/O方式。

epoll_event 的取值有：

- EPOLLIN 文件有数据可读
- EPOLLOUT 文件可写
- EPOLLHUP 文件被挂起

epoll_create 函数用于创建 epoll 句柄，参数 size 表明内核要监听的描述符数量。调用成功时返回 epoll 句柄，失败时返回-1。

epoll_ctl函数用于注册要监听的事件类型，其中 epfd 表示 epoll 句柄，op 表示 fd 操作类型，fd 是要操作的描述符。

op 的操作类型有：

- EPOLL_CTL_ADD 注册新的fd
- EPOLL_CTL_MOD 修改已注册的fd
- EPOLL_CTL_DEL 从epfd中删除fd
