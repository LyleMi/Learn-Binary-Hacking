setsockopt
========================================
setsockopt用于任意类型、任意状态套接口的设置选项值，函数原型为 ``int setsockopt(int s, int level, int optname, void* optval, socklen_t* optlen);`` 。

选项为：

- sockfd 标识一个套接口的描述字
- level 选项定义的层次，定义在 ``include/linux/socket.h`` 中
    - SOL_IP 0
    - SOL_SOCKET 通用套接字选项
    - IPPROTO_TCP TCP选项
    - IPPROTO_IP IP选项
    - IPPROTO_IPV6
- optname 需设置的选项，定义在 ``include/uapi/linux/in.h`` 中
    - IP_MULTICAST_TTL 设置多播组数据的TTL值
    - IP_ADD_MEMBERSHIP 在指定接口上加入组播组
    - IP_DROP_MEMBERSHIP 退出组播组
    - IP_MULTICAST_IF 获取默认接口或设置接口
    - IP_MULTICAST_LOOP 禁止组播数据回送
- optval 指向存放选项待设置的新值的缓冲区的指针
- optlen optval缓冲区长度

成功执行时，返回0。失败返回-1，errno被设为以下的某个值

- EBADF：sock不是有效的文件描述词
- EFAULT：optval指向的内存并非有效的进程空间
- EINVAL：在调用setsockopt()时，optlen无效
- ENOPROTOOPT：指定的协议层不能识别选项
- ENOTSOCK：sock描述的不是套接字
