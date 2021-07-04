mmap系列函数
========================================

mmap
----------------------------------------
内存映射函数mmap负责把文件内容映射到进程的虚拟内存空间，通过对这段内存的读取和修改，来实现对文件的读取和修改，而不需要再调用read/write等操作。

.. code-block:: cpp

    void* mmap(void * addr, size_t len, int prot, int flags, int fd, off_t offset)

对应头文件为 ``#include <sys/mman.h>`` 。

其中addr指定映射的起始地址, 通常设为NULL, 由内核指定；length表示映射到内存区域的大小。

prot映射区的保护方式，可以是：

- PROT_EXEC: 映射区可被执行
- PROT_READ: 映射区可被读取
- PROT_WRITE: 映射区可被写入
- PROT_NONE: 映射区域不能存取

flags表示映射区的特性，可以是:

- MAP_FIXED: 如果参数 ``start`` 所指的地址无法成功建立映射时，则放弃映射，不对地址做修正，通常不鼓励用此flag。
- MAP_FIXED_NOREPLACE: (Linux 4.17后) 功能与 MAP_FIXED 类似。不同点在于，如果页已经存在，则不会建立映射。
- MAP_SHARED: 对映射区域的写入数据会复制回文件内，而且允许其他映射该文件的进程共享。
- MAP_SHARED_VALIDATE: (Linux 4.15后) 与 MAP_SHARED 类似，但是会检查其他 flag
- MAP_PRIVATE: 对映射区域的写入操作会产生一个映射文件的复制，即私有的写入时复制(copy-on-write)对此区域作的任何修改都不会写回原来的文件内容。
- MAP_ANONYMOUS: 建立匿名映射。此时会忽略参数fd，不涉及文件，而且映射区域无法和其他进程共享。
- MAP_DENYWRITE: 只允许对映射区域的写入操作，其他对文件直接写入的操作将会被拒绝。
- MAP_LOCKED: 将映射区域锁定住，这表示该区域不会被置换（swap）。
- MAP_POPULATE: 预先读需要的页，减少之后的发生页错误时的消耗
- MAP_UNINITIALIZED: (Linux 2.6.33后可以使用) 不初始化页，仅在内核配置了 ``CONFIG_MMAP_ALLOW_UNINITIALIZED`` 时可以使用，主要用于优化嵌入式设备的性能

fd是由open返回的文件描述符, 代表要映射的文件，如果使用匿名内存映射时，fd设为-1。当系统不支持匿名内存映射时，则可以使用fopen打开/dev/zero文件。

offset表示以文件开始处的偏移量，必须是分页大小的整数倍，通常为0，表示从文件头开始映射。

若映射成功则返回映射区的内存起始地址，否则返回MAP_FAILED(-1)，错误原因存于errno 中。

具体的错误代码如下:

- EBADF: 参数fd 不是有效的文件描述词
- EACCES: 存取权限有误。如果是MAP_PRIVATE 情况下文件必须可读，使用MAP_SHARED则要有PROT_WRITE以及该文件要能写入
- EINVAL: 参数start、length 或offset有一个不合法
- EAGAIN: 文件被锁住，或是有太多内存被锁住
- ENOMEM: 内存不足

munmap
----------------------------------------
解除映射可使用 ``munmap`` 函数，原型为

.. code-block:: cpp

    int munmap(void *start, size_t length);

msync
----------------------------------------
进程在映射空间的对共享内容的改变往往在调用 ``munmap`` 后才写回到磁盘文件中，如果要在这之前同步，可以使用 ``msync`` ，其函数原型为：

.. code-block:: cpp

    int msync(void * addr, size_t len, int flags);

其中addr为需要同步的地址，len表示要同步数据的大小。flags 取值如下：

- MS_ASYN: 异步写
- MS_SYN: 同步写
- MS_INVALIDAT: 无效的cache 数据
