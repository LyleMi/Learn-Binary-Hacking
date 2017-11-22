Heap
===========================================


历史
-------------------------------------------

ptmalloc2来自于dlmalloc的分支。此后，添加线程支持并于2006年发布。正式发布后，patmalloc2集成到glibc源码中。随着源码集成，代码修改便直接在glibc malloc源码里进行。因此ptmalloc2与glibc之间的malloc实现有很多不同。

在早期的Linux里，dlmalloc被用做默认的内存分配器。但之后因为ptmalloc2添加了线程支持，ptmalloc2成为了Linux默认内存分配器。线程支持可帮助提升内存分配器以及应用程序的性能。

在dlmalloc里，当两个线程同时调用malloc时，只有一个线程能进入到临界段，因为这里的空闲列表数据结构是所有可用线程共用的。因此内存分配器要在多线程应用里耗费时间，从而导致性能降低。

然而在ptmalloc2里，当两个线程同时调用malloc时，会立即分配内存。因为每个线程维护一个单独的堆分段，因此空闲列表数据结构正在维护的这些堆也是独立的。这种维护独立堆以及每一个线程享有空闲列表数据结构的行为被称为Per Thread Arena。

Chunk
-------------------------------------------

在内存中，堆（低地址到高地址，属性RW）有两种分配方式（与malloc申请chunk做区分）:

- mmap: 当申请的size大于128kb时，由mmap分配
- brk: 当申请的size小于128kb时，由brk分配，第一次分配132KB（main arena），第二次在brk下分配，不够则执行系统调用，向系统申请

在内存中进行堆的管理时，系统基本是以 chunk 作为基本单位，chunk的结构在源码中有定义

::

    struct malloc_chunk {

      INTERNAL_SIZE_T      mchunk_prev_size;  /* Size of previous chunk (if free).  */
      INTERNAL_SIZE_T      mchunk_size;       /* Size in bytes, including overhead. */

      struct malloc_chunk* fd;         /* double links -- used only if free. */
      struct malloc_chunk* bk;

      /* Only used for large blocks: pointer to next larger size.  */
      struct malloc_chunk* fd_nextsize; /* double links -- used only if free. */
      struct malloc_chunk* bk_nextsize;
    };

chunk的状态可以分为三种：allocated chunk、free chunk、top chunk


Bin
---------------------------------

bin在内存中用来管理free chunk，bin为带有头结点（链表头部不是chunk）的链表数组，根据特点，将bin分为四种，分别为：fastbin、unsortedbin、smallbin、largebin

- fastbin
    - 根据chunk大小维护多个单向链表
    - sizeof(chunk) < 64(bytes)
    - 下一chunk（内存中）的free标志位不取消，显示其仍在使用
    - 后进先出（类似栈），先free的先被malloc
    - 拥有维护固定大小chunk的10个链表
- unsortedbin
    - 双向循环链表
    - 不排序
    - 暂时存储free后的chunk，一段时间后会将chunk放入对应的bin中
    - 只有一个链表
- smallbin
    - 双向循环链表
    - sizeof(chunk) < 512 (bytes)
    - 先进先出（类似队列）
    - 16,24...64,72...508 bytes(62个链表)
- largebin
    - 双向循环链表
    - sizeof(chunk) >= 512 (bytes)
    - free chunk中多两个指针分别指向前后的large chunk
    - 63个链表
        - 0-31(512+64*i)
        - 32-48(2496+512*i)
        - ...
    - 链表中chunk大小不固定，先大后小

Malloc逻辑
---------------------------------

当接收到申请内存的请求时，malloc的处理逻辑如下

- 申请长度位于 fastbin 时
    - 根据大小获得fastbin的index
    - 根据index获取fastbin中链表的头指针
        - 如果头指针为 NULL，转去smallbin
    - 将头指针的下一个chunk地址作为链表头指针
    - 分配的chunk保持inuse状态，避免被合并
    - 返回除去chunk_header的地址
