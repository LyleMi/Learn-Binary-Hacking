IO相关系统调用
========================================

.. toctree::
   :maxdepth: 2
   :caption: 目录:

   open
   read
   write
   close
   dup
   fcntl
   seek
   sync
   select
   poll
   epoll

除了系统调用对应的函数外，C 同样提供了 fopen / fdopen / fclose / fgetc / fputc 等函数，这些函数是对系统调用的封装，在用户层实现了 I/O 的缓冲。这些函数也被称为 标准I/O 。

对于I/O唤醒，最经典的方式是IO多路复用，常见的系统调用有 select / poll / epoll 。
此外还有 FreeBSD 独有的 kqueue 。
