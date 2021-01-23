启动
========================================
接通电源后，计算机加载BIOS或UEFI，进行相应的启动程序。

而后BIOS/UEFI加载MBR，MBR调用GRUB，而后调用Linux内核，调用内核文件，之后执行 ``/sbin/init`` 作为所有系统进程的起点。 ``init`` 读取配置文件 ``/etc/inittab`` ，并根据 ``inittab`` 执行相应程序。

其中常见的内核文件有：

- vmlinux
    - 编译出来的最原始的内核文件，未压缩。
- zImage
    - vmlinux经过gzip压缩后的文件。
    - zImage解压缩内核到低端内存(第一个640K)
- bzImage
    - bz表示 ``big zImage``
    - bzImage解压缩内核到高端内存(1M以上)
- uImage
    - U-boot专用的映像文件，在zImage之前加上一个长度为0x40的tag
- vmlinuz
    - bzImage/zImage文件的拷贝或指向bzImage/zImage的链接。
- initrd
    - ``initial ramdisk`` 的简写
    - 一般被用来临时的引导硬件到实际内核vmlinuz能够接管并继续引导的状态

然后根据不同的运行级别执行 ``/etc/rcX.d/`` 目录下的文件， 例如运行级别为5时，执行 ``/etc/rc5.d/`` 下的文件，接着执行 ``/etc/rc.d/rc.local`` ，最后设置好tty，为用户登录做准备。
