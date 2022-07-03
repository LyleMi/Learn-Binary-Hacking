fcntl
========================================
fcntl用于根据文件描述词来操作文件的特性，函数原型如下：

.. code-block:: cpp

    #include <fcntl.h>; 
    int fcntl(int fd, int cmd); 
    int fcntl(int fd, int cmd, long arg); 
    int fcntl(int fd, int cmd, struct flock *lock); 

fcntl函数有5种功能： 

- 复制一个现有的描述符 ``cmd=F_DUPFD``
- 获得/设置文件描述符标记 ``cmd=F_GETFD或F_SETFD``
- 获得/设置文件状态标记 ``cmd=F_GETFL或F_SETFL``
- 获得/设置异步I/O所有权 ``cmd=F_GETOWN或F_SETOWN``
- 获得/设置记录锁 ``cmd=F_GETLK,F_SETLK或F_SETLKW``

cmd 选项有

- F_DUPFD 返回一个如下的描述符:                            
    - 最小的大于或等于arg的一个可用的描述符                          
    - 与原始操作符一样的某对象的引用               
    - 如果对象是文件的话,返回一个新的描述符，这个描述符与arg共享相同的偏移量(offset)
    - 相同的访问模式(读、写或读/写)                          
    - 相同的文件状态标志(如:两个文件描述符共享相同的状态标志)                            
    - 与新的文件描述符结合在一起的close-on-exec标志被设置成交叉式访问execve系统调用
- F_GETFD
    - 取得与文件描述符fd联合close-on-exec标志，类似FD_CLOEXEC
    - 如果返回值和FD_CLOEXEC进行与运算结果是0的话，文件保持交叉式访问exec()
    - 否则如果通过exec运行的话，文件将被关闭(arg被忽略)                  
- F_SETFD
    - 设置close-on-exec。该flag以参数arg的FD_CLOEXEC位决定                  
- F_GETFL
    - 取得fd的文件状态标志，如同下面的描述一样(arg被忽略)                    
- F_SETFL
    - 设置给arg描述符状态标志，可以更改的几个标志是：O_APPEND、O_NONBLOCK、O_SYNC和O_ASYNC
- F_GETOWN
    - 取得当前正在接收SIGIO或者SIGURG信号的进程id或进程组id，进程组id返回成负值(arg被忽略)                    
- F_SETOWN
    - 设置将接收SIGIO和SIGURG信号的进程id或进程组id，进程组id通过提供负值的arg来说明，否则arg将被认为是进程id
              
F_GETFL和F_SETFL的标志如下面的描述:            

- O_NONBLOCK
    - 非阻塞I/O
    - 如果read调用没有可读取的数据或者如果write操作将阻塞，read或write调用返回-1和EAGAIN错误
- O_APPEND
    - 强制每次写(write)操作都添加在文件大的末尾，相当于open的O_APPEND标志         
- O_DIRECT
    - 最小化或去掉reading和writing的缓存影响，系统将企图避免缓存你的读或写的数据
    - 如果不能够避免缓存，那么它将最小化已经被缓存了的数 据造成的影响.如果这个标志用的不够好，将大大的降低性能                      
- O_ASYNC
    - 当I/O可用的时候，允许SIGIO信号发送到进程组，例如当有数据可以读的时候

fcntl的返回值与命令有关。如果出错，所有命令都返回-1，如果成功则返回某个其他值。下列三个命令有特定返回值：F_DUPFD、F_GETFD、F_GETFL以及F_GETOWN。第一个返回新的文件描述符，第二个返回相应标志，最后一个返回一个正的进程ID或负的进程组ID。
