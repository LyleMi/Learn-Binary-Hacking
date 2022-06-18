Windows 容器
========================================
随着云计算的不断发展，Windows也引入了容器技术，在Windows中，Docker可以以两种模式运行：

- 部署一个重量级的、完全隔离的，基于 Hyper-V 的容器。桌面版和服务器版本都支持
- 部署一个轻量级的、基于OS隔离的，服务器容器 (server silo container)。仅支持有对应 license 的服务器版本。

Silo
----------------------------------------
silo实际上基于一个特殊的、配置了额外规则和能力的job。silo也可以分为 application silo 和 server silo。

Silo 的隔离使用以下的技术完成：

- 被称为 base OS 的基础 Windows 镜像 (WIM) 
- 宿主操作系统的 Ntdll
- 基于 Wcifs.sys 驱动的虚拟文件系统沙箱
- 基于 VReg 内核模块的虚拟注册表沙箱
- Session Manager (smss.exe)

每一个 Server Silo 都会拥有以下的隔离的属性：

- 用户数据：包含客制化的系统路径、session id, pid 等
- 对象目录命名空间：包含 \SystemRoot \Device 等目录
- 基于 base OS WIM 的 API 集合
- Logon Session
- ETW Tracing 与 logger 上下文
