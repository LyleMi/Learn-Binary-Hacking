write
========================================
write的函数原型为 ``ssize_t write(int fd, void *buf, size_t nbytes);`` 。

write()返回值通常与参数nbytes相同，否则可能出错。出错存在两种情况，返回-1时表示写操作失败，返回值小于nbytes时，只写入了部分数据。
