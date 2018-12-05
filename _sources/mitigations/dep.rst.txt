NX
=================================================

简介
-------------------------------------------------
即使在今天，栈溢出也是一个很常见的安全问题。而其最常见的攻击方式则是覆盖返回地址，并跳转至shellcode。

DEP通常会去掉堆栈的可执行权限（Non-Executable Stack）并设置代码段为不可写来使得这种payload无法生效。

在DEP提出之后，也出现了相当多的绕过方式，几乎所有的绕过方式都基于一个事实：即使堆栈被标记为不可执行，但是其他地方仍然可以执行代码，而且，基于覆写返回地址的攻击几乎一直有效。

最开始的绕过方法是return-into-libc，而后则扩展到ret2plt, ret2strpy, ret2gets, ret2syscall, ret2data, ret2text, ret2code, ret2dl-resolve等方法。

- ret2data: 很常见的一种方式，把shellcode放到data段中，然后跳至data段执行
- ret2libc: 栈溢出攻击对栈有完全的控制权，即可以完全覆写返回地址和参数
- ret2strcpy：这个方式也是基于ret2libc，但是几乎可以执行任意代码。在这里，返回到strcpy，src指向栈上的shellcode，dst指向选择好的可写可执行的内存地址
- ret2gets：类似strcpy
  
