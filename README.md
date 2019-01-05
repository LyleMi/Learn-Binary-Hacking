# 二进制安全学习笔记

一些常见的二进制安全相关知识，来自于本人学习过程中做的总结，尚有缺漏或未完成的部分，持续更新中，如有错误请不吝指出。在线版本可点击[此处](https://lylemi.github.io/Learn-Binary-Hacking/)查看。

在完成的过程中参考了一些blog，皆在文末给出相应链接，感谢blog作者的分享。

该仓库仅供学习和交流使用，请勿使用相关内容进行非法行为。

### 目录结构

- 基础知识
    - Heap
    - Stack
    - LLVM
    - Sandbox
- 计算机体系结构
    - 计算机系统构成
    - CPU
    - 汇编基础
    - 固件
    - 内核
- 操作系统
    - Linux
    - Windows
    - Mac OS
- 二进制文件
    - ELF
    - PE
    - DLL
- 栈相关漏洞
    - ROP
    - 栈溢出
- 堆相关漏洞
    - Use After Free
    - Off By One
    - 堆溢出
- 保护机制
    - ASLR
    - Canary
    - CFI
    - NX
- 工具
    - 列表
    - gcc
    - gdb
    - pwntools
    - ulimit

### 生成 HTML 格式的文档

```shell
$ sudo pip install sphinx
$ sudo pip install sphinx-rtd-theme
$ make html
```

### 反馈

欢迎大家提出各种意见和建议，不胜感激。

反馈邮箱 ``lylemi@126.com``
