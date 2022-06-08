服务
========================================

简介
----------------------------------------
服务(Services)程序是后台运行的进程，常用来执行特定的任务，不需要和用户进行交互。比如自动更新服务、后台智能传输服务、事件日志服务等。

服务程序受Service Control Manager(SCM，即services.exe进程)所控制，其服务程序的配置数据位于 ``HKLM\System\CurrentControlSet\Services`` 。

服务通常由三个部分组成：服务应用、服务控制程序 (service control program, SCP)、服务控制管理器 (service control manager, SCM)。

Windows提供了内置的SCP，可以启动、停止、继续运行程序。用户也可以自定义SCP程序来玩橙更细粒度的控制。服务程序是普通的Windows可执行程序，只是会有一些附加模块和SCM通信。

Windows服务有三个名称，分别为在系统中执行的进程名、在注册表中的名称 (internal name)，在服务管理工具中的名字(display name)。当服务没有设置display name时，使用internal name做为替代。

需要注意的是，服务进程和运行的服务并不是一一对应的关系，可能多个服务会共享一个进程。这种共享关系是在注册表中定义的。
