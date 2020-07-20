# 二进制安全学习笔记

### 序言
---

这份笔记是笔者在学习二进制安全相关知识中的一些记录，用作整理思路、记录想法。笔者所学浅薄、精力有限，在整理笔记的过程中难免存在一些错误或是不完整的部分，正在逐步修正和补充。如果存在疏漏、错误，欢迎各位读者以[Issue](https://github.com/LyleMi/Learn-Binary-Hacking/issues/new)或者[PR](https://github.com/LyleMi/Learn-Binary-Hacking/pulls)的方式批评指正，感激不尽。

在编写笔记的过程中参考、摘抄了很多资料，都在文末留下了相应的链接，十分感谢文章作者的分享。其中笔记的在线版本可以点击[这里](https://lylemi.github.io/Learn-Binary-Hacking/)查看。

### 笔记大纲
---

1. 基础知识
    1.1. 计算机发展简史
    1.2. 布尔代数
    1.3. 大小端与数据表示
2. 计算机体系结构
    2.1. 计算机系统构成
    2.2. CPU
    2.3. 内存
    2.4. 硬盘
    2.5. 总线
    2.6. 设备
    2.7. 内核
3. 嵌入式设备
    3.1. 固件解包
    3.2. HAL
    3.3. 硬件
    3.4. 4G
    3.5. BLE
    3.6. 可信启动
    3.7. 路由设备
    3.8. Android
    3.9. iOS
    3.10. 安全性
4. 汇编基础
    4.1. 区别
    4.2. x86
    4.3. ARM
    4.4. MIPS
    4.5. PowerPC
5. 编译原理
    5.1. 基础知识
    5.2. 词法分析
    5.3. 语法分析
    5.4. 语义分析
    5.5. 中间代码生成
    5.6. 代码优化
    5.7. 代码生成
    5.8. LLVM
    5.9. JIT
    5.10. 相关术语
6. 操作系统
    6.1. 概述
    6.2. Boot
    6.3. Linux
    6.4. Mac OS
    6.5. Windows
    6.6. Android
    6.7. iOS
7. 虚拟化
    7.1. 基础
    7.2. 虚拟化技术
    7.3. VMWare
    7.4. KVM
    7.5. Xen
    7.6. QEMU
    7.7. Unicorn
8. 逆向工程
    8.1. ELF
    8.2. PE
    8.3. DLL
9. 漏洞利用基础
    9.1. Shellcode
    9.2. Stack
    9.3. Heap
10. 栈相关漏洞
    10.1. 调用机制
    10.2. ROP
    10.3. 栈溢出
11. 堆相关漏洞
    11.1. Use After Free
    11.2. Off By One
    11.3. 堆溢出
12. 其他漏洞
    12.1. 格式化字符串
    12.2. Type Confusion
13. 防御策略
    13.1. ASLR
    13.2. Canary
    13.3. CFI
    13.4. NX
    13.5. 沙箱机制
    13.6. 安全编程
14. 工具与资源
    14.1. 工具列表
    14.2. 书单
    14.3. 文档资料
    14.4. 逆向工具
    14.5. 调试器
    14.6. 编译工具
    14.7. ulimit
15. 其他
    15.1. 垃圾回收
    15.2. 沙箱

### 本地编译
---

```bash
git clone https://github.com/LyleMi/Learn-Binary-Hacking.git
cd Learn-Binary-Hacking
pip install sphinx sphinx-rtd-theme
make html
```

### Contribution
---

如果有任何的问题、意见或者建议欢迎以Issue或PR的形式提出，不胜感激。

感谢所有的[贡献者](https://github.com/LyleMi/Learn-Binary-Hacking/graphs/contributors)。

### License
---

项目以CC0 1.0许可证发布，可以点击[这里](https://github.com/LyleMi/Learn-Binary-Hacking/blob/master/LICENSE)浏览全文。

### 注
---

该笔记仅供学习和交流使用，请读者遵守《[中华人民共和国网络安全法](http://www.npc.gov.cn/npc/xinwen/2016-11/07/content_2001605.htm)》，勿对非授权目标进行测试。
