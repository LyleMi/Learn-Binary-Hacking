poll
========================================
``poll`` 系统调用的接口如下：

.. code-block:: cpp

    #include <poll.h>

    typedef struct pollfd {
        int fd;         // 监听的文件描述符
        short events;   // 需要关注的的事件
        short revents;  // 实际发生的事件
    } pollfd_t;

    int poll(struct pollfd fds[], nfds_t nfds, int timeout);

在参数中， fds 是要监听的文件描述符结合。其中 events 的取值有：

- POLLIN 有数据可读
- POLLRDNORM 有普通数据可读
- POLLRDBAND 有优先数据可读
- POLLPRI 有紧迫数据可读
- POLLOUT 文件写操作不阻塞
- POLLWRNORM 写普通数据不会导致阻塞
- POLLWRBAND 写优先数据不会导致阻塞
- POLLMSGSIGPOLL 消息可用

需要监听多个事件时，可以使用按位与的方式为 events 赋值。
nfds 为文件描述符集合的数量，timeout 为超时时间。

poll() 函数返回fds集合中就绪的读、写，或异常的描述符数量，返回0表示超时，返回-1表示出错。
