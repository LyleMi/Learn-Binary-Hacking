fork
========================================

简介
----------------------------------------
fork 系统调用将创建一个与父进程几乎一样的新进程，复制了父亲进程的资源，包括内存的内容task_struct内容

在Linux的早期系统中，当发出fork()系统调用时，内核原样复制父进程的整个地址空间并把复制的那一份分配给子进程。这种完整复制需要如下操作：

- 为子进程的页表分配页帧
- 为子进程的页分配页帧
- 初始化子进程的页表
- 把父进程的页复制到子进程相应的页中

现代的Linux内核采用一种更为有效的方法，称之为写时复制。父进程和子进程共享页帧而不是复制页帧。然而，只要页帧被共享，它们就不能被修改，即页帧被保护。无论父进程还是子进程何时试图写一个共享的页帧，就产生一个异常，这时内核就把这个页复制到一个新的页帧中并标记为可写。原来的页帧仍然是写保护的：当其他进程试图写入时，内核检查写进程是否是这个页帧的唯一属主，如果是，就把这个页帧标记为对这个进程是可写的。

fork失败时返回值为负数，成功时子进程返回0，父进程返回子进程的pid。

源码分析
----------------------------------------
fork 相关代码在以下的位置中：

- kernel/include/linux/sched.h
- kernel/include/linux/kthread.h
- kernel/arch/arm/include/asm/thread_info.h
- kernel/kernel/fork.c
- kernel/kernel/exit.c
- kernel/kernel/sched/core.c

fork的调用流为 ``fork->sys_fork->do_fork->copy_process`` 。即用户空间调用fork()方法，经过syscall陷入内核空间，内核根据系统调用号找到相应的 ``sys_fork`` 系统调用。``sys_fork`` 调用 ``do_fork`` 。``do_fork()`` 会进行一些check过程，之后便是进入核心方法 ``copy_process`` 。

其中fork/vfork/pthread_create等几个系统调用都最终调用了 ``do_fork``，pthread_create调用的flags参数为 ``CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND`` ，fork调用的flags参数为 ``SIGCHLD`` ，vfork调用的flags参数为 ``CLONE_VFORK | CLONE_VM | SIGCHLD`` 。关于flags参数的意义在clone调用的文档中可以看到。

.. code-block:: cpp

    SYSCALL_DEFINE0(fork)
    {
      return do_fork(SIGCHLD, 0, 0, NULL, NULL);
    }

其中 ``do_fork`` 的实现为

.. code-block:: cpp

    long do_fork(unsigned long clone_flags,
            unsigned long stack_start,
            unsigned long stack_size,
            int __user *parent_tidptr,
            int __user *child_tidptr)
    {
      return _do_fork(clone_flags, stack_start, stack_size,
          parent_tidptr, child_tidptr, 0);
    }

其中clone_flags为clone方法传递过程的flags，标记子进程从父进程中需要继承的资源清单。

stack_start为子进程用户态的堆栈地址，fork()过程该值为0，clone()过程赋予有效值。

stack_size为不必要的参数，默认设置为0。

parent_tidptr为用户态下父进程的tid地址。

child_tidptr为用户态下子进程的tid地址。

``_do_fork`` 主要流程为：

- 执行copy_process，复制进程描述符，分配pid
- 如果由vfork调用，则执行相应的初始化过程
- 执行wake_up_new_task，唤醒子进程，分配CPU时间片
- 如果由vfork调用，则父进程等待子进程执行exec函数来替换地址空间

``copy_process`` 的主要流程为：

- 执行dup_task_struct()，拷贝当前进程task_struct
- 检查进程数是否超过系统所允许的上限 (默认32678)
- 执行sched_fork()，设置调度器相关信息，设置task进程状态为TASK_RUNNING，并分配CPU资源
- 执行copy_xxx()，拷贝进程的files/fs/mm/io/sighand/signal等信息
- 执行copy_thread_tls()，拷贝子进程的内核栈信息
- 执行alloc_pid()，为新进程分配新pid

缺点
----------------------------------------
不是线程安全的，fork 创建的子进程只有一个线程（调用线程的副本），当一个线程在 fork 时，如果另一个线程此时进行内存分配并持有堆锁，任何在子进程中分配内存的尝试（从而获得相同的锁）都将立即发生死锁。

fork对性能有较大的消耗。
