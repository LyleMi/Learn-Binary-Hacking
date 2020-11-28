共享内存
========================================

简介
----------------------------------------
共享内存相关的函数在 ``#include <sys/shm.h>`` / ``#include <sys/ipc.h>`` 中定义。

shm_open
----------------------------------------
shm_open 的原型为 ``int shm_open(const char *name, int oflag, mode_t mode);`` 。

该函数用于创建或打开一个共享内存，成功返回一个整数的文件描述符，错误返回-1。

其中name为共享内存区的名字，oflag为标志位，mode为权限位。

shm_unlink
----------------------------------------
shm_unlink 的原型为 ``int shm_unlink(const char *name);`` 。

shmget
----------------------------------------
shmget 的原型为 ``int shmget(key_t key, size_t size, int shmflg)`` 。

key为0(IPC_PRIVATE)时，会建立新共享内存对象。key为大于0的32位整数时，根据参数shmflg来确定操作。

size为大于0的整数时，表示新建的共享内存大小，以字节为单位。size为0时，只获取共享内存。

shmflg为0时，取共享内存标识符，若不存在则函数会报错。当 ``shmflg&IPC_CREAT`` 为真时，如果内核中不存在键值与key相等的共享内存，则新建一个共享内存；如果存在这样的共享内存，返回此共享内存的标识符。

函数成功返回共享内存的标识符，出错返回-1，错误原因存储到errno中。n

shmat
----------------------------------------
shmat用于把共享内存区对象映射到调用进程的地址空间，函数原型为 ``void *shmat(int shmid, const void *addr, int flag);`` 。

shmdt
----------------------------------------
shmdt用于解除共享内存的映射，函数原型为 ``int shmdt(void *addr);`` 。

shmctl
----------------------------------------
函数原型 ``int shmctl(int shmid, int cmd, struct shmid_ds *buf);`` 。

其中shmid表示要操作的共享内存标识符。

cmd取值为：

- IPC_STAT (获取对象属性)
- IPC_SET (设置对象属性)
- IPC_RMID (删除对象)

其中buf指定IPC_STAT/IPC_SET时用以保存/设置属性。

函数返回值0表示成功，-1表示出错。

ftok
----------------------------------------
``key_t ftok(const char *pathname, int proj_id);``
成功返回key，失败返回-1。

操作命令
----------------------------------------
ipcs / ipcrm / ipcmk。
