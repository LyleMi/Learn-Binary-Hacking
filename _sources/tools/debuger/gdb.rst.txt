gdb
========================================

gdb
----------------------------------------

- BreakPoint 断点
    - ``break [PROBE_MODIFIER] [LOCATION] [thread THREADNUM] [if CONDITION]``
    - 设置断点
        - ``break *0x080483d0`` 在内存0x080483d0处下断点
        - ``break <func name>`` 在函数处下断点
        - ``break *<func name> + 4`` 在函数偏移处下断点
    - 相关操作
        - ``c`` 从断点处继续运行
        - ``d[elete] 1`` 删除断点1
        - ``d 1 2`` 删除断点1 2
        - ``d`` 删除所有断点
        - ``dis[able] 1`` Disable breakpoint 1
        - ``en[able] 1`` Enable breakpoint 1
- ``c[ontinue]`` 运行至下一个断点或程序结束
- ``command`` 设置断点的命令
- ``disas <func name>`` 反汇编函数
- ``fin[ish]`` 跳出当前函数 / 循环
- ``i`` info
    - ``i b`` breakpoints info
    - ``i f`` frame info
    - ``i s`` stack info
- ``ignore <break_list> count``
    - break_list所指定的断点号将被忽略count次
- ``n`` / ``next`` 单步运行
- ``p <func name>`` 找函数地址
- ``r <param>`` 从头开始运行
- ``s`` / ``step`` 单步运行 遇到调用时深入
- ``file`` 加载文件
- ``x/nfu address`` 打印内存
    - ``n`` 表示要显示的内存单元的个数
    - ``f`` 表示显示方式
        - ``x`` 按十六进制格式显示变量。
        - ``d`` 按十进制格式显示变量。
        - ``u`` 按十进制格式显示无符号整型。
        - ``o`` 按八进制格式显示变量。
        - ``t`` 按二进制格式显示变量。
        - ``a`` 按十六进制格式显示变量。
        - ``i`` 指令地址格式
        - ``c`` 按字符格式显示变量。
        - ``f`` 按浮点数格式显示变量。
    - ``u`` 表示一个地址单元的长度
        - ``b`` 表示单字节
        - ``h`` 表示双字节
        - ``w`` 表示四字节
        - ``g`` 表示八字节
    - 地址可以是内存地址或者是寄存器


gdb-peda
----------------------------------------

- ``aslr`` show aslr setting
- ``checksec`` Check for various security options of binary
- ``dumpargs`` Display arguments passed to a function when stopped at a call instruction
- ``dumprop`` Dump all ROP gadgets in specific memory range
- ``elfheader`` Get headers information from debugged ELF file
- ``elfsymbol`` Get non-debugging symbol information from an ELF file
- ``lookup`` Search for all addresses/references to addresses which belong to a memory range
- ``patch`` Patch memory start at an address with string/hexstring/int
- ``pattern`` Generate, search, or write a cyclic pattern to memory
    - ``pattern_create 200`` 生成一段长度为200的字符串
    - ``pattern_offset 0x223333`` 在0x223333的位置根据匹配找溢出的字节数
- ``procinfo`` Display various info from /proc/pid/
- ``pshow`` Show various PEDA options and other settings
- ``pset`` Set various PEDA options and other settings
- ``readelf`` Get headers information from an ELF file
- ``ropgadget`` Get common ROP gadgets of binary or library
- ``ropsearch`` Search for ROP gadgets in memory
- ``searchmem|find`` Search for a pattern in memory; support regex search
- ``shellcode`` Generate or download common shellcodes.
- ``skeleton`` Generate python exploit code template
- ``vmmap`` Get virtual mapping address ranges of section(s) in debugged process
- ``xormem`` XOR a memory region with a key
