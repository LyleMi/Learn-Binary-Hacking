heap
================================


历史
---------------------------------

历史： ptmalloc2 来自于 dlmalloc 的分支。此后，添加线程支持并于 2006 年发布。正式发布后，patmalloc2 集成到 glibc 源码中。随着源码集成，代码修改便直接在 glibc malloc 源码里进行。因此 ptmalloc2 与 glibc 之间的 malloc 实现有很多不同。

系统调用：在之前的文章可见malloc的内部调用不是 brk 就是 mmap 系统调用。

线程化：在早期的 Linux 里，dlmalloc 被用做默认的内存分配器。但之后因为 ptmalloc2 添加了线程支持，ptmalloc2 成为了 Linux 默认内存分配器。线程支持可帮助提升内存分配器以及应用程序的性能。在 dlmalloc 里，当两个线程同时调用 malloc 时，只有一个线程能进入到临界段，因为这里的空闲列表数据结构是所有可用线程共用的。因此内存分配器要在多线程应用里耗费时间，从而导致性能降低。然而在 ptmalloc2 里，当两个线程同时调用 malloc 时，会立即分配内存。因为每个线程维护一个单独的堆分段，因此空闲列表数据结构正在维护的这些堆也是独立的。这种维护独立堆以及每一个线程享有空闲列表数据结构的行为被称为 Per Thread Arena。


ref
---------------------------------

http://wps2015.org/drops/drops/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%20glibc%20malloc.html
http://www.malloc.de/en/
http://g.oswego.edu/dl/html/malloc.html
