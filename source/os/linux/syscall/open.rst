open / openat
========================================
当传给函数的路径名是绝对路径时，open与openat无区别，此时openat自动忽略第一个参数fd。

当传给函数的是相对路径时，如果openat()函数的第一个参数fd是常量AT_FDCWD时，则其后的第二个参数路径名是以当前工作目录为基址的；否则以fd指定的目录文件描述符为基址。

.. code-block:: cpp

  int open(const char *path, int oflag, mode_t mode);
  int openat(int fd, const char *path, int oflag, mode_t mode);
