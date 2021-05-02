ptrace
========================================
ptrace函数原型为 ``long ptrace(enum __ptrace_request request, pid_t pid, void *addr, void *data);`` ，其中 ``enum __ptrace_request request`` 指示了ptrace要执行的命令，``pid_t pid`` 指示ptrace要跟踪的进程，``void *addr`` 指示要监控的内存地址，``void *data`` 存放读取出的或者要写入的数据。

函数执行成功时返回0，错误时返回-1，同时设置errno。

其中request的选项包括：

- PTRACE_TRACEME
    - 表示本进程将被其父进程跟踪
- PTRACE_PEEKTEXT
    - 从内存地址中读取一个WORD，内存地址为addr
- PTRACE_PEEKDATA
    - 由于Linux不区分文本与数据段的地址空间，PTRACE_PEEKDATA与PTRACE_PEEKTEXT无区别
- PTRACE_PEEKUSER
    - 从USER区域中读取一个WORD，内存地址为addr，值将作为结果返回，其中偏移地址addr通常是是字对齐的，data将被忽略
- PTRACE_PEEKTEXT
    - 向内存地址中写入一个WORD，内存地址为addr
- PTRACE_PEEKDATA
- PTRACE_PEEKUSER
    - 向USER区域中写入一个WORD，内存地址为addr
- PTRACE_CONT
    - 继续运行之前停止的子进程
    - 如果data是非空的，它被解释为要发送给Tracee 的信号
- PTRACE_KILL
    - 杀掉子进程
- PTRACE_SINGLESTEP
    - 设置单步执行标志
    - 子进程在每次机器指令后都被暂停
- PTRACE_GETREGS
    - 读取寄存器
- PTRACE_GETFPREGS
    - 读取浮点寄存器
- PTRACE_SETREGS
    - 设置寄存器
- PTRACE_SETFPREGS
    - 设置浮点寄存器
- PTRACE_ATTACH
    - attach到一个指定的进程，使其成为当前进程跟踪的子进程
    - 子进程的行为等同于它进行了一次PTRACE_TRACEME操作
- PTRACE_DETACH
- PTRACE_SYSCALL
    - 进程在每次系统调用之后暂停
- PT_DENY_ATTACH
    - 反调试
- PTRACE_GETSIGINFO
    - 获取导致子进程停止执行的信号信息，并将其存放在父进程内由data指向的位置
    - addr参数将被忽略
- PTRACE_SETSIGINFO
    - 将父进程内由data指向的数据作为siginfo_t结构体拷贝到子进程
    - addr参数将被忽略
- PTRACE_SETOPTIONS
    - 将父进程内由data指向的值设定为ptrace选项
    - data作为位掩码来解释
- PTRACE_GETEVENTMSG
    - 获取刚发生的ptrace事件消息，并存放在data指向的位置
    - addr参数将被忽略
- PTRACE_SYSEMU
    - 用于用户模式的程序仿真子进程的所有系统调用
- PTRACE_SYSEMU_SINGLESTEP
    - 用于用户模式的程序仿真子进程的所有系统调用

其中PTRACE_TRACEME和PTRACE_ATTACH的区别为：PTRACE_TRACEME是子进程主动申请被TRACE。而PTRACE_ATTACH是父进程自己要attach到子进程，相当于子进程是被动的trace。

WORD的长度在64位程序中是64位，32位程序中是32位。
ptrace系统调用在核心对应的处理函数为sys_ptrace。

在trace中Tracer指追踪进程，Tracee指被追踪、被观察的进程。

PTRACE_SETOPTIONS 的取值可能为：

- PTRACE_O_EXITKILL
    - 当跟踪进程退出时，向所有被跟踪进程发送SIGKILL信号将其退出
- PTRACE_O_TRACECLONE
    - 被跟踪进程在下一次调用clone()时将其停止，并自动跟踪新产生的进程
    - 新的进程在开始时收到SIGSTOP信号
    - 其新产生的进程的pid可以通过PTRACE_GETEVENTMSG获取
- PTRACE_O_TRACEEXEC
    - 被跟踪进程在下一次调用exec()函数时使其停止
- PTRACE_O_TRACEEXIT
    - 被跟踪进程在退出是停止其执行
    - 被跟踪进程的退出状态可通过PTRACE_GETEVENTMSG获得
- PTRACE_O_TRACEFORK
    - 被跟踪进程在下次调用fork()时停止执行，并自动跟踪新产生的进程
    - 新的进程在开始时收到SIGSTOP信号
    - 其新产生的进程的pid可以通过PTRACE_GETEVENTMSG获取
- PTRACE_O_TRACEVFORK
    - 被跟踪进程在下次调用vfork()时停止执行，并自动跟踪新产生的进程
    - 新的进程在开始时收到SIGSTOP信号
    - 其新产生的进程的pid可以通过PTRACE_GETEVENTMSG获取
