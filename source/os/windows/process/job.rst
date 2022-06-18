Job
========================================

基础
----------------------------------------
Windows的Job是用于控制一个或多个进程的内核对象。进程可以属于任意数量的Job，在大部分情况下只属于某一个Job。

Job在Windows中扮演了重要的角色，作用包括：

- 管理Windows现代APP（UWP）
- 实现Windows的容器管理
- 实现 DAM (Desktop Activity Moderator) 的相关功能
- 实现 DFSS 的定义和管理 (Dynamic Fair-Share Scheduling)
- 用于辅助实现 Run As / Application Box / Program Compatibility Assistant 
- 提供应用沙箱的部分功能

Job 设置
----------------------------------------
Job 支持以下的配置：

- 活动进程的最大值
- 以Job为单位的用户模式的CPU时间限制
- 以进程为单位的用户模式的CPU时间限制
- Job 的CPU亲和性 (affinity)
- 进程 / Job 的最大虚拟内存使用量
- CPU 时间控制
- 网络带宽控制
- 磁盘 I/O 控制
- ...
