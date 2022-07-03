sync 系列
========================================
sync 系列函数有 ``sync``  ``fsync``  ``fdatasync`` ，性能消耗依次降低。

sync 函数原型为 ``void sync (void);`` 用于将内核缓冲区的所有数据写回磁盘，一般不建议使用。

fsync 函数原型为 ``int fsync (int fd);``  ，要求传入一个以写权限打开的 fd ，将文件描述符对应的内核缓冲区数据写回磁盘。

fdatasync 函数原型为 ``int fdatasync (int fd);``  ，和 fsync 不同的是，fdatasync 只会更新文件数据而不会更新文件的元数据（时间戳等）。
