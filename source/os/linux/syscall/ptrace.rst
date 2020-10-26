ptrace
========================================
ptrace函数原型为 ``long ptrace(enum __ptrace_request request, pid_t pid, void *addr, void *data);`` ，其中 ``enum __ptrace_request request`` 指示了ptrace要执行的命令，``pid_t pid`` 指示ptrace要跟踪的进程，``void *addr`` 指示要监控的内存地址，``void *data`` 存放读取出的或者要写入的数据。

函数执行成功时返回0，错误时返回-1，同时设置errno。

其中request的选项包括：

- PTRACE_TRACEME
    - 表示本进程将被其父进程跟踪，交付给这个进程的所有信号(除SIGKILL之外)，都将使其停止，父进程将通过wait()获知这一情况
- PTRACE_PEEKTEXT
    - 从内存地址中读取一个WORD，内存地址为addr
- PTRACE_PEEKDATA
- PTRACE_PEEKUSER
    - 从USER区域中读取一个WORD，内存地址为addr
- PTRACE_PEEKTEXT
    - 向内存地址中写入一个WORD，内存地址为addr
- PTRACE_PEEKDATA
- PTRACE_PEEKUSER
    - 向USER区域中写入一个WORD，内存地址为addr
- PTRACE_CONT
    - 继续运行之前停止的子进程
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

WORD的长度在64位程序中是64位，32位程序中是32位。
ptrace系统调用在核心对应的处理函数为sys_ptrace。
