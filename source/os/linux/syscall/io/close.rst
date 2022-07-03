close系列调用
========================================

close
----------------------------------------
.. code-block:: cpp

    #include <unistd.h>
    int close(int fd);

shutdown
----------------------------------------
.. code-block:: cpp

    #include <sys/socket.h>
    int shutdown(int sockfd, int how);

howto 的取值可以为：

- SHUT_RD
    - 断开输入流，清空输入缓冲区
    - 套接字无法接收数据，无法调用输入相关函数
- SHUT_WR
    - 传递输出缓冲区中还有未传输的数据，断开输出流
    - 套接字无法发送数据
- SHUT_RDWR
    - 同时断开 I/O 流
    - 相当于分别以 SHUT_RD / SHUT_WR 为参数两次调用 shutdown

在socket的使用场景下， ``shutdown`` 会等待输出缓冲区中的数据传输完毕再发送FIN包，而 ``close`` 不会处理输出缓冲区，立即向网络中发送FIN包。也就是说，调用 ``close`` 将丢失输出缓冲区中的数据。另外 ``shutdown`` 并不会关闭套接字，关闭套接字仍需要调用 ``close`` 。
