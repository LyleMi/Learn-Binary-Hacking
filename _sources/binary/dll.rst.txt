DLL
====================================

简介
------------------------------------
dll是被映射到exe进程的地址空间中去，换句话说，它可以被认为寄生在exe之上，自己并不独立存在。
而对于需要独立执行的dll，Windows使用svchost.exe、dllhost.exe和rundll32.exe来执行这些dll。


svchost.exe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

官方的解释是 ``svchost.exe is a generic host process name for services that run from dynamic-link libraries.``


dllhost.exe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
它的存在是为了容纳COM组件。

rundll32.exe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
rundll是Windows系统自带的一个直接执行DLL中导出函数的小工具

ref
---------------------------------

https://zhuanlan.zhihu.com/p/30000572