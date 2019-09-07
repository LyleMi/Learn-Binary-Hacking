Windows
========================================

版本历史
----------------------------------------

+----------------------------+---------+--------------+
| Windows Version            | Version | Release Date |
+============================+=========+==============+
| Windows NT 3 .1            | 3.1     | 1993.7       |
+----------------------------+---------+--------------+
| Windows NT 3 .5            | 3.5     | 1994.9       |
+----------------------------+---------+--------------+
| Windows NT 3 .51           | 3.51    | 1995.5       |
+----------------------------+---------+--------------+
| Windows NT 4 .0            | 4.0     | 1996.7       |
+----------------------------+---------+--------------+
| Windows 2000               | 5.0     | 1999.12      |
+----------------------------+---------+--------------+
| Windows XP                 | 5.1     | 2001.8       |
+----------------------------+---------+--------------+
| Windows Server 2003        | 5.2     | 2003.3       |
+----------------------------+---------+--------------+
| Windows Vista              | 6.0     | 2007.1       |
+----------------------------+---------+--------------+
| Windows Server 2008        | 6.0     | 2008.3       |
+----------------------------+---------+--------------+
| Windows 7                  | 6.1     | 2009.10      |
+----------------------------+---------+--------------+
| Windows Server 2008 R2     | 6.1     | 2009.10      |
+----------------------------+---------+--------------+
| Windows 8                  | 6.2     | 2012.10      |
+----------------------------+---------+--------------+
| Windows 10                 | 10.0    | 2015.7       |
+----------------------------+---------+--------------+
| Windows Server 2016        | 10.0    | 2016.9       |
+----------------------------+---------+--------------+
| Windows Server 2019        | 10.0    | 2019.10      |
+----------------------------+---------+--------------+

Windows NT
----------------------------------------
Windows NT是Microsoft发行的一系列操作系统，其第一个版本于1993年7月发布。它是一个独立于处理器的多处理和多用户操作系统。

Windows NT的第一个版本是Windows NT 3.1，是为工作站和服务器计算机生成的。它旨在补充基于MS-DOS（包括Windows 1.0到Windows 3.1x）的Windows的消费者版本。

渐渐地，Windows NT系列已经扩展到微软针对所有个人计算机的通用操作系统产品线，不再使用Windows 9x系列。

NT是第一个纯粹的32位Windows版本，而面向消费者的对应产品Windows 3.1x和Windows 9x则是16位/32位的混合版本。它是一种多架构操作系统。最初，它支持多种指令集架构，包括IA-32，MIPS和DEC Alpha;稍后添加了对PowerPC，Itanium，x64和ARM的支持。

最新版本支持x86（更具体地说是IA-32和x64）和ARM。 Windows NT系列的主要功能包括Windows Shell，Windows API，Native API，Active Directory，组策略，硬件抽象层，NTFS，BitLocker，Windows应用商店，Windows Update和Hyper-V。

运行模式
----------------------------------------
Windows支持Intel x86/x64 的两种处理器模式内核模式（Ring 0）和用户模式（Ring 3）。

内核模式下的代码可以访问所有的内存空间；可以直接操纵硬件。用户模式下的代码无权访问系统空间的内存页面；无法直接操纵硬件。用户模式向内核模式的切换是受控制的。

基本模块
----------------------------------------
Windows操作系统内核模式下的基本模块包含Windows执行体、Windows内核体、设备驱动程序、窗口和图形系统、硬件抽象层。

其中Windows执行体是Windows内核体的上层接口，包含了基本的操作系统服务，如进程与线程管理、内存管理、I/O管理、网络连接、进程间通信以及安全服务。

Windows内核体实现与硬件体系结构支持的代码，实现底层的操作系统功能，如线程调度 、中断和异常分发处理、多处理器同步等。

设备驱动程序包括硬件设备驱动程序、文件系统与网络设备驱动程序。

窗口和图形系统即Win32k.sys（Windows子系统的内核模式部分），实现了图形用户界 面（GUI）函数，包括窗口的处理、绘制等。

