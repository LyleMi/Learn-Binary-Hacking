Linux
========================================


.. toctree::
   :maxdepth: 2
   :caption: 目录:

   source
   fs
   addr
   mitigation
   syscall

简介
----------------------------------------
Linux内核在1991年首次被Linus发布，采用GPL(GNU General Public License)许可证开源。

Linux内核采用单内核结构，也借鉴类微内核的优点：模块、抢占式内核、支持内核线程以及动态装载内核模块。

Linux的版本以 ``kernel_version.major_revision.minor_revision`` 的格式表示，其中偶数 ``major_revision`` 代表稳定版，奇数代表开发版。

Linux的所有历史版本都可以在 `这里 <https://mirrors.edge.kernel.org/pub/linux/kernel/>`_ 找到。

启动
----------------------------------------
接通电源后，计算机加载BIOS或UEFI，进行相应的启动程序。

而后BIOS/UEFI加载Linux内核，执行init作为所有系统进程的起点。 init 读取配置文件 ``/etc/inittab`` ，并根据 ``inittab`` 执行相应程序。

然后根据不同的运行级别执行 ``/etc/rcX.d/`` 目录下的文件， 例如运行级别为5时，执行 ``/etc/rc5.d/`` 下的文件，接着执行 ``/etc/rc.d/rc.local`` ，最后设置好tty，为用户登录做准备。

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
- `Linux 文件系统剖析 <https://www.ibm.com/developerworks/cn/linux/l-linux-filesystem/>`_
