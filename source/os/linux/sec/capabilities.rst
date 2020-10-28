Capabilities
========================================

简介
----------------------------------------
Capabilities机制是在 Linux 2.2 之后引入的，用于将之前与超级用户root(UID=0)关联的特权细分为不同的功能组，Capabilites 作为线程的属性存在，每个功能组都可以独立启用和禁用。其本质上就是将内核调用分门别类，具有相似功能的内核调用被分到同一组中。

这样一来，权限检查的过程就变成了：在执行特权操作时，如果线程的有效身份不是 root，就去检查其是否具有该特权操作所对应的capabilities，并以此为依据，决定是否可以执行特权操作。

Capabilities可以在进程执行时赋予，也可以直接从父进程继承。

列表
----------------------------------------
- CAP_AUDIT_CONTROL
    - 启用和禁用内核审计
    - 改变审计过滤规则
    - 检索审计状态和过滤规则
- CAP_AUDIT_READ
    - 允许通过 multicast netlink 套接字读取审计日志
- CAP_AUDIT_WRITE
    - 将记录写入内核审计日志
- CAP_BLOCK_SUSPEND
    - 使用可以阻止系统挂起的特性
- CAP_CHOWN
    - 修改文件所有者的权限
- CAP_DAC_OVERRIDE
    - 忽略文件的 DAC 访问限制
- CAP_DAC_READ_SEARCH
    - 忽略文件读及目录搜索的 DAC 访问限制
- CAP_FOWNER
    - 忽略文件属主 ID 必须和进程用户 ID 相匹配的限制
- CAP_FSETID
    - 允许设置文件的 setuid 位
- CAP_IPC_LOCK
    - 允许锁定共享内存片段
- CAP_IPC_OWNER
    - 忽略 IPC 所有权检查
- CAP_KILL
    - 允许对不属于自己的进程发送信号
- CAP_LEASE
    - 允许修改文件锁的 FL_LEASE 标志
- CAP_LINUX_IMMUTABLE
    - 允许修改文件的 IMMUTABLE 和 APPEND 属性标志
- CAP_MAC_ADMIN
    - 允许 MAC 配置或状态更改
- CAP_MAC_OVERRIDE
    - 忽略文件的 DAC 访问限制
- CAP_MKNOD
    - 允许使用 mknod() 系统调用
- CAP_NET_ADMIN
    - 允许执行网络管理任务
- CAP_NET_BIND_SERVICE
    - 允许绑定到小于 1024 的端口
- CAP_NET_BROADCAST
    - 允许网络广播和多播访问
- CAP_NET_RAW
    - 允许使用原始套接字
- CAP_SETGID
    - 允许改变进程的 GID
- CAP_SETFCAP
    - 允许为文件设置任意的 capabilities
- CAP_SETPCAP
- CAP_SETUID
    - 允许改变进程的 UID
- CAP_SYS_ADMIN
    - 允许执行系统管理任务，如加载或卸载文件系统、设置磁盘配额等
- CAP_SYS_BOOT
    - 允许重新启动系统
- CAP_SYS_CHROOT
    - 允许使用 chroot() 系统调用
- CAP_SYS_MODULE
    - 允许插入和删除内核模块
- CAP_SYS_NICE
    - 允许提升优先级及设置其他进程的优先级
- CAP_SYS_PACCT
    - 允许执行进程的 BSD 式审计
- CAP_SYS_PTRACE
    - 允许跟踪任何进程
- CAP_SYS_RAWIO
    - 允许直接访问 /devport、/dev/mem、/dev/kmem 及原始块设备
- CAP_SYS_RESOURCE
    - 忽略资源限制
- CAP_SYS_TIME
    - 允许改变系统时钟
- CAP_SYS_TTY_CONFIG
    - 允许配置 TTY 设备
- CAP_SYSLOG
    - 允许使用 syslog() 系统调用
- CAP_WAKE_ALARM
    - 允许触发一些能唤醒系统的东西(比如 CLOCK_BOOTTIME_ALARM 计时器)

线程的 capabilities
----------------------------------------
每一个线程都有5个capabilities集合，每一个集合使用64位掩码来表示。这5个capabilities集合分别是：

- Permitted
- Effective
- Inheritable
- Bounding
- Ambient

每个集合中都包含零个或多个capabilities。这5个集合的具体含义如下：

Permitted
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
定义了线程能够使用的capabilities的上限。

Effective
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
内核检查Effective集合以判断线程是否可以进行特权操作时。

Inheritable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
当执行exec()系统调用时，能够被新的可执行文件继承的capabilities。

Bounding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Bounding集合是Inheritable集合的超集，如果某个capability不在Bounding集合中，即使它在Permitted集合中，该线程也不能将该capability添加到它的Inheritable集合中。

Bounding集合的capabilities在执行fork()系统调用时会传递给子进程的Bounding集合，并且在执行execve系统调用后保持不变。

当线程运行时，不能向Bounding集合中添加capabilities。

一旦某个capability被从Bounding集合中删除，便不能再添加回来。

将某个capability从Bounding集合中删除后，如果之前Inherited集合包含该capability，将继续保留。但如果后续从Inheritable集合中删除了该capability，便不能再添加回来。

Ambient
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Linux 4.3内核新增的capabilities集合，用于弥补Inheritable的不足。Ambient具有如下特性：

- Permitted和Inheritable未设置的capabilities，Ambient也不能设置。
- 当Permitted和Inheritable关闭某权限(比如 CAP_SYS_BOOT)后，Ambient也随之关闭对应权限。确保进程降低权限后子进程也会降低权限。
- 非特权用户如果在Permitted集合中有一个capability，那么可以添加到Ambient集合中，这样它的子进程可以获取这个capability。
