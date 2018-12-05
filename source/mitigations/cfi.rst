CFI
================================

简介
--------------------------------
攻击者常常溢出覆盖或者直接篡改寄存器EIP的值，篡改间接调用的地址，进而控制了程序的执行流程。

CFI（控制流完整性）是一种漏洞利用的缓解措施。CFI用于降低攻击的可能性。最简单的理解CFI的概念，在运行时强制要求程序按编译时希望的想法去执行。

一个程序的控制流可以用一张图来呈现，这张图被称为控制流图（CFG，control flow graph）。CFG是一个每个节点都是程序基本块的有向图，图中的每一条有向边都是可能的控制流路径。CFI则用来保证CFG在运行时和编译时相同。

CFI On Window
--------------------------------
执行流保护（CFG，Control Flow Guard）是微软从Windows 8.1 update 3及Windows 10技术预览版开始，默认启用的一项缓解技术。这项技术通过在间接跳转前插入校验代码，检查目标地址的有效性，进而可以阻止执行流跳转到预期之外的地点， 最终及时并有效的进行异常处理，避免引发相关的安全问题。

这种思想及技术在业界有了较为成熟的应用，此次Windows 10将其引入，以便提高其安全性。但是绿盟科技安全团队（NSFST）在分析CFG的实现机制过程中，发现了CFG存在全面绕过的方法，随即向微软提报，并在随后的一段时间内，配合微软修复了这个问题。

CFG原理
--------------------------------
在编译启用了CFG的模块时，编译器会分析出该模块中所有间接函数调用可达的目标地址，并将这一信息保存在Guard CF Function Table中。

同时，编译器还会在所有间接函数调用之前插入一段校验代码，以确保调用的目标地址是预期中的地址

操作系统在创建支持CFG的进程时，将CFG Bitmap映射到其地址空间中，并将其基址保存在ntdll!LdrSystemDllInitBlock+0x60中。

CFG Bitmap是记录了所有有效的间接函数调用目标地址的位图，出于效率方面的考虑，平均每1位对应8个地址（偶数位对应1个0x10对齐的地址，奇数位对应剩下的15个非0x10对齐的地址）。

提取目标地址对应位的过程如下：

- 取目标地址的高24位作为索引i；
- 将CFG-Bitmap当作32位整数的数组，用索引i取出一个32位整数bits；
- 取目标地址的第4至8位作为偏移量n；
- 如果目标地址不是0x10对齐的，则设置n的最低位；
- 取32位整数bits的第n位即为目标地址的对应位。

操作系统在加载支持CFG的模块时，根据其Guard CF Function Table来更新CFG Bitmap中该模块所对应的位。同时，将函数指针_guard_check_icall_fptr初始化为指向 ntdll!LdrpValidateUserCallTarget。

ntdll!LdrpValidateUserCallTarget从CFG Bitmap中取出目标地址所对应的位，根据该位是否设置来判断目标地址是否有效。若目标地址有效，则该函数返回进而执行间接函数调用；否则，该函数将抛出异常而终止当前进程。

Ref
--------------------------------
- `Let's talk about CFI: clang edition <https://blog.trailofbits.com/2016/10/17/lets-talk-about-cfi-clang-edition/>`_
- `分析及防护 Win10 执行流保护绕过问题 <http://blog.csdn.net/yingzheng1983/article/details/47340985>`_
- `Enforcing Forward-Edge Control-Flow Integrity in GCC & LLVM <http://www.pcc.me.uk/~peter/acad/usenix14.pdf>`_
