# 二进制安全学习笔记

记录了一些常见的二进制安全相关知识，所学浅薄、精力有限，因此这份笔记有一些错误或是还没完成的部分，正在逐渐补充或修正。如果存在错误欢迎各位读者以Issue或者PR的方式批评指正。

在编写笔记的过程中参考、摘抄了很多资料，都在文末留下了相应的链接，感谢文章作者的分享。

笔记的在线版本可以点击[这里](https://lylemi.github.io/Learn-Binary-Hacking/)查看。

### 目录结构

- 基础知识
    - 计算机发展简史
    - 布尔代数
    - 大小端与数据表示
- 计算机体系结构
    - CPU
    - 内核
- 嵌入式设备
- 汇编基础
- 编译原理
    - LLVM
    - JIT
- 操作系统
    - Boot
    - Linux
    - Windows
    - Mac OS
- 逆向工程
    - ELF
    - PE
    - DLL
- 漏洞利用基础
    - Shellcode编写
    - Heap
    - Stack
- 栈相关漏洞
    - 栈溢出
    - ROP
- 堆相关漏洞
    - 堆溢出
    - Use After Free
    - Off By One
- 其他漏洞
    - 格式化字符串
    - 类型混淆
    - 整形溢出
- 防御策略
    - 安全编程
    - DEP
    - ASLR
    - Canary
    - CFI
    - NX
    - Sandbox
    - 反调试
- 漏洞挖掘
    - Fuzz
    - 符号执行
- 工具
    - 列表
    - gcc
    - gdb
    - pwntools
    - ulimit
- 其他
    - 垃圾回收
    - 沙箱

### 生成 HTML 格式的文档

```shell
$ sudo pip install sphinx sphinx-rtd-theme
$ make html
```
