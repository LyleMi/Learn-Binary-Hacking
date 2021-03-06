vfork
========================================

简介
----------------------------------------
vfork创建的子进程与父进程共享数据段，而且由vfork()创建的子进程将先于父进程运行。

- 由vfork创造出来的子进程还会导致父进程挂起，除非子进程exit或者execve才会唤起父进程
- 由vfok创建出来的子进程共享了父进程的所有内存，包括栈地址，直至子进程使用execve启动新的应用程序为止
- 由vfork创建出来的子进程不应该使用return返回调用者，或者使用exit()退出，但是可以使用_exit()函数来退出

源码分析
----------------------------------------
.. code-block:: cpp

    SYSCALL_DEFINE0(vfork)
    {
      return _do_fork(CLONE_VFORK | CLONE_VM | SIGCHLD, 0,
          0, NULL, NULL, 0);
    }

与fork区别
----------------------------------------
- fork子进程拷贝父进程的数据段、代码段，vfork子进程与父进程共享数据段
- fork父子进程的执行次序不确定，vfork保证子进程先运行
