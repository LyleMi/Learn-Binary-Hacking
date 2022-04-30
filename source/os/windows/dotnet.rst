.NET
========================================
+-------------------------+------------------------+
| Windows Version         | .Net Framework Version |
+=========================+========================+
| Windows 8               | 4.5                    |
+-------------------------+------------------------+
| Windows 8.1             | 4.5.1                  |
+-------------------------+------------------------+
| Windows 10              | 4.6                    |
+-------------------------+------------------------+
| Windows 10 version 1511 | 4.6.1                  |
+-------------------------+------------------------+
| Windows 10 version 1607 | 4.6.2                  |
+-------------------------+------------------------+

.NET 框架由 CLR (The Common Language Runtime) 和 FCL (The .NET Framework Class Library) 两个核心模块组成。

CLR 是 .NET 的运行时引擎，同时包含一个将 CIL (Common
Intermediate Language) 指令转义为机器语言的 JIT，还包含GC、类型检查等模块。
CLR 以 COM in-process server (DLL) 的形式实现。

FCL 包含了一些网络、数据库等函数接口的实现。

具体来说，一个 .NET 程序，自顶向下为 .NET APP (EXE) -> FCL (DLLs) -> CLR (COM DLL Server) -> Windows API DLLs -> Windows Kernel 。
