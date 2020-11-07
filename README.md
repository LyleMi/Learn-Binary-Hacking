# 二进制安全学习笔记

### 序言
---

这份笔记是笔者在学习二进制安全相关知识中的一些记录，用作整理思路、记录想法。笔者所学浅薄、精力有限，在整理笔记的过程中难免存在一些错误或是不完整的部分，正在逐步修正和补充。如果存在疏漏、错误，欢迎各位读者以[Issue](https://github.com/LyleMi/Learn-Binary-Hacking/issues/new)或者[PR](https://github.com/LyleMi/Learn-Binary-Hacking/pulls)的方式批评指正，感激不尽。

在编写笔记的过程中参考、摘抄了很多资料，都在文末留下了相应的链接，十分感谢文章作者的分享。其中笔记的在线版本可以点击[这里](https://binhack.readthedocs.io/zh/latest/)查看。

### 笔记大纲
---

1. 基础知识
    1. 计算机发展简史
    2. 布尔代数
    3. 大小端与数据表示
2. 计算机体系结构
    1. 计算机系统构成
    2. CPU
    3. BIOS
    4. 内存
    5. 硬盘
    6. 总线
    7. 设备
    8. I/O
    9. 内核
3. 嵌入式设备
    1. 固件解包
    2. HAL
    3. 硬件
    4. 4G
    5. BLE
    6. 可信启动
    7. 路由设备
    8. Android
    9. iOS
    10. 安全性
4. 汇编基础
    1. 区别
    2. x86
    3. ARM
    4. MIPS
    5. PowerPC
5. 编译原理
    1. 基础知识
    2. 词法分析
    3. 语法分析
    4. 语义分析
    5. 中间代码生成
    6. 代码优化
    7. 代码生成
    8. LLVM
    9. JIT
    10. 相关术语
6. 操作系统
    1. 概述
    2. Boot
    3. Linux
    4. Mac OS
    5. Windows
    6. Android
    7. iOS
7. 虚拟化
    1. 基础
    2. 虚拟化技术
    3. Virtio
    4. VMWare
    5. KVM
    6. Xen
    7. QEMU
    8. Unicorn
    9. Intel虚拟化技术
    10. 其他
8. 逆向工程
    1. ELF
    2. PE
    3. DLL
    4. Mach-O
9. 漏洞利用基础
    1. Shellcode
    2. Stack
    3. Heap
10. 栈相关漏洞
    1. 调用机制
    2. ROP
    3. 栈溢出
11. 堆相关漏洞
    1. Use After Free
    2. Off By One
    3. 堆溢出
12. 其他漏洞
    1. 格式化字符串
    2. Type Confusion
13. 防御策略
    1. ASLR
    2. Canary
    3. CFI
    4. NX
    5. 沙箱机制
    6. 安全编程
14. 工具与资源
    1. 工具列表
    2. 书单
    3. 文档资料
    4. 逆向工具
    5. 调试器
    6. 编译工具
    7. 系统工具
15. 其他
    1. 垃圾回收
    2. 沙箱
    3. 常见术语

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
