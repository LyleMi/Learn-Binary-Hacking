seek
========================================

lseek 函数的原型为

.. code-block:: cpp

    #include <unistd.h>
    #include <sys/types.h>

    off_t lseek(int fd, off_t offset, int whence);

参数 offset 的含义取决于参数 whence：

- ``whence`` 为 ``SEEK_SET`` ，设置偏移量为 offset
- ``whence`` 是 ``SEEK_CUR``，设置当前偏移加 offset，offset 可以为负
- ``whence`` 为 ``SEEK_END`` ，设置当前偏移为文件长度加 offset，offset 可以为负

成功时返回新的偏移，否则返回 -1 。
