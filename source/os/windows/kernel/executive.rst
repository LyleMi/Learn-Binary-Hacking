Executive
========================================
Windows executive 是 Ntoskrnl.exe 的上层。Executive包含下面几种函数：

- 暴露给用户模式调用的函数
- 通过 DeviceIoControl 调用的设备驱动函数
- 通过WDK暴露的，内核模式下调用的函数
- 未文档化的，内核模式下调用的函数
- 定义为的全局符号但是未导出的函数
- 属于模块内部的函数

Executive包含下面几个主要模块：

- 配置管理：系统注册表的实现与管理
- 进程管理
- Security Reference Monitor (SRM) 
- I/O管理
- Plug and Play (PnP)管理
- 电源管理
- Windows Driver Model (WDM)
- Windows Management Instrumentation (WMI) routines
- 内存管理
- 缓存管理
