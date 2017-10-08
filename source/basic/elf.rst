ELF
================================

elf有三种段
.text
.bss
.data

.text 只读，
.data .bss 可读可写

.data 段保存静态且已经初始化好的变量
.bss 段保存未初始化的变量
.text 段保存整个程序的代码信息