syscall
========================================

简介
----------------------------------------
syscall是内核提供用户空间程序与内核空间进行交互的一套标准接口，这些接口让用户态程序能受限访问硬件设备，比如申请系统资源、操作设备读写、创建新进程等。用户空间发起请求，内核空间负责执行，这些接口便是用户空间和内核空间共同识别的桥梁。

Syscall是通过中断方式实现的，用户程序通过软中断切入内核态，执行指令。Linux内核中，每个Syscall都有唯一的系统调用号对应。

系统调用列表可在 ``linux/arch/sh/include/uapi/asm/unistd_64.h`` 中找到。

ioctl
----------------------------------------
ioctl 是设备驱动程序中设备控制接口函数，一个字符设备驱动通常会实现设备打开、关闭、读、写等功能，在一些需要细分的情境下，如果需要扩展新的功能，通常以增设 ioctl 命令的方式实现。

.. code-block:: cpp

    #include <sys/ioctl.h> 
    int ioctl(int fd, int cmd, ...) ;

其中fd是文件描述符，cmd是交互协议，设备驱动将根据 cmd 执行对应操作，最后是可变参数，依赖 cmd 指定长度以及类型。ioctl 函数执行成功时返回 0，失败则返回 -1 并设置全局变量 errorno 值。

fcntl
----------------------------------------
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

execve
----------------------------------------
``execve`` 系统调用的原型如下：

.. code-block:: cpp

    int execve(const char *filename, char *const argv[], char *const envp[]); 

它所对应的三个参数分别是程序文件名，执行参数，环境变量。

select
----------------------------------------
select是IO多种复用的一种实现，它将需要监控的fd分为读，写，异常三类，其返回时是读、写、异常事件发生或者超时。

``select`` 系统调用的原型如下：

.. code-block:: cpp

    int select (int __nfds, fd_set *__restrict __readfds,
               fd_set *__restrict __writefds,
               fd_set *__restrict __exceptfds,
               struct timeval *__restrict __timeout);

open / openat
----------------------------------------
当传给函数的路径名是绝对路径时，open与openat无区别，此时openat自动忽略第一个参数fd。

当传给函数的是相对路径时，如果openat()函数的第一个参数fd是常量AT_FDCWD时，则其后的第二个参数路径名是以当前工作目录为基址的；否则以fd指定的目录文件描述符为基址。

.. code-block:: cpp

  int open(const char *path, int oflag, mode_t mode);
  int openat(int fd, const char *path, int oflag, mode_t mode);

mmap
----------------------------------------
内存映射函数mmap负责把文件内容映射到进程的虚拟内存空间，通过对这段内存的读取和修改，来实现对文件的读取和修改，而不需要再调用read/write等操作。

.. code-block:: cpp

    void* mmap(void * addr, size_t len, int prot, int flags, int fd, off_t offset)

其中addr指定映射的起始地址, 通常设为NULL, 由内核指定。
length表示映射到内存区域的大小。

prot映射区的保护方式，可以是：

- PROT_EXEC: 映射区可被执行
- PROT_READ: 映射区可被读取
- PROT_WRITE: 映射区可被写入
- PROT_NONE: 映射区域不能存取

flags表示映射区的特性，可以是:

- MAP_FIXED: 如果参数 ``start`` 所指的地址无法成功建立映射时，则放弃映射，不对地址做修正，通常不鼓励用此flag。
- MAP_SHARED: 对映射区域的写入数据会复制回文件内，而且允许其他映射该文件的进程共享。
- MAP_PRIVATE: 对映射区域的写入操作会产生一个映射文件的复制，即私有的写入时复制(copy-on-write)对此区域作的任何修改都不会写回原来的文件内容。
- MAP_ANONYMOUS: 建立匿名映射。此时会忽略参数fd，不涉及文件，而且映射区域无法和其他进程共享。
- MAP_DENYWRITE: 只允许对映射区域的写入操作，其他对文件直接写入的操作将会被拒绝。
- MAP_LOCKED: 将映射区域锁定住，这表示该区域不会被置换（swap）。

fd是由open返回的文件描述符, 代表要映射的文件，如果使用匿名内存映射时，fd设为-1。当系统不支持匿名内存映射时，则可以使用fopen打开/dev/zero文件。

offset表示以文件开始处的偏移量，必须是分页大小的整数倍，通常为0，表示从文件头开始映射。

若映射成功则返回映射区的内存起始地址，否则返回MAP_FAILED(-1)，错误原因存于errno 中。

具体的错误代码如下:

- EBADF: 参数fd 不是有效的文件描述词
- EACCES: 存取权限有误。如果是MAP_PRIVATE 情况下文件必须可读，使用MAP_SHARED则要有PROT_WRITE以及该文件要能写入
- EINVAL: 参数start、length 或offset有一个不合法
- EAGAIN: 文件被锁住，或是有太多内存被锁住
- ENOMEM: 内存不足

解除映射可使用 ``munmap`` 函数，原型为

.. code-block:: cpp

    int munmap(void *start, size_t length);
