DLL
========================================

由于DLL分布在各个位置，不能在链接时硬编码，因此在加载时使用动态解析的方法 (name resolution)。

在解析二进制文件的依赖时，Windows在搜索路径中查找文件。搜索DLL路径的优先级如下：

- 启动的进程的位置
- Windows系统目录 (例如 C:\Windows\System32)
- 16位 Windows系统目录 (例如 C:\Windows\System)
- Windows目录 (例如 C:\Windows)
- 当前目录
- %PATH% 环境变量指定的位置
