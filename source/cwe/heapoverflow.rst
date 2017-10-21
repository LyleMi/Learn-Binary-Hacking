堆溢出
=========================================

第一部分
-----------------------------------------

首先简要介绍一下堆chunk的结构
在malloc.c中找到关于堆chunk结构的代码

::

    struct malloc_chunk {

      INTERNAL_SIZE_T prev_size; 
      /* Size of previous chunk (if free).  */
      
      INTERNAL_SIZE_T size;
      /* Size in bytes, including overhead. */

      struct malloc_chunk* fd;
      /* double links -- used only if free. */
      struct malloc_chunk* bk;

      /* Only used for large blocks: pointer to next larger size.  */
      struct malloc_chunk* fd_nextsize; 
      /* double links -- used only if free. */
      struct malloc_chunk* bk_nextsize;
    };


这指明了一个heap chunk是如下的结构

::

    +-----------+---------+------+------+-------------+
    |           |         |      |      |             |
    |           |         |      |      |             |
    | prev_size |size&Flag|  fd  |  bk  |             |
    |           |         |      |      |             |
    |           |         |      |      |             |
    +-----------+---------+------+------+-------------+


如果本chunk前面的chunk是空闲的，那么第一部分prev_size会记录前面一个chunk的大小，第二部分是本chunk的size,因为它的大小需要8字节对齐，所以size的低三位一定会空闲出来，这时候这三个位置就用作三个Flag(最低位:指示前一个chunk是否正在使用;倒数第二位:指示这个chunk是否是通过mmap方式产生的;倒数第三位:这个chunk是否属于一个线程的arena)。之后的FD和BK部分在此chunk是空闲状态时会发挥作用。FD指向下一个空闲的chunk，BK指向前一个空闲的chunk，由此串联成为一个空闲chunk的双向链表。如果不是空闲的。那么从fd开始，就是用户数据了。(详细信息请参考glibc的malloc.c部分，在此不再多做解释。)

引用一位外国博主的漏洞示例程序解释

::

  /* 
   Heap overflow vulnerable program. 
   */
  #include <stdlib.h>
  #include <string.h>

  int main( int argc, char * argv[] )
  {
      char * first, * second;

      /*[1]*/
      first = malloc( 666 );
      /*[2]*/ 
      second = malloc( 12 );
      /*[3]*/
      if(argc!=1)
          strcpy( first, argv[1] );
      /*[4]*/ 
      free( first );
      /*[5]*/
      free( second );
      /*[6]*/
      return( 0 );
  }


这个程序在[3]处有很明显的堆溢出漏洞，argv[1]中的内容若过长则会越界覆盖到second部分。

程序堆结构如下

::

  +---------------------+   <--first chunk ptr
  |     prev_size       |
  +---------------------+
  |     size=0x201      |          
  +---------------------+   <--first                  
  |                     |
  |     allocated       |         
  |      chunk          |      
  +---------------------+   <--second chunk ptr                
  |    prev_size        |         
  +---------------------+                     
  |    size=0x11        |         
  +---------------------+   <--second                  
  |     Allocated       |         
  |       chunk         |     
  +---------------------+   <-- top                  
  |     prev_size       |            
  +---------------------+                     
  |    size=0x205d1     |           
  +---------------------+                      
  |                     |
  |                     |
  |                     |
  |        TOP          |   
  |                     |
  |       CHUNK         |    
  |                     |
  +---------------------+


此时利用方法为通过溢出构造，使得second chunk

::
  
  prev_size=任意值
  size=-4(因为最低位的flag没有设置，所以prev_size可以任意)
  fd=free@got-12
  bk=shellcode地址

在我们的payload将指定位置的数值改好后。下面介绍在[4][5]行代码执行时发生的详细情况。

第四行执行free(first)发生如下操作

1).检查是否可以向后合并

首先需要检查previous chunk是否是空闲的（通过当前chunk size部分中的flag最低位去判断），当然在这个例子中，前一个chunk是正在使用的，不满足向后合并的条件。

2).检查是否可以向前合并

在这里需要检查next chunk是否是空闲的(通过下下个chunk的flag的最低位去判断)，在找下下个chunk(这里的下、包括下下都是相对于chunk first而言的)的过程中，首先当前chunk+当前size可以引导到下个chunk，然后从下个chunk的开头加上下个chunk的size就可以引导到下下个chunk。但是我们已经把下个chunk的size覆盖为了-4，那么它会认为下个chunk从prev_size开始就是下下个chunk了，既然已经找到了下下个chunk，那就就要去看看size的最低位以确定下个chunk是否在使用，当然这个size是-4，所以它指示下个chunk是空闲的。

在这个时候，就要发生向前合并了。即first chunk会和 first chunk的下个chunk(即second chunk)发生合并。在此时会触发unlink(second)宏，想将second从它所在的bin list中解引用。

但是执行unlink宏之后，再调用free其实就是调用shellcode，这时就可以执行任意命令了。

第二部分
-------------------------------------

::

    /* Take a chunk off a bin list */
    #define unlink(AV, P, BK, FD) {                                            
        FD = P->fd;                                                                      
        BK = P->bk;                                                                      
        if (__builtin_expect (FD->bk != P || BK->fd != P, 0))                      
          malloc_printerr (check_action, "corrupted double-linked list", P, AV);  
        else {                                                                      
            FD->bk = BK;                                                              
            BK->fd = FD;                                                              
            if (!in_smallbin_range (P->size)                                      
                && __builtin_expect (P->fd_nextsize != NULL, 0)) {                      
                if (__builtin_expect (P->fd_nextsize->bk_nextsize != P, 0)              
                    || __builtin_expect (P->bk_nextsize->fd_nextsize != P, 0))    
                  malloc_printerr (check_action,                                      
                                   "corrupted double-linked list (not small)",    
                                   P, AV);                                              
                if (FD->fd_nextsize == NULL) {                                      
                    if (P->fd_nextsize == P)                                      
                      FD->fd_nextsize = FD->bk_nextsize = FD;                      
                    else {                                                              
                        FD->fd_nextsize = P->fd_nextsize;                              
                        FD->bk_nextsize = P->bk_nextsize;                              
                        P->fd_nextsize->bk_nextsize = FD;                              
                        P->bk_nextsize->fd_nextsize = FD;                              
                      }                                                              
                  } else {                                                              
                    P->fd_nextsize->bk_nextsize = P->bk_nextsize;                      
                    P->bk_nextsize->fd_nextsize = P->fd_nextsize;                      
                  }                                                                      
              }                                                                      
          }                                                                              
    }


参考链接
-------------------------------------

- `堆溢出的unlink利用方法 <http://wooyun.jozxing.cc/static/drops/tips-7326.html>`_
- `Double Free浅析 <http://wooyun.jozxing.cc/static/drops/binary-7958.html>`_