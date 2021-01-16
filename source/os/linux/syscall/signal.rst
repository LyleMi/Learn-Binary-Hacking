signal系列调用
========================================

signal
----------------------------------------
signal系统调用用于为指定的信号安装新的处理句柄，信号处理句柄可以是用户指定的函数。

.. code-block:: cpp

    #include <signal.h>
    typedef void (*sighandler_t)(int);
    sighandler_t signal(int signum, sighandler_t handler); 

其中在 signal.h 头文件中还有以下几个常量的定义

.. code-block:: cpp

    #define SIG_ERR (void (*)())-1
    #define SIG_DFL (void (*)())0
    #define SIG_IGN (void (*)())1

sigaction
----------------------------------------
sigaction函数用来查询和设置信号处理方式，它是用来替换早期的signal函数。sigaction函数原型及说明如下：

.. code-block:: cpp

    #include <signal.h>

    struct sigaction
    {

        void (*sa_handler) (int);
        void  (*sa_sigaction)(int, siginfo_t *, void *);
        sigset_t sa_mask;
        int sa_flags;
        void (*sa_restorer) (void);

    }

    int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact)

其中signum可以指定SIGKILL和SIGSTOP以外的所有信号。sa_handler和signal()的参数handler相同。sa_sigaction用于得到信号编号，并获得被调用的原因以及产生问题的上下文的相关信息。sa_mask用来设置在处理该信号时暂时将sa_mask指定的信号搁置。

sa_flags用来设置信号处理的其他相关操作，可用 OR 运算（|）组合：

- A_NOCLDSTOP: 如果参数signum为SIGCHLD，则当子进程暂停时并不会通知父进程
- SA_ONESHOT/SA_RESETHAND: 当调用新的信号处理函数前，将此信号处理方式改为系统预设的方式
- SA_RESTART: 被信号中断的系统调用会自行重启
- SA_NOMASK/SA_NODEFER: 在处理此信号未结束前不理会此信号的再次到来
- SA_SIGINFO: 信号处理函数是带有三个参数的sa_sigaction

函数运行成功返回0，运行错误返回-1，错误原因存于error中。

错误信息：

- EINVAL: 参数signum不合法，或是企图拦截SIGKILL/SIGSTOP信号
- EFAULT: 参数act，oldact指针地址无法存取
- EINTR: 此调用被中断

信号集操作函数
----------------------------------------

信号集创建
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
信号集用来描述一类信号的集合，信号集操作函数用于把多个信号当作一个集合进行处理，信号集创建函数原型如下:

.. code-block:: cpp

    #include <signal.h>
    int sigemptyset(sigset_t *set)
    int sigfillset(sigset_t *set)
    int sigaddset(sigset_t *set, int signum)
    int sigdelset(sigset_t *set, int signum)
    int sigismember(sigset_t *set, int signum)

其中set表示信号集，signum表示信号值。

信号屏蔽位
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
函数原型如下:

.. code-block:: cpp

    #include <signal.h>
    int sigprocmask(int how, const sigset_t *set, sigset_t *oset);

其中how决定函数的操作方式，有三种取值：

- SIG_BLOCK: 增加一个信号集合到当前进程的阻塞集合之中
- SIG_UNBLOCK: 从当前的阻塞集合之中删除一个信号集合
- SIG_SETMASK: 将当前的信号集合设置为信号阻塞集合

set表示信号集，oset代表信号屏蔽字。

信号搁置查询
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
函数原型为 ``int sigpending(sigset_t *set)`` 。

