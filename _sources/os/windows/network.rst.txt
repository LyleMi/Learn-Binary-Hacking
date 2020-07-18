网络机制
========================================

Windows的网络结构和组件
----------------------------------------
- 网络应用程序与服务进程
    - 对应OSI应用层，通常使用各类网络API  DLL来实现网络交互与通信功能。
- 网络API DLL及TDI客户端
    - 对应OSI会话层与表示层，为应用程序提供 了独立于具体协议的网络交互实现方式，包括Windows套接字（Winsock）、远程过程 调用（RPC）、Web访问API、命名管道和 邮件槽以及其他网络API接口。
    - TDI客户端作为内核模式驱动程序，是网络 API接口内核部分的具体实现：将网络API的请求转换成IRP，通过TDI标准格式化后，发 送给下层的协议驱动（TDI传输器）。

TDI
----------------------------------------
TDI（Transport Driver Interface）传输驱动程序接口，也被称之为TDI传输器、NDIS协议驱动程序 、协议驱动程序等，对应OSI的网络层和传输层，实现了TCP/IP、NetBEUI、IPX等协议栈，接受上层TDI客户端的IRP请求，并 调用NDIS库中提供的网卡驱动程序功能进行网络传输。

TDI传输器通过透明地进行如分片与重组、排序确认与重传等一 系列消息操作，为上层网络应用提供了便捷的支持。
Windows Vista之后，不再使用TDI，而是Windows filter  platform（WFP）和Winsock kernel（WSK）了。

WSK，Winsock内核：提供内核模式下的网络通信，使用类似 于用户态Winsock的编程语法，但也提供IRP和事件回调函数的 异步I/O操作等特性。WSK还在Windows下一代TCP/IP网络协议 栈中默认支持IPv6功能。

WFP，Windows过滤平台：一套API函数和系统服务，提供创建 网络过滤应用程序的能力，允许应用程序追踪、过滤甚至修改网 络数据包

NDIS
----------------------------------------
NDIS（Network Driver Interface Specification）库及小端 口（miniport）驱动程序位于OSI的链路层，为各种不同的网卡驱动程序和TDI传输层之间 构建一个封装接口。

NDIS库（ndis.sys）对于上层为网络程序模块屏蔽了不同的网 卡硬件类型，NDIS miniport驱动对于下层，为网卡制造商开发设备驱动程序提供了屏蔽Windows内核环境细节信息的编程接口。

NDIS包含各种网卡硬件的设备驱动程序，处于OSI模型的物理层。
