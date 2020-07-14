WMI
========================================

简介
----------------------------------------
Windows Management Instrumentation (WMI) 是 Web-Based Enterprise Management (WBEM)的一个实现，从 Windows NT 4.0 开始出现在所有的 Windows 操作系统中，用于提供操作界面和对象模式以便访问有关操作系统、设备、应用程序和服务的管理信息。

WMI由4个部分组成，分别为：management applications, WMI infrastructure, providers, and managed objects。

工具
----------------------------------------

wmic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
wmic 是一个与 WMI 进行交互的命令行工具，拥有大量的 WMI 对象的方便记忆的默认别名。

wbemtest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
wbemtest 是一个带有图形界面的 WMI 诊断工具。它能够枚举对象实例、执行查询、注册事件、修改 WMI 对象和类，并且可以在本地或远程去调用方法。

winrm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
winrm 是 WS-Management 协议的 Microsoft 实现，该协议为使用 Web 服务的本地计算机和远程计算机之间的通信提供了一种安全的方式。

wmic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
wmic 是一个简单的 Linux 命令行工具，用于执行 WMI 查询。
