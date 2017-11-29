off-by-one
=================================

一个字节溢出被称为off-by-one，曾经的一段时间里，off-by-one被认为是不可以利用的，但是后来研究发现在堆上哪怕只有一个字节的溢出也会导致任意代码的执行。同时堆的off-by-one利用也出现在国内外的各类CTF竞赛中。

参考链接
-------------------------------------

- `Linux下堆漏洞利用 off by one <http://blog.csdn.net/nibiru_holmes/article/details/62040763>`_