- 长度位于 smallbin 时
    - 根据大小获得smallbin的index
    - 根据index获取smallbin中双向循环链表的头指针
    - 将链表最后一个chunk赋值给victim
        - 若victim为表头，此时链表为空，不从smallbin中分配
        - 若victim为0，此时链表未初始化，将fastbin中的chunk合并
        - 其他情况取出victim,设置inuse
    - 检查victim是否为main_arena,设置标志位
    - 返回除去chunk_header的地址
- 长度位于 largebin 时:
    - 根据大小获得largebin的index
    - 将fastbin中chunk合并，加入到unsortbin中

进一步处理

- unsortedbin
    - 反向遍历unsortedbin,检查 2*size_t<chunk_size<内存总分配量
    - unsortedbin的特殊分配
        - 如果前一步smallbin分配未完成
        - 并且 unsortedbin中只有一个chunk
        - 并且该chunk为 last remainder chunk
        - 并且该chunk大小 >（所需大小+最小分配大小）
        - 则切分一块分配
    - 如果请求大小正好等于当前遍历chunk的大小，则直接分配
    - 继续遍历，将合适大小的chunk加入到smallbin中，向前插入作为链表的第一个chunk。(smallbin中每个链表中chunk大小相同)
    - 将合适大小的chunk加入到largebin中，插入到合适的位置（largebin中每个链表chunk由大到小排列）
- largebin
    - 反向遍历largebin，由下到上查找，找到合适大小后切分
        切分后大小<最小分配大小，返回整个chunk，会略大于申请大小
        切分后大小>最小分配大小，加入 unsortedbin。
    - 未找到，index+1，继续寻找

如果这之后还未找到合适的chunk，就会使用top chunk进行分配，还是没有的话，如果在多线程环境中，fastbin可能会有新的chunk，再次执行合并，并向unsortedbin中重复上面的步骤，之后还是没有的话，就只能向系统申请。

以上为malloc分配的经过

malloc检查
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- 从fastbin中取出chunk后，检查size是否属于fastbin
- 从smallbin中除去chunk后，检查victim->bk->fd == victim
- 从unsortbin取chunk时，要检查2*size_t<chunk_size<内存总分配量
- 从largebin取chunk时，切分后的chunk要加入unsortedbin,需要检查 unsortedbin的第一个chunk的bk是否指向unsortedbin

free机制
---------------------------------

- 使用 chunksize(p) 宏获取p的size
- 安全检查
    - chunk的指针地址不能溢出
    - chunk 的大小 >= MINSIZE(最小分配大小)，并且检查地址是否对齐
- 大小为fastbin时
    - 检查下一个chunk的size：2*size_t<chunk_size<内存总分配量
    - double free检查：检查当前free的chunk是否与fastbin中的第一个chunk相同，相同则报错

其他情况
---------------------------------
- 检查下一个chunk的size
    - 2*size_t<chunk_size<内存总分配量
    - 如果当前 chunk 为 sbrk()分配，那么它相邻的下一块 chunk 超过了分配区的地址，会报错
- double free检查
    - 检查当前free的chunk是否为top chunk，是则报错
    - 根据下一块的inuse标识检查当前free的chunk是否已被free
- unlink合并
    - 检查前后chunk是否free，然后向后（top chunk方向）合并，并改变对应的inuse标志位
    - unlink检查
        - I.当前chunk的size是否等于下一chunk的prev_size
        - II.P->bk->fd == P && P->bk->fd == P
    - 如果合并后 chunk_size > 64bytes,则调用函数合并fastbin中的chunk到unsortedbin中
    - 将合并后的chunk加入unsortedbin
- unsortedbin检查
    - 检查 unsortedbin的第一个chunk的bk是否指向unsortedbin


References
---------------------------------

- `深入理解 <http://wps2015.org/drops/drops/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%20glibc%20malloc.html>`_
- `malloc homepage <http://www.malloc.de/en/>`_
- `Memory Allocator <http://g.oswego.edu/dl/html/malloc.html>`_
- `Dance in Heap 1 <http://www.freebuf.com/articles/system/151372.html>`_
