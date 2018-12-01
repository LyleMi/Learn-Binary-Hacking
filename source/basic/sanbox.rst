沙箱机制
========================================

命名空间
---------------------------------
Linux中支持namespace（命名空间）机制，这是一种轻量级的虚拟化形式。命名空间建立系统的不同视图，对于每一个命名空间看来，都像是一台独立的Linux机器。目前Linux支持六种命名空间，分别为：

- mnt (mount points, file systems)
- pid (processes)
- net (network stack)
- ipc (system V IPC)
- uts (host name)
- user (UIDs)

chroot
---------------------------------
chroot是较老的沙箱实现方式，其内部的机制和文件系统的命名空间类似。

参考链接
---------------------------------
- `Many approaches to sandboxing in Linux <https://opensourceforu.com/2016/07/many-approaches-sandboxing-linux/amp/>`_
