进程启动
========================================

_start函数
----------------------------------------
在加载器完成堆栈设置，argc、argv、envp设置、文件描述符配置之后，加载器会完成重定位，调用预置的初始化函数，并在最后将程序控制权交还给 ``_start`` 。

__libc_start_main
----------------------------------------
``__libc_start_main`` 的定义如下：

.. code-block:: cpp

    int __libc_start_main( 
        int (*main) (int, char * *, char * *),
        int argc, char * * ubp_av,
        void (*init) (void),
        void (*fini) (void),
        void (*rtld_fini) (void),
        void (*stack_end)
    );

其中 ``stack_end`` 是已经对齐的栈指针。


``__libc_start_main`` 的的主要功能为：

- 处理 ``setuid`` / ``setgid`` 程序的安全问题
- 启动线程
- 把 ``fini`` 函数和 ``rtld_fini`` 函数作为参数传递给 ``at_exit`` 调用，使它们在 ``at_exit`` 里被调用，从而完成用户程序和加载器的调用结束之后的清理工作
- 调用 ``main`` 函数
- 调用 ``exit`` 函数，并将main函数的返回值传递给它