硬件抽象层即Hal.dll文件，用于屏蔽Windows内核、驱动程序与平台硬件差异性的底 层代码。

系统进程
----------------------------------------
- Idle
    - 每个CPU一个idle线程做空闲CPU时间统计
- System
    - 内核模式的系统进程
- Wininit.ext
    - Session 0 初始化
- Csrss.exe
    - Windows子系统进程，即客户端/服务器运行进程
- Smss.exe
    - 会话（Session）管理器，系统启动时第一个运行的进程
- Winlogon.exe
    - 处理交互式登录
- Services.exe
    - 服务控制管理器，负责启动和、停止、暂停、恢复服务
- Svchost.exe
    - 共享进程服务的宿主进程
- Lsass.exe
    - 本地安全授权子系统，验证用户登录、授权和审计

注册表
----------------------------------------

简介
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
注册表（Registry）是包含操作系统和其它软件的所有设置和配置相关数据的目录。

注册表的逻辑结构类似于磁盘上的文件系统。注册表包含了键（key）和值（value）。其中键类似文件系统中的目录，而值就像文件，键可以包含子键（subkey）和值。值中存储着配置数据，数据有多种类型。最顶层的键称为根键（root key）。

涉及到注册表的有：

1. 在启动时，boot loader从注册表中读取数据。
2. 内核启动时，内核驱动从注册表中读取配置。
3. 在登陆时，读取网络驱动设置、墙纸、菜单、启动项等。
4. 在应用启动时，读取各种配置文件。

数据类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- REG_NONE
    - 没有值
- REG_SZ
    - 固定长度 Unicode
- REG_EXPAND_SZ
    - 可变长度 Unicode
- REG_BINARY
    - 任意长度二进制数据
- REG_DWORD
    - 32 bit 数字
- REG_DWORD_BIG_ENDIAN
    - 32 bit 数字，大端
- REG_LINK
    - Unicode 符号链接，指向另一个注册表项
- REG_MULTI_SZ
    - Unicode 字符串
- REG_RESOURCE_LIST
    - 硬件资源描述
- REG_FULL_RESOURCE_DESCRIPTOR
    - 硬件资源描述
- REG_RESOURCE_REQUIREMENTS_LIST
    - 资源需求
- REG_QWORD
    - 64 bit 数字

root key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- HKEY_CURRENT_USER：和当前登陆用户有关的数据
    - AppEvents：事件关联
    - Console：控制台设定
    - Control Panel：控制面板
    - Environment：环境变量
    - EUDC：用户定义字符
    - Identities：Windows Mail账户信息
    - Keyboard Layout：键盘布局
    - Network：网络驱动信息
    - Printers：打印机
    - Software：软件
    - Volatile Environment：可变的环境变量
- HKEY_USERS：所有用户有关的数据
- HKEY_CLASSES_ROOT
    - 文件后缀关联
    - COM（Component Object Model）注册
    - UAC
- HKEY_LOCAL_MACHINE：系统相关的数据
    - HKLM\BCD00000000
    - HKLM\COMPONENTS
    - HKLM\HARDWARE
    - HKLM\SAM
    - HKLM\SECURITY
    - HKLM\SOFTWARE
    - HKLM\SYSTEM
- HKEY_PERFORMANCE_DATA：性能相关的数据
- HKEY_CURRENT_CONFIG：当前配置文件

服务
----------------------------------------
服务（Services）程序是后台运行的进程，常用来执行特定的任务，不需要和用户进行交互。比如自动更新服务、后台智能传输服务、事件日志服务等。

服务程序受Service Control Manager（SCM，即services.exe进程）所控制，其服务程序的配置数据位于 ``HKLM\System\CurrentControlSet\Services`` 。

服务通常由三个部分组成：服务应用、服务控制程序 (service control program, SCP)、服务控制管理器 (service control manager, SCM)。

Windows提供了内置的SCP，可以启动、停止、继续运行程序。用户也可以自定义SCP程序来玩橙更细粒度的控制。服务程序是普通的Windows可执行程序，只是会有一些附加模块和SCM通信。

