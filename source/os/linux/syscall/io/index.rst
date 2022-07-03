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

除了系统调用对应的函数外，C 同样提供了 fopen / fdopen / fclose / fgetc / fputc 等函数，这些函数是对系统调用的封装，在用户层实现了 I/O 的缓冲。这些函数也被称为 标准I/O 。
