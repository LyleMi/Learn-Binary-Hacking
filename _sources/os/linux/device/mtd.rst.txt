MTD
========================================

简介
----------------------------------------
内存技术设备(Memory Technology Device, MTD)是Linux的存储设备中的一个子系统。设计此系统的目的是，对于内存类的设备提供一个抽象层的接口。

MTD设备通常可分为四层，依次为：设备节点、MTD设备层、MTD原始设备层和硬件驱动层。

设备节点通过mknod在/dev子目录下建立MTD块设备节点（主设备号为31）和MTD字符设备节点（主设备号为90）。通过访问此设备节点即可访问MTD字符设备和块设备。MTD设备层定义出MTD的块设备（主设备号31）和字符设备（设备号90）。原始设备层定义了大量的关于MTD的数据和操作函数。硬件驱动层负责对硬件的读、写和擦除操作。

源码
----------------------------------------
Linux中MTD的所有源码位于 ``/drivers/mtd`` 子目录下。

其中目录如下:

- drivers/mtd/chips: CFI/jedec接口通用驱动
- drivers/mtd/nand: nand通用驱动和部分底层驱动程序
- drivers/mtd/maps: nor flash映射关系相关函数
- drivers/mtd/devices: nor flash底层驱动

主要文件为:

- mtdcore.c:  MTD原始设备接口相关实现
- mtdpart.c:  MTD分区接口相关实现
- mtdchar.c:  MTD字符设备接口相关实现，设备号31
- mtdblock.c: MTD块设备接口相关实现，设备号90

dev
----------------------------------------
/dev/mtdN是Linux中的MTD架构中，系统自己实现的mtd分区所对应的字符设备，其里面添加了一些ioctl，支持很多命令，如MEMGETINFO，MEMERASE等。mtdN系列字符设备的主设备号为90，次设备号为0、2、4、6。

``/dev/mtdN`` 和 ``/dev/mtdblockN`` 是同一个MTD设备的同一个分区，mtdblockN 块设备的主设备号为31，次设备号为0、1、2、3。
