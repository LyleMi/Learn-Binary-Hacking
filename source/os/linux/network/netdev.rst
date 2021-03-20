网络设备
========================================

网桥
----------------------------------------
网桥(Bridge)也称桥接器，是连接两个局域网的存储转发设备，用它可以完成具有相同或相似体系结构网络系统的连接。

Linux网络协议栈已经支持了网桥的功能，但需要进行相关的配置才可以进行正常的转发。而要配置Linux网桥功能，需要配置工具bridge-utils，生成网桥配置的工具有brctl、ip等。

网桥可以理解为实现在内核中的二层交换机。

配置方法

::

    ip link add br0 type bridge
    ip link set br0 up
    ip link set br0 down

tun/tap
----------------------------------------
tun/tap设备支持很多的类UNIX平台。tun表示虚拟的是点对点设备，tap表示虚拟的是以太网设备。tap位于网络OSI模型的二层，tun位于网络的三层。

veth-pair
----------------------------------------
Virtual Ethernet Pair简称veth pair，是一对的虚拟设备接口，是一个成对的端口。所有从一端进入的数据包都将从另一端出来。

veth 可以通过 ``ip link add eth1-br1 type veth peer name phy-br1`` 命令创建。
