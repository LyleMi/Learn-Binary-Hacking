seccomp
========================================
seccomp (secure computing mode) 是Linux Kernel 从2.6.23版本引入的一种安全机制。

seccomp可以通过系统调用 ``ptrctl`` 或者 ``seccomp`` 开启，需要内核配置中开启了 ``CONFIG_SECCOMP`` 和 ``CONFIG_SECCOMP_FILTER`` 。

seccomp支持 ``SECCOMP_MODE_STRICT`` 和 ``SECCOMP_MODE_FILTER`` 两种模式。在 ``SECCOMP_MODE_STRICT`` 模式下，进程不能使用 ``read`` ``write`` ``_exit``和``sigreturn`` 以外的其他系统调用。

在 ``SECCOMP_MODE_FILTER`` 模式下，可以利用 BerkeleyPacket Filter 配置哪些系统调用及它们的参数可以被进程使用。
