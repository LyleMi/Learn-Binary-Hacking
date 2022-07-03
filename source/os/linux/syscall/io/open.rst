open / openat
========================================
当传给函数的路径名是绝对路径时，open与openat无区别，此时openat自动忽略第一个参数fd。

当传给函数的是相对路径时，如果openat()函数的第一个参数fd是常量AT_FDCWD时，则其后的第二个参数路径名是以当前工作目录为基址的；否则以fd指定的目录文件描述符为基址。

.. code-block:: cpp

  int open(const char *path, int oflag);
  int open(const char *path, int oflag, mode_t mode);
  int openat(int fd, const char *path, int oflag, mode_t mode);

其中oflag可取的值如下：

- O_RDONLY: 只读打开
- O_WRONLY: 只写打开
- O_RDWR: 读，写打开
- O_CREAT: 若文件不存在，则创建它，需要使用mode选项。来指明新文件的访问权限
- O_APPEND: 追加写，如果文件已经有内容，这次打开文件所写的数据附加到文件的末尾而不覆盖原来的内容
- O_DIRECT：对文件的操作不使用系统缓存，需要手动字节对齐
- O_SYNC：使用系统缓存、所有操作落盘
- DEFAULT：类似 O_SYNC ，操作不一定落盘

其中mode可取的值如下：

- S_IRUSR
    - 文件所有者有读 (r) 权限
- S_IWUSR
    - 文件所有者有写 (w) 权限
- S_IRGRP
    - 文件所属组有读 (r) 权限
- S_IWGRP
    - 文件所属组有写 (w) 权限
- S_IROTH
    - 文件所属other有读 (r) 权限
- S_IWOTH
    - 文件所属other有写 (w) 权限
