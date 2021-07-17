namespace
========================================

简介
----------------------------------------
namespace 是 Linux 内核用来隔离内核资源的方式。通过 namespace 可以让一些进程只能看到与自己相关的一部分资源，而另外一些进程也只能看到与它们自己相关的资源，这两拨进程根本就感觉不到对方的存在。具体的实现方式是把一个或多个进程的相关资源指定在同一个 namespace 中。

Linux 在很早的版本中就实现了部分的 namespace，比如内核 2.4 就实现了 mount namespace。大多数的 namespace 支持是在内核 2.6 中完成的，比如 IPC、Network、PID、和 UTS。还有个别的 namespace 比较特殊，比如 User，从内核 2.6 就开始实现了，但在内核 3.8 中才宣布完成。同时，随着 Linux 自身的发展以及容器技术持续发展带来的需求，也会有新的 namespace 被支持，比如在内核 4.6 中就添加了 Cgroup namespace。

隔离内容
----------------------------------------
.. list-table:: 隔离功能
    :header-rows: 1

    * - namespace
      - 系统调用参数
      - 隔离内容
    * - UTS
      - CLONE_NEWUTS
      - 主机和域名
    * - IPC
      - CLONE_NEWIPC
      - 信号量、消息队列和共享内存
    * - PID
      - CLONE_NEWPID
      - 进程编号
    * - Network
      - CLONE_NEWNET
      - 网络设备、网络栈、端口等
    * - Mount
      - CLONE_NEWNS
      - 挂载点 (文件系统)
    * - User
      - CLONE_NEWUSER
      - 用户和用户组 
