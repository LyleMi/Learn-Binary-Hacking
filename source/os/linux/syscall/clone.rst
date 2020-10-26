clone
========================================
clone的函数原型为 ``int clone(int (fn)(void ), void *child_stack, int flags, void *arg);`` 。

其中fn是函数指针，child_stack是为子进程分配系统堆栈空间，flags标志用来描述需要从父进程继承哪些资源，arg是传给子进程的参数。

其中flags取值如下：

- CLONE_PARENT
    - 创建的子进程的父进程是调用者的父进程，新进程与创建它的进程成了“兄弟”而不是“父子”
- CLONE_FS
    - 子进程与父进程共享相同的文件系统，包括root、当前目录、umask
- CLONE_FILES
    - 子进程与父进程共享相同的文件描述符（file descriptor）表
- CLONE_NEWNS
    - 在新的namespace启动子进程
    - namespace描述了进程的文件hierarchy
- CLONE_SIGHAND
    - 子进程与父进程共享相同的信号处理（signal handler）表
- CLONE_PTRACE
    - 若父进程被trace，子进程也被trace
- CLONE_VFORK
    - 父进程被挂起，直至子进程释放虚拟内存资源
- CLONE_VM
    - 子进程与父进程运行于相同的内存空间
- CLONE_PID
    - 子进程在创建时PID与父进程一致
- CLONE_THREAD
    - Linux 2.4中增加以支持POSIX线程标准
    - 子进程与父进程共享相同的线程群
