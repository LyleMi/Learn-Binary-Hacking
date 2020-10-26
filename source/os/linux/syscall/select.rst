select
========================================
select是IO多种复用的一种实现，它将需要监控的fd分为读，写，异常三类，其返回时是读、写、异常事件发生或者超时。

``select`` 系统调用的原型如下：

.. code-block:: cpp

    int select (int __nfds, fd_set *__restrict __readfds,
               fd_set *__restrict __writefds,
               fd_set *__restrict __exceptfds,
               struct timeval *__restrict __timeout);
