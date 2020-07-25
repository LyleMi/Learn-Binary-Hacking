Virtio
========================================

简介
----------------------------------------
virtio 是一种 I/O 半虚拟化解决方案，是一套通用 I/O 设备虚拟化的程序，是对半虚拟化 Hypervisor 中的一组通用 I/O 设备的抽象。提供了一套上层应用与各 Hypervisor 虚拟化设备（KVM，Xen，VMware等）之间的通信框架和编程接口，减少跨平台所带来的兼容性问题，大大提高驱动程序开发效率。

架构
----------------------------------------
virtio 可以分为四层，包括前端 guest 中各种驱动程序模块，后端 Hypervisor 上的处理程序模块，中间用于前后端通信的 virtio 层和 virtio-ring 层，virtio 这一层实现的是虚拟队列接口，算是前后端通信的桥梁，而 virtio-ring 则是该桥梁的具体实现，它实现了两个环形缓冲区，分别用于保存前端驱动程序和后端处理程序执行的信息。

设备类型
----------------------------------------

设备类型与ID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
============    ====================================
设备 ID         设备名称
============    ====================================
0               reserved (invalid)
1               network card
2               block device
3               console
4               entropy source
5               memory ballooning (traditional)
6               ioMemory
7               rpmsg
8               SCSI host
9               9P transport
10              mac80211 wlan
11              rproc serial
12              virtio CAIF
13              memory balloon
16              GPU device
17              Timer/Clock device
18              Input device 
============    ====================================



网络设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
virtio网络设备是虚拟以太网卡，是virtio支持的最复杂的设备。

块设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
virtio块设备是简单的虚拟块设备（即磁盘）。 读取和写入请求（以及其他请求）默认被放置在队列中，并由设备提供服务（可能是无序的）。

Console设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
virtio控制台设备是用于数据输入和输出的简单设备。 一台设备可以有一个或多个端口。 每个端口都有一对输入和输出虚拟队列。 此外，设备具有一对控制IO虚拟状态。 控制信息用于在设备和驱动程序之间传递有关在连接的任一侧打开和关闭端口的信息，从设备指示特定端口是否为控制台端口，添加新端口，端口热插拔/ 拔出等，并从驱动程序指示是否已成功添加端口或设备，端口打开/关闭等。对于数据IO，在接收队列中放置一个或多个空缓冲区，用于输入数据和输出字符。 放置在传输队列中。

熵设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
熵设备提供了高度的随机性。

Balloon 设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
通常来说，要改变客户机占用的宿主机内存，是要先关闭客户机，修改启动时的内存配置，然后重启客户机才能实现。而内存的ballooning（气球）技术可以在客户机运行时动态地调整它所占用的宿主机内存资源，而不需要关闭客户机。

SCSI Host 设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
virtio SCSI Host设备将一个或多个虚拟逻辑单元（例如磁盘）组合在一起，并允许使用SCSI协议与其通信。

参考链接
----------------------------------------

官方文档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- `virtio: Towards a De-Facto Standard For Virtual I/O Devices <https://www.ozlabs.org/~rusty/virtio-spec/virtio-paper.pdf>`_
- `Virtual I/O Device (VIRTIO) Version 1.0 <http://docs.oasis-open.org/virtio/virtio/v1.0/virtio-v1.0.html>`_
- `libvirt <https://wiki.libvirt.org/page/Main_Page>`_

Blog
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- `Virtio 简介 <https://www.cnblogs.com/bakari/p/8309638.html>`_
- `Virtio：针对 Linux 的 I/O 虚拟化框架 <https://www.ibm.com/developerworks/cn/linux/l-virtio/index.html>`_
