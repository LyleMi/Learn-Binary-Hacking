子系统
========================================
Windows中每个可执行文件都与一个环境子系统(Environment subsystems)相对应。在可执行文件执行时，系统根据文件头中的信息对应找到环境子系统。

可执行文件在对应调用DLL时，也是调用对应子系统的DLL(subsystem DLL)。具体来说，调用分为三种情况：

- 函数仅涉及子系统的用户模式
- 函数需要子系统的系统进程
- 函数涉及到Windows系统调用，例如ReadFile/WriteFile

子系统由 Session Manager (Smss.exe) 进程启动，相关信息在注册表 HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\SubSystems 中。

