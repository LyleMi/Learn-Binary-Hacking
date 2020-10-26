execve
========================================
exec系列系统调用的原型如下：

.. code-block:: cpp

    int execl(const char *path, const char *arg, ...);
    int execle(const char *path, const char *arg, ..., char * const envp[]);
    int execlp(const char *file, const char *arg, ...);
    int execv(const char *path, char *const argv[]);
    int execvp(const char *file, char *const argv[]);
    int execvpe(const char *file, char *const argv[], char *const envp[]);

其中path参数表示要启动程序的完整路径，file表示程序文件名，argv表示执行参数，一般argv第一个参数为要执行命令名，不是带路径且argv必须以NULL结束，envp表示环境变量。

上述exec系列函数底层都是通过execve系统调用实现， ``int execve(const char *filename, char *const argv[], char *const envp[]);`` 。