WMI
----------------------------------------
Windows Management Instrumentation (WMI) 是 Web-Based Enterprise Management (WBEM)的一个实现，用于提供操作界面和对象模式以便访问有关操作系统、设备、应用程序和服务的管理信息。

WMI由4个部分组成，分别为：management applications, WMI infrastructure, providers, and managed objects。


进程与线程
----------------------------------------

进程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
进程（Process）是一个应用程序运行的实例，包含以下一些基本组件：私有虚拟地址、可执行体程序、被操作系统分配的一份资源句柄（Handles）列表、访问控制令牌（Token）、进程标识号一个或多个线程。

访问控制令牌是用以唯一的标识所有者及其所属组以及和该进程相关联的特权（Privilege）信息。

进程创建步骤分为七步：

- 转换并校验参数和标记
- 打开可执行映像文件（.exe）
- 创建Windows执行体进程对象
- 创建初始线程（栈，上下文和 执行体线程对象等）
- 通知Windows子系统初始化了一个进程
- 开始运行初始线程。（除非进程创建的时候被挂起）
- 在新进程和线程的上下文空间中，完成地址空间的初始化（ 比如加载必需的DLL文件）并开始执行程序

线程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
线程（Thread）是CPU调度执行的基本单元，其包含CPU状态、两个栈、线程本地存储（TLS）、线程标识号和访问控制令牌。

线程的两个栈分别用于内核模式和用户模式。线程本地存储（TLS）包含一个私有存储空间，用来保存子系统、运行时库以及DLL文件等。

访问控制令牌（Access Token），用以唯一的标识所有者及其所属组以及和该线程相关联的特权（Privilege）信息

安全机制
----------------------------------------

核心模块
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- SRM: Security reference monitor

内存破坏漏洞防护机制
----------------------------------------

DEP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
DEP（Data Executive Protection，数据执行保护）从Windows XP SP2开始引入，缺省仅为基本的Windows程序和服务启用DEP。

DEP的基本原理是将数据所在的内存页标记为不可执行，当程序产生溢出，恶意代码试图在数据段执行指令时，CPU会产生异常而不去执行指令。

实现DEP机制需要CPU的支持。为此AMD公司推出了EVP（enhanced virus protection）技术，Intel推出了EDB （execute disable bit）技术，这些技术在原理上均是在内存的页面表（Page Table）中加入一个特殊的标 识位（NX/XD）来标识是否允许在该页上执行指令。

DEP有四种可选参数：

– Optin：对于大多数用户版本的操作系统来说，默认仅将DEP保护是仅仅为一些基本的Windows程序和服务启用。该模式可被应用程序动态关闭
– Optout：系统为所有在所选列表外的程序和服务启用DEP，这种模式下，DEP仍可被应用程序关闭。该模式多用于服务器版本的操作系统，如Windows Server 2003/2008
– AlwaysOn：对所有的进程启用DEP的保护，不存在排除列表。该模式下，DEP不可以被关闭，这是一种仅仅在64位操作系统上才能实现的工作模式，这在最大限度上保证了所有程序都能够抵御常见的数据溢出攻击
– AlwaysOff：对所有的进程都禁用了DEP，DEP也不能被应用程序动态开启，这该模式一般只有在特殊场合才会使用

ASLR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ASLR（ Address Space Layout Randomization， 内存地址空间布局随机化）在加载程序到内存空间时随机化各个模块的起始加载地址，防止攻击者定位攻击指令代码的位置。

ASLR需要操作系统及应用程序的双重支持才能发挥作用，支持ASLR的程序在PE头中会设置IMAGE_DLL_CHARACTERISTICS_DYNAMIC_BASE标识表明其支持ASLR。

ASLR主要影响的部分模块随机化、堆栈随机化和PEB/TEB随机化。模块随机化指系统将PE文件映射到内存时，对其加载基地址进行随机化处理， 基地址在系统启动时确定，系统重启后会变化。堆栈随机化指每次程序加载后，其内存空间中堆、栈的基址都会发生变化。于是内存中的变量所在的地址也会发生变化。

