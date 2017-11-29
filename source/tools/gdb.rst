gdb
================================

gdb
--------------------------------

- ``break *0x080483d0`` 在内存0x080483d0处下断点
- ``break <func name>`` 在函数处下断点
- ``c`` 从断点处继续运行
- ``checksec`` 查看保护
- ``d 1`` 删除第一个断点
- ``disas <func name>`` 反汇编函数
- ``i`` info
    - ``i b`` breakpoints info
    - ``i f`` frame info
    - ``i s`` stack info
- ``n`` 单步运行
- ``p <func name>`` 找函数地址
- ``r <param>`` 从头开始运行
- ``s`` 单步运行
- ``x/20x $esp`` 打印esp变量

gdb-peda
--------------------------------

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
