gdb
================================

gdb
--------------------------------

r 从头开始运行
c 从断点处继续运行
stepi 单步运行
p <func main> 找函数地址
disas <func name> 反汇编
break *0x080483d0 下断点
x/20x $esp 打印esp变量


gdb-peda
--------------------------------

pattern_create 200 生成一段长度为200的字符串
pattern_offset 0x223333 在0x223333的位置根据匹配找溢出的字节数