ASLR在Windows Vista/7引入实现，但机制尚不完善，攻击者还能在一定范围内进行漏洞利用。比如使用堆喷射、利用没有随机化的系统或软件的EXE/DLL等方式。

EPM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
EPM （Enhanced Protection Mode，增强保护模式）也叫做“沙盒模式”（Sandbox Mode），本质上是隔离进程和降低权限。该机制Windows 8的IE 10开始引入，从Windows 8.1开始默认启用。

PatchGuard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
PatchGuard是在64位版本的Windows操作系统中提供的新功能，用于保护操作系统的核心结构，防止他们被其他程序修改。

其对系统服务描述符表SSDT（System Service Descriptor Table）、全局描述符表GDT（Global Descriptor Table）和中断描述符表IDT（Interrupt Descriptor Table）、系统映像System images（ntoskrnl.exe, ndis.sys, hal.dll）等进行保护。

PatchGuard处在系统任务的一个较高层面上，通过每隔一定时间进行一些固定的检查来确定这些系统关键内容是否更改。这些检查主要通过将核心内容与缓存中已保存的已知正确的备份进行对比，检测间隔大约为5-10分钟左右的某一随机选择时间。

PatchGuard的缺点在于缺乏本地硬件水平的支持，只能通过轮询的形式，而不是采用事件驱动或硬件驱动的形式。

Code Signing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Code Signing（代码签名）检查机制需要加载到系统内核中运行的驱动程序必须有数字签名以保证 其代码的完整性，否则系统内核就不加载该驱动程序。

代码完整性检测被加载到内核中的驱动程序或系统文件是否已经被签名，或正在运行系统管理员账户权限的系统文件是否已被恶意软件篡改。在基于x64版本的操作系统下，内核模式的驱动程序必须进行数字签名后才能被加载。

其他
----------------------------------------

SSDT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SSDT（System Services Descriptor Table）是系统服务描述符表，这个表把 Ring3 的 Win32 API 和 Ring0 的内核 API 联系起来。

SSDT 并不仅仅只包含一个庞大的地址索引表，它还包含着一些其它有用的信息，诸如地址索引的基地址、服务函数个数等。

通过修改此表的函数地址可以对常用Windows函数及API进行Hook，从而实现对一些关心的系统动作进行过滤、监控的目的。

一些HIPS、防毒软件、系统监控、注册表监控软件往往会采用此接口来实现自己的监控模块。

IDT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
IDT（Interrupt Descriptor Table）是中断描述符表，是操作系统用于处理中断的。

GDT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
GDT，即全局描述表（GDT Global Descriptor Table）。

LDT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
LDT（Local Descriptor Table），即局部描述符表。

常见缩写
----------------------------------------
- Alpc: Advanced Local Inter-Process Communication
- Cc: Common Cache
- Cm: Configuration manager
- Dbgk: Debugging Framework for User-Mode
- Em: Errata Manager
- Etw: Event Tracing for Windows
- Ex: Executive support routines
- FsRtl: File system driver run-time library
- Hvl: Hypervisor Library Io I/O manager
- Kd: Kernel Debugger
- Ke: Kernel
- Lsa: Local Security Authority
- Mm: Memory manager
- Nt: NT system services (most of which are exported as Windows functions) Ob Object manager
- Pf: Prefetcher
- Po: Power manager
- Pp: PnP manager
- Ps: Process support
- Rtl: Run-time library
- Se: Security
- Sm: Store Manager
- Tm: Transaction Manager
- Vf: Verifier
- Wdi: Windows Diagnostic Infrastructure Whea Windows Hardware Error Architecture Wmi Windows Management Instrumentation
- Zw: Mirror entry point for system services 

参考链接
----------------------------------------
- `windows kernel exploit tutorial <https://www.redog.me/tags/windows-kernel-exploit-tutorial/>`_
- `HEVD <https://github.com/hacksysteam/HackSysExtremeVulnerableDriver>`_
- `HolicPOC <https://github.com/leeqwind/HolicPOC>`_
- `Windows NT Wiki <https://en.wikipedia.org/wiki/Windows_NT>`_
