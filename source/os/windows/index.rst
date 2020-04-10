Windows
========================================

.. toctree::
   :maxdepth: 2
   :caption: 目录:

   history
   basic
   fs
   registry
   security

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
- `AppLocker <https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview>`_
