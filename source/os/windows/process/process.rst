进程
========================================

基础
----------------------------------------
程序是静态的指令序列，而进程(Process)是一个应用程序运行的容器，拥有执行一个程序特定实例时需要的各种资源。进程包含以下一些基本组件：私有虚拟地址、可执行体程序、被操作系统分配的一份资源句柄(Handles)列表、访问控制令牌(Token)、进程标识号一个或多个线程。

访问控制令牌是用以唯一的标识所有者及其所属组以及和该进程相关联的特权(Privilege)信息。

创建
----------------------------------------
Windows 提供了多个创建进程的API。最简单的创建进程函数是CreateProcess，与创建进程使用相同的 access token。需要创建拥有特定 token 的进程，可以使用 CreateProcessAsUser。这两个函数由 Kernel32.dll 提供，在底层都是调用 Kernel32.dll 中的 CreateProcessInternal 。

还有一些函数例如 CreateProcessWithTokenW、CreateProcessWithLogonW (由 advapi32.dll 提供) 与 CreateProcessAsUser 类似，但是在调用的权限要求上有所不同。这两个函数都由 Secondary Logon service (seclogon.dll, 由svchost启动) 调用RPC完成。当调用相关函数时，SecLogon 服务会启动，实际则由 seclogon.dll 调用 CreateProcessAsUser 完成。

CreateProcessInternal 实际调用 NtDll.dll 中的 NtCreateUserProcess ，对应内核中的 NtCreateUserProcess 。

进程创建步骤分为七步：

- 转换并校验参数和标记
- 打开可执行映像文件(.exe)
- 创建Windows执行体进程对象
- 创建初始线程(栈，上下文和 执行体线程对象等)
- 通知Windows子系统初始化了一个进程
- 开始运行初始线程。(除非进程创建的时候被挂起)
- 在新进程和线程的上下文空间中，完成地址空间的初始化( 比如加载必需的DLL文件)并开始执行程序

其他进程创建
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
除了正常的进程之外，还有一些其他类型的进程。例如UWP（Windows 应用），原生进程、内核模式进程、微进程(Pico Process)等。

UWP同样由 CreateProcessAsUser 创建。而原生进程则由内核直接创建，并不调用 CreateProcess 等API，而是在内核中直接调用 NtCreateUserProcess 。另外，由于 CreateProcessInternal 会拒绝加载原生镜像，原生进程也不会由 Windows 应用加载。

内核进程则是由 NtCreateProcessEx 创建，由Windows子系统管理的微进程由 PspCreatePicoProcess 创建。

特殊进程
----------------------------------------

受保护的进程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
拥有调试权限(debug token)的进程可以申请调试其它进程的权限，进行修改内存、注入代码、暂停线程、查询信息等操作。

而由于一些进程的特殊性，例如数字版权管理 (Digital Rights Management, DRM) 等情况。Windows引入了受保护的进程 (Protected processes) 的概念。只有拥有微软特定签名的应用才可以启动称为受保护的进程。

常见的受保护的应用有 Audiodg 、Windows Error Reporting (WER) 等。

此外，为了保护系统进程的完整性，处于用户空间的内核代码也是受保护的。

在 Windows 8.1 / Windows Server 2012 R2 之后，引入了 Protected Process Light (PPL) 的概念，对 Protected Processes 做了扩展。

Minimal processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
当内核模式设置特定的 flag 并调用 NtCreateProcessEx 时，会执行 PsCreateMinimalProcess API ，会创建一个 小进程 (Minimal processes) 。

Minimal processes 的特点是：

- 不会设置用户模式的内存，也就没有 PEB 等结构
- 不会加载 NTDLL 
- 不会有 section 对象，即不会关联到可执行文件
- 向 EPROCESS 设置 Minimal flag，对应的线程也是 Minimal threads，没有 TEB 等结构

在Windows 10中，至少有 System process 和 Memory Compression process 两个 Minimal processes ，开启基于虚拟化的安全选项时 (Virtualization-Based Security) ，则还有 Secure System process 。

Pico processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
由于 Minimal processes 在用户空间的能力相对有限，Windows 引入了微进程的概念 (Pico processes)，Pico processes 由 Pico Provider 提供能力。

Pico Provider 提供了模拟其它内核的能力，让 Windows 可以兼容其它操作系统的可执行文件。

Provider 需要使用 PsRegisterPicoProvider 注册，Pico Provider 需要比其它所有的第三方驱动都先加载。对应的，这样的驱动需要微软的签名 (Microsoft Signer Certificate and Windows Component EKU)。

Pico Provider 将实现以下的函数：

- 用于创建 Pico process 的函数
- 用于获取 Pico process 上下文的函数
- 用于设置 Pico process 上下文的函数
- 用于创建 Pico thread 的函数
- 用于获取 Pico thread 上下文的函数
- 用于设置 Pico thread 上下文的函数
- ...

和 Pico Provider 直接相关的驱动是 Lxss.sys 与 LxCore.sys 。
