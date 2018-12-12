Linux
========================================

版本
----------------------------------------
Linux的版本以 ``kernel_version.major_revision.minor_revision`` 的格式表示，其中偶数 ``major_revision`` 代表稳定版，奇数代表开发版。

Linux的所有历史版本都可以在 `这里 <https://mirrors.edge.kernel.org/pub/linux/kernel/>`_ 找到。

防御措施
----------------------------------------

SMEP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SMEP（Supervisor Mode Execution Protection）是一种减缓内核利用的cpu策略，禁止内核态到用户态内存页的代码执行，每一页都有smep标识来标明是否允许ring0的代码执行。

内存管理
----------------------------------------

内存回收
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在运行时需要回收一些很少使用的内存页面来保证系统持续有内存使用。页面回收的方式有页回写、页交换和页丢弃三种方式。

页回写指一个很少使用的页的后备存储器是一个块设备（例如文件映射），则可以将内存直接同步到块设备，腾出的页面可以被重用。

页交换指页面没有后备存储器，则可以交换到特定swap分区，再次被访问时再交换回内存。

页丢弃指页面的后备存储器是一个文件，但文件内容在内存不能被修改（例如可执行文件），那么在当前不需要的情况下可直接丢弃。 

回收时间一般在内存紧缺时回收，或者周期性回收，或者由用户触发回收。

参考链接
----------------------------------------
- `The Linux Kernel Archives <https://www.kernel.org>`_
- `The Linux Kernel documentation <https://www.kernel.org/doc/html/latest/>`_
- `Linux内核调试 <https://xz.aliyun.com/t/2024>`_
- `利用KGDB调试内核驱动模块 <http://blog.nsfocus.net/gdb-kgdb-debug-application/>`_
- `Linux kernel pwn notes <http://www.mamicode.com/info-detail-2399209.html>`_
- `Linux kernel ROP <https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/linux-kernel-rop-ropping-your-way-to-part-1/>`_
- `slub算法 <https://blog.csdn.net/lukuen/article/details/6935068>`_
- `SMEP bypass <https://github.com/black-bunny/LinKern-x86_64-bypass-SMEP-KASLR-kptr_restric>`_
