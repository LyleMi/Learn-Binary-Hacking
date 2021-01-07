recv系列系统调用
========================================
.. code-block:: cpp

    #include <sys/types.h>
    #include <sys/socket.h>
    ssize_t recv(int sock, void *buf, size_t len, int flags);
    ssize_t recvfrom(int sock, void *buf, size_t len, int flags,
         struct sockaddr *from, socklen_t *fromlen);
    ssize_t recvmsg(int sock, struct msghdr *msg, int flags);

其中参数的含义如下：

- sock：表示发送数据的套接字
- buf：指向将要发送数据的缓冲区
- len：缓冲区的长度
- flags：是零个或者多个标志的组合体，可通过or一起设置
- from：指向存放对端地址的区域，如果为NULL，不储存对端地址
- fromlen
    - 作为入口参数，指向存放表示from最大容量的内存单元
    - 作为出口参数，指向存放表示from实际长度的内存单元
- msg：指向存放进入消息头的内存缓冲

其中flag的标志如下：

- MSG_DONTWAIT：操作不会被阻塞
- MSG_ERRQUEUE：指示应该从套接字的错误队列上接收错误值
- MSG_PEEK：指示数据接收后，在接收队列中保留原数据，不将其删除，随后的读操作还可以接收相同的数据
- MSG_TRUNC：返回封包的实际长度，即使它比所提供的缓冲区更长，只对packet套接字有效
- MSG_WAITALL：要求阻塞操作，直到请求得到完整的满足。然而，如果捕捉到信号，错误或者连接断开发生，或者下次被接收的数据类型不同，仍会返回少于请求量的数据
- MSG_EOR：指示记录的结束，返回的数据完成一个记录
- MSG_TRUNC：指明数据报尾部数据已被丢弃，因为它比所提供的缓冲区需要更多的空间
- MSG_CTRUNC：指明由于缓冲区空间不足，一些控制数据已被丢弃
- MSG_OOB：指示接收到out-of-band数据
- MSG_ERRQUEUE：指示除了来自套接字错误队列的错误外，没有接收到其它数据

返回值

- 成功执行时，返回接收到的字节数
- 若另一端已关闭连接则返回0
- 失败返回-1，并设置errno

errno的含义如下：

- EAGAIN：套接字已标记为非阻塞，而接收操作被阻塞或者接收超时
- EBADF：sock不是有效的描述词
- ECONNREFUSE：远程主机阻绝网络连接
- EFAULT：内存空间访问出错
- EINTR：操作被信号中断
- EINVAL：参数无效
- ENOMEM：内存不足
- ENOTCONN：与面向连接关联的套接字尚未被连接上
- ENOTSOCK：sock索引的不是套接字
