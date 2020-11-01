read
========================================
read的函数原型为 ``ssize_t read(int fd, void *buf, size_t nbytes);`` 。

read()返回值为读到的字节数，如果已经到文件末尾，返回0，若错误发生，返回-1。
