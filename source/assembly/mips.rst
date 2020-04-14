MIPS
========================================

简介
----------------------------------------
MIPS的意思是“无内部互锁流水级的微处理器”（Microprocessor without interlocked piped stages），其机制是尽量利用软件办法避免流水线中的数据相关问题。它最早是在80年代初期由斯坦福（Stanford）大学Hennessy教授领导的研究小组研制出来的。MIPS公司的R系列就是在此基础上开发的RISC工业产品的微处理器。这些系列产品为很多计算机公司采用构成各种工作站和计算机系统。

指令特点
----------------------------------------
- 所有指令都是32位长
- 指令操作必须符合流水线
    - MIPS指令一次只能修改一个寄存器的值
- 3操作数指令
- 32个寄存器
- 没有条件标志位

数据类型
----------------------------------------
- 所有MIPS指令都是32位
- 单个字符用单引号，例如：'b'
- 字符串用双引号，例如："A string"

寄存器
----------------------------------------
- MIPS下一共有32个通用寄存器
- 在汇编中，寄存器标志由 ``$`` 符开头
- 寄存器表示可以有两种方式
    - 直接使用该寄存器对应的编号，例如：从 ``$0`` 到 ``$31``
    - 使用对应的寄存器名称
        - 例如： ``$t1`` , ``$sp`` 
- 栈的走向是从高地址到低地址

寄存器编号及其用途
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

==================      ===========     =========
寄存器编号              寄存器名          寄存器用途
==================      ===========     =========
0                       zero            永远返回零
1                       $at             汇编保留寄存器
2-3                     $v0 - $v1       存储表达式或者是函数的返回值
4-7                     $a0 - $a3       存储子程序的前4个参数，在子程序调用过程中释放
8-15                    $t0 - $t7       临时变量
16-23                   $s0 - $s7       静态变量
24-25                   $t8 - $t9       临时变量
26-27                   $k0 - $k1       中断函数返回值
28                      $gp             指向静态数据块的中间地址
29                      $sp             栈顶指针
30                      $fp             帧指针
31                      $ra             返回地址
==================      ===========     =========

读写操作
----------------------------------------
- load word
    - ``lw  register_destination, RAM_source``
- store word
    - ``sw  register_source, RAM_destination``
- store byte
    - ``sb  register_source, RAM_destination``
- load immediate
    - ``li  register_destination, value``

寻址
----------------------------------------
- 直接寻址
    - ``la  $t0, var1``
- 间接寻址
    - ``lw  $t2, ($t0)``
- 偏移
    - ``lw  $t2, 4($t0)``

算术指令集
----------------------------------------
- sub
    - ``sub $t2,$t3,$t4``
    - ``$t2 = $t3 - $t4``
- addi
    - ``addi $t2,$t3, 5``
    - ``$t2 = $t3 + 5;``
    - add immediate
- addu
    - ``addu $t1,$t6,$t7``
    - ``# $t1 = $t6 + $t7;``
    - add as unsigned integers
- subd      
    - ``subu $t1,$t6,$t7``
    - ``$t1 = $t6 + $t7;``
    - subtract as unsigned integers
- mult
    - ``mult $t3,$t4``
    - multiply 32-bit quantities in $t3 and $t4
    - store 64-bit result in special registers Lo and Hi
    - ``(Hi,Lo) = $t3 * $t4``
- div
    - ``div $t5,$t6``
    - ``Lo = $t5 / $t6``
    - ``Hi = $t5 mod $t6``
    - ``mfhi $t0``
    - move quantity in special register Hi to $t0
    - ``mflo $t1``
    - move quantity in special register Lo to $t1

控制流
----------------------------------------
- ``b   target``
    - unconditional branch to program label target
- ``beq $t0,$t1,target``
    - branch to target if  $t0 = $t1
- ``blt $t0,$t1,target``
    - branch to target if  $t0 < $t1
- ``ble $t0,$t1,target``
    - branch to target if  $t0 <= $t1
- ``bgt $t0,$t1,target``
    - branch to target if  $t0 > $t1
- ``bge $t0,$t1,target``
    - branch to target if  $t0 >= $t1
- ``bne $t0,$t1,target``
    - branch to target if  $t0 <> $t1
- ``j   target``
    - unconditional jump to program label target
- ``jr  $t3``
    - jump to address contained in $t3
- ``jal sub_label``
    - copy program counter (return address) to register $ra (return address register)
