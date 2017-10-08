off-by-one
=================================

一个字节溢出被称为off-by-one，曾经的一段时间里，off-by-one被认为是不可以利用的，但是后来研究发现在堆上哪怕只有一个字节的溢出也会导致任意代码的执行。同时堆的off-by-one利用也出现在国内外的各类CTF竞赛中，但是在网络上还不能找到一篇系统的介绍堆off-by-one利用的教程。在这篇文章中我列出了5种常见的堆上的off-by-one攻击方式，并且给出了测试DEMO，测试的环境均为x86。

背景知识
-------------------------------------
 首先，目前的linux使用的是基于ptmalloc的堆管理器。在ptmalloc中堆块被分为以下四种类型

1.fastbin

fastbin的范围处于16～64byte，使用单向链表来维护。每次从fastbin中分配堆块时，都会从尾部取出。fastbin块的inuse位永远是置于1的，并且享有最高的优先权，在分配和释放时总会最先考虑fastbin。

2.unsort bin

unsort bin在bins[]中仅占有一个位置，除了fastbin外的其他块被释放后都会进入到这里来作为一个缓冲，每当进行malloc时会把堆块从unsort bin中取出并放到对于的bins[]中。

3.small bin

small bin是指大于16byte且小于512byte的堆块，使用双向链表链接，不会有两个相邻的空的small bin块，因为一旦出现这种情况，相邻的块就会被合并成一个块。通常是在调用free函数时触发这一过程。需要注意的是在相邻空块合并时会调用unlink()宏来进行取下操作，但是调用malloc()时的取下操作却没有使用unlink宏。

4.large bin

超出large bin范围的即为large bin，large bin相比其他块而言具有一条额外的由fd_nextsize和bk_nextsize域组成的链表结构。

其中size域低三位作为标志位，我们最需要记住的就是inuse位，这个位确定了前一个块是否处于使用状态。

是的，在ptmalloc中一个块是否使用是由下一个块进行记录的。

参考链接
-------------------------------------

- `Linux下堆漏洞利用 off by one <http://blog.csdn.net/nibiru_holmes/article/details/62040763>`_
