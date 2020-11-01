wait / waitpid
========================================

wait
----------------------------------------
wait的函数原型为 ``pid_t wait (int * status);`` ，相关的头文件是 ``#include<sys/types.h>`` / ``#include<sys/wait.h>`` 。

进程调用wait后，就立即阻塞自己，由wait自动分析是否当前进程的某个子进程已经退出，如果找到了这样一个已经变成僵尸的子进程，wait就会收集这个子进程的信息，并把它彻底销毁后返回。如果没有找到这样的子进程，wait就会一直阻塞直到出现一个这样的子进程为止。

子进程的结束状态值会由参数status返回，如果不在意结束状态值，则参数status可以设成NULL。

如果执行成功则返回子进程识别码(PID)，如果有错误发生则返回-1。失败原因存于errno中。

waitpid
----------------------------------------
waitpid的函数原型为 ``pid_t waitpid(pid_t pid,int * status,int options);`` ，相关的头文件是 ``#include<sys/types.h>`` / ``#include<sys/wait.h>`` 。

waitpid会暂时停止目前进程的执行，直到有信号来到或子进程结束。如果在调用waitpid时子进程已经结束，则waitpid会立即返回子进程结束状态值。子进程的结束状态值会由参数status返回，如果不在意结束状态值，则参数status可以设成NULL。

参数pid 为欲等待的子进程识别码，其他数值意义如下：

- pid < -1
    - 等待进程组识别码为pid绝对值的任何子进程
- pid = -1
    - 等待任何子进程，相当于wait
- pid = 0
    - 等待进程组识别码与目前进程相同的任何子进程
- pid > 0
    - 等待任何子进程识别码为pid的子进程

参数option可以为0或下面的OR组合：

- WNOHANG
    - 如果没有任何已经结束的子进程则马上返回，不予以等待。
- WUNTRACED
    - 如果子进程进入暂停执行情况则马上返回，但结束状态不予以理会

子进程的结束状态返回后存于status，可用下面几个宏判别结束情况：

- WIFEXITED(status)
    - 如果子进程正常结束则为非0值
- WEXITSTATUS(status)
    - 取得子进程exit()返回的结束代码，一般会先用WIFEXITED来判断是否正常结束才能使用此宏
- WIFSIGNALED(status)
    - 如果子进程是因为信号而结束则此宏值为真
- WTERMSIG(status)
    - 取得子进程因信号而中止的信号代码，一般会先用WIFSIGNALED来判断后才使用此宏
- WIFSTOPPED(status)
    - 如果子进程处于暂停执行情况则此宏值为真。一般只有使用WUNTRACED时才会有此情况
- WSTOPSIG(status)
    - 取得引发子进程暂停的信号代码，一般会先用WIFSTOPPED来判断后才使用此宏

如果执行成功则返回子进程识别码(PID)，如果有错误发生则返回-1。失败原因存于errno中。
