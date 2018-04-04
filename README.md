# 二进制安全学习笔记

一些常见的二进制安全相关知识，来自于本人学习过程中做的总结，尚有缺漏或未完成的部分，持续更新中，如有错误请不吝指出。在完成的过程中参考了一些blog，皆在文末给出相应链接，感谢blog作者的分享。

该仓库仅供学习和交流使用，请勿使用相关内容进行非法行为。

### 目录结构

- 基础知识
    - dll
    - Linux
    - heap
    - 指令
    - ELF
    - PE(portable executable)
    - 寄存器
    - LLVM
- Common Weakness Enumeration: CWE
    - Type Confusion
    - User After Free
    - 栈溢出
    - 堆溢出
- 攻击方式
    - rop
    - off-by-one
    - 格式化字符串
- 保护机制
    - ASLR
    - 执行流保护（CFG）
    - Non-Executable Stack
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