Socket
========================================

Unix Socket
----------------------------------------
Unix Socket 可以使同一台操作系统上的两个或多个进程进行数据通信。Unix Socket 既可以使用字节流，又可以使用数据队列。

常见结构
----------------------------------------
sockaddr是通用的套接字地址 是linux网络通信的地址结构体的一种，此数据结构用做bind、connect、recvfrom、sendto等函数的参数，指明地址信息。

.. code-block:: c

    struct sockaddr {
        sa_family_t sa_family;  /* address family, AF_xxx   */
        char        sa_data[14];    /* 14 bytes of protocol address */
    };

sockaddr_in是internet环境下套接字的地址形式，和sockaddr长度一样，都是16个字节。二者是并列结构，指向sockaddr_in结构的指针也可以指向sockaddr。

.. code-block:: c

    struct sockaddr_in {
      __kernel_sa_family_t  sin_family; /* Address family       */
      __be16        sin_port;   /* Port number          */
      struct in_addr    sin_addr;   /* Internet address     */

      /* Pad to size of `struct sockaddr'. */
      unsigned char     __pad[__SOCK_SIZE__ - sizeof(short int) -
                sizeof(unsigned short int) - sizeof(struct in_addr)];
    };

sockaddr_un是unix环境下套接字的地址形式，这种方式也称为本地套接字。

.. code-block:: c

    struct sockaddr_un {
        __kernel_sa_family_t sun_family; /* AF_UNIX */
        char sun_path[UNIX_PATH_MAX];   /* pathname */
    };

常用函数
----------------------------------------
socket调用常用的函数有：socket/bind/listen/connect/accept/read/write/close等，其中部分函数的详细定义与分析见syscall部分。

socket
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
socket 函数的原型为 ``int socket(int domain, int type, int protocol);`` 。

domain 用于指定通信协议族，常用的协议族有: 

- AF_UNIX
- AF_LOCAL
- AF_INET (IPv4)
- AF_INET6 (IPv6)
- ...

type 用于指定socket类型，常用的socket类型有: 

- SOCK_STREAM (TCP)
- SOCK_DGRAM (udP)
- SOCK_SEQPACKET
- SOCK_RAW
- SOCK_RDM
- SOCK_PACKET

protocol 用于指定socket所使用的协议，一般设置为0，表示使用type中的默认协议。

listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
listen 函数的原型为 ``int listen(int sockfd, int backlog);`` 。其中sockfd即socket描述符，backlog:为相应socket可以排队的最大连接个数，表示排队连接队列的长度。

服务器在调用socket()、bind()之后就会调用listen()来监听这个socket。

connect
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
connect函数的原型为 ``int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);`` 。其中sockfd即socket描述符，addr是socket地址，addrlen为地址长度。

在内核中，根据用户传入的 sockfd 查询对应的 socket 内核对象。

在 socket 为 tcp ipv4 的类型时，调用位于 ``net/ipv4/tcp_ipv4.c`` 的 ``tcp_v4_connect`` 函数，随后调用 ``inet_hash_connect`` 函数选择端口。

如果之前调用过 ``bind`` ，则设置到对应的端口。否则读取 ``net.ipv4.ip_local_port_range`` 内核参数获取端口范围。而后在可选择的端口范围中，不断随机选择端口，如果端口没有被占用，则使用对应的端口。这里是否占用是判断四元组（源IP、源端口、目的IP、目的端口）是否已经存在。

这里需要注意的是，在大量发起连接时，端口被大量占用，上述的随机选择端口并检查是否被占用的流程会反复执行，造成较大的性能损耗。

read / write
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
常见的用于读写的函数有：

.. code-block:: c

    int read(int socket, char *buffer, size_t len);
    int write(int socket, char *buffer, size_t len);

    ssize_t send(int sockfd, const void *buf, size_t len, int flags);
    ssize_t recv(int sockfd, void *buf, size_t len, int flags);

    ssize_t sendto(int sockfd, const void *buf, size_t len, int flags, const struct sockaddr *dest_addr, socklen_t addrlen);
    ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);

    ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);
    ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);

close
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
close的函数原型为 ``int close(int fd);`` ，用于关闭socket描述字。
