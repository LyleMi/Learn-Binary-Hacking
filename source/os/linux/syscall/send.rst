send系列系统调用
========================================

函数原型
----------------------------------------
.. code-block:: cpp

    #include <sys/types.h>
    #include <sys/socket.h>
    ssize_t send(int sock, const void *buf, size_t len, int flags);
    ssize_t sendto(int sock, const void *buf, size_t len, int flags, const struct sockaddr *to, socklen_t tolen);
    ssize_t sendmsg(int sock, const struct msghdr *msg, int flags);

其中参数的含义如下：

- sock：表示发送数据的套接字
- buf：指向将要发送数据的缓冲区
- len：缓冲区的长度
- flags：是以下零个或者多个标志的组合体，可通过or一起设置
    - MSG_DONTROUTE
        - 不要使用网关来发送封包，只发送到直接联网的主机
        - 这个标志主要用于诊断或者路由程序
    - MSG_DONTWAIT：操作不会被阻塞
    - MSG_EOR：终止一个记录
    - MSG_MORE：调用者有更多的数据需要发送
    - MSG_NOSIGNAL：当另一端终止连接时，请求在基于流的错误套接字上不要发送SIGPIPE信号
    - MSG_OOB：发送out-of-band数据
- to：指向存放接收端地址的区域，可为NULL
- tolen：以上内存区的长度，可以为0
- msg：指向存放发送消息头的内存缓冲

返回值有三类：

- 返回值 = 0
- 返回值 < 0：发送失败，错误原因存于全局变量errno中
- 返回值 > 0：表示发送的字节数（实际上是拷贝到发送缓冲中的字节数）

错误代码：

- EBADF 非合法的socket处理代码
- EFAULT 参数中有一指针指向无法存取的内存空间
- ENOTSOCK sockfd是文件描述符，非socket
- EINTR 被信号中断
- EAGAIN 此操作会令进程阻断，但sockfd为不可阻断
- ENOBUFS 系统的缓冲内存不足
- ENOMEM 核心内存不足
- EINVAL 传给系统调用的参数不正确
