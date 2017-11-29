ELF
=====================================

概述
--------------------------------------
ELF(Executable and Linking Format)是一种对象文件的格式，用于定义不同类型的对象文件(Object files)中存放的对象、以及存放对象的格式。最初ELF由UNIX系统实验室（USL）开发和发布，作为应用程序二进制接口（ABI）的一部分。 

ELF标准旨在通过为开发人员提供一套简化的软件开发流程二进制接口定义，可以在多种环境下运行。 

对象文件
---------------------------------------

对象文件(Object files)有如下三个种类

可重定位的对象文件(Relocatable file)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
这是由汇编器汇编生成的 .o 文件。后面的链接器(link editor)拿一个或一些 Relocatable object files 作为输入，经链接处理后，生成一个可执行的对象文件 (Executable file) 或者一个可被共享的对象文件(Shared object file)。

可执行的对象文件(Executable file)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在Linux系统里面，存在两种可执行的东西。除了Executable file，另外一种就是可执行的脚本(如shell脚本)。注意这些脚本不是Executable file，它们只是文本文件，但是执行这些脚本所用的解释器就是 Executable file，比如 bash shell 程序。

可被共享的对象文件(Shared object file)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

这些就是所谓的动态库文件，也即.so文件。如果拿前面的静态库来生成可执行程序，那每个生成的可执行程序中都会有一份库代码的拷贝。如果在磁盘中存储这些可执行程序，那就会占用额外的磁盘空间；另外如果拿它们放到Linux系统上一起运行，也会浪费掉宝贵的物理内存。如果将静态库换成动态库，那么这些问题都不会出现。

ELF格式
---------------------------------------

ELF格式有两种

::

            Linking View
    +--------------------------+
    |       ELF header         |
    +--------------------------+
    |   Program header table   |
    |        (optional)        |
    +--------------------------+
    |        Section 1         |
    +--------------------------+
    |           ...            |
    +--------------------------+
    |        Section n         |
    +--------------------------+
    |           ...            |
    +--------------------------+
    |   Section header table   |
    +--------------------------+

           Execution View
    +--------------------------+
    |        ELF header        |
    +--------------------------+
    |   Program header table   |
    +--------------------------+
    |       Segment 1          |
    +--------------------------+
    |           ...            |
    +--------------------------+
    |       Segment n          |
    +--------------------------+
    |           ...            |
    +--------------------------+
    |   Section header table   |
    |        (optional)        |
    +--------------------------+


段
---------------------------------------

.bss
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds uninitialized data that contribute to the program's memory image. By definition, the system initializes the data with zeros when the program begins to run. 

.comment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds version control information.

.data and .data1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
These sections hold initialized data that contribute to the program's memory image.

.debug
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds information for symbolic debugging. The contents are unspecified.

.dynamic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds dynamic linking information. The section's attributes will include the SHF_ALLOC bit. Whether the SHF_WRITE bit is set is processor specific.

.dynstr
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds strings needed for dynamic linking, most commonly the strings that represent the names associated with symbol table entries.

.dynsym
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds the dynamic linking symbol table, as ''Symbol Table'' describes.

.fini
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds executable instructions that contribute to the process termination code. That is, when a program exits normally, the system arranges to execute the code in this
section.

.got
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds the global offset table. See ''Special Sections'' in Part 1 and ''Global Offset Table'' in Part 2 for more information.

.hash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds a symbol hash table. See ''Hash Table'' in Part 2 for more information.

.init
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds executable instructions that contribute to the process initialization code.
That is, when a program starts to run, the system arranges to execute the code in this section before calling the main program entry point (called main for C programs).

.interp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds the path name of a program interpreter. If the file has a loadable segment that includes the section, the section's attributes will include the SHF_ALLOC bit; otherwise, that bit will be off.

.line
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds line number information for symbolic debugging, which describes the correspondence between the source program and the machine code. The contents are unspecified.

.note
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds information in the format.

.plt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds the procedure linkage table.

.relname and .relaname
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
These sections hold relocation information, as ''Relocation'' below describes. If the file has a loadable segment that includes relocation, the sections' attributes will include the
SHF_ALLOC bit; otherwise, that bit will be off. Conventionally, name is supplied by the section to which the relocations apply. Thus a relocation section for .text normally would have the name .rel.text or .rela.text.

.rodata and .rodata1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
These sections hold read-only data that typically contribute to a non-writable segment in the process image.

.shstrtab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds section names.

.strtab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds strings, most commonly the strings that represent the names associated with symbol table entries. If the file has a loadable segment that includes the symbol string table, the section's attributes will include the SHF_ALLOC bit; otherwise, that bit will be off.

.symtab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds a symbol table, as ''Symbol Table'' in this section describes. If the file has a loadable segment that includes the symbol table, the section's attributes will include the SHF_ALLOC bit; otherwise, that bit will be off.

.text
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This section holds the ''text,'' or executable instructions, of a program.

内存布局
--------------------------------------

::

    ----------------------------------
    |        Kernel Space            |    1GB
    ----------------------------------
    |                                |    Random Stack offset
    ----------------------------------
    |       Stack (growth down)      |    RLIMIT_STACK
    ----------------------------------
    |                                |    Random mmap offset
    ----------------------------------
    |       Memory Map Segement      |
    |  (including dynamic libraries) |
    |      e.g. /lib/libc.so         |  
    |         (growth down)          |    
    ----------------------------------
    |                                |  
    ----------------------------------
    |        Heap (growth up)        |   
    ----------------------------------
    |                                |  Random brk offset
    ----------------------------------
    |          BSS Segement          |   
    | Uninitalized static variables, | 
    |       filled with zeros.       | 
    |  e.g. static char *userName;   |   
    ----------------------------------
    |          Data Segement         |
    |  Static variables initialized  |
    |    by the programmer.          |
    ----------------------------------
    |          Text Segement         |
    ----------------------------------


