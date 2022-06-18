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

在第一步时，会对应设置进程的优先级，值可以为：

- Idle or Low (4)
- Below Normal (6)
- Normal (8)
- Above Normal (10)
- High (13)
- Real-time (24)

如果没有设置该Flag，默认优先级为 Normal 。

在第二步中，如果可执行文件是 bat / cmd 等特殊文件，会执行 cmd.exe ；如果是 Win16 (Windows 3.1) / DOS 可执行文件 / COM / pif 等，则会执行 NtVdm.exe ；其它条件下，会直接执行原本的 exe 文件。

如果该进程设置了受保护的标志位，则检查签名也在这一步完成。

第三步可以细分为：

- 设置 EPROCESS 对象
    - 如果没有显式设置，则继承父进程的属性、邮件及设置等
    - 设置进程 exit status 为 STATUS_PENDING
    - 存储父进程的ID
    - 检查并申请进程运行所需的权限，创建 primary access token
- 创建初始化进程空间
    - 创建的空间包括：page directory / hyperspace page / VAD bitmap page / working set list
- 初始化内核进程结构 KPROCESS
    - 设置优先级
- 完成进程地址空间的设置
- 设置 PEB
- 完成执行进程对象的设置

在第四步时，Windows执行体对象已经建立，但是线程未初始化。此时会由 NtCreateThread 调用 PspCreateThread 完成线程的初始化。PspCreateThread 依赖于 PspAllocateThread 和 PspInsertThread 。PspAllocateThread 处理线程实际初始化的部分。PspInsertThread 处理线程句柄的创建、安全属性，而后调用 KeStartThread 将执行对象变为操作系统可以调度的线程。

PspAllocateThread 实现了以下操作：

- 创建并初始化线程执行对象
- 设置线程创建时间、线程id (TID)
- 初始化线程的堆栈和上下文
- 创建 TEB 结构体
- 将进程的开始地址存储到 ETHREAD 中
- 调用 KeInitThread 设置 KTHREAD 结构体
- 如果线程是 UMS 线程，调用 PspUmsInitThread 用于初始化 UMS 状态

PspAllocateThread 操作结束后，NtCreateUserProcess 调用 PspInsertThread 进行以下操作:

- 检查进程、线程是否已经终止或者线程是否已经启动，如果存在上述情况，取消线程创建
- 调用 KeStartThread 初始化 KTHREAD 的线程对象
- 在 non-x86 系统中，如果该线程是进程的第一个线程，将进程插入到 KiProcessListHead 中
- 增加线程计数器，继承进程的IO优先级
- 将线程插入到进程的线程列表
- 如果线程是进程的第一个线程，进程注册的创建时回调将被调用

NtCreateUserProcess 响应后， CreateProcessInternalW 进行子系统相关的操作。

在这一步中，主要进行相关的检查，例如可执行文件头、证书（根据组策略）。收集一系列的信息并发送至 Csrss 。信息包括：

- path name 与 SxS path name
- 进程与线程句柄
- 段句柄
- access token 句柄
- media 信息
- PEB 地址
- ...

在收到信息后，Windows子系统进行以下的操作：

- CsrCreateProcess 复制进程和线程句柄
- 创建 Csrss 进程结构体 (CSR_PROCESS)
- 创建 Csrss 线程结构体 (CSR_THREAD)
- CsrCreateThread 将线程插入到线程列表
- 将 Csrss 进程结构体插入 Windows 子系统的进程列表

而后在第六步中，进程环境已经设置完成，线程所需要的资源都相应分配。Windows 子系统也对要运行的信息有所了解。在调用者没有额外设置 CREATE_SUSPENDED 标志位的情况下，线程已经可以对应启动了。

在第七步中，线程真正开始执行。内核调用 KiStartUserThread ， KiStartUserThread 将线程的 IRQL 级别从 DPC (Deferred Procedure Call) 修改为 APC ，而后调用内核的线程初始化函数， PspUserThreadStartUp 。

PspUserThreadStartUp 执行以下的操作：

- 设置异常处理链
- 设置 IRQL 级别为 PASSIVE_LEVEL
- ...

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
