Cgroups
========================================

四个组件
----------------------------------------
Cgroups包含Cgroup、Subsystem、hierarchy、task四个组件。

Cgroup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Cgroup是对进程分组管理的一种机制，一个Cgroup包含一组进程，并可以在上面添加添加Linux Subsystem的各种参数配置，将一组进程和一组Subsystem的系统参数关联起来。

Subsystem
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Subsystem是一个资源调度控制器不同版本的Kernel所支持的有所偏差，可以通过 ``cat /proc/cgroups`` 查看

- blkio 
    - 对块设备(比如硬盘)的IO进行访问限制
- cpu
    - 设置进程的CPU调度的策略，比如CPU时间片的分配
- cpuacct
    - 统计/生成cgroup中的任务占用CPU资源报告
- cpuset
    - 在多核机器上分配给任务(task)独立的CPU和内存节点(内存仅使用于NUMA架构)
- devices
    - 控制cgroup中对设备的访问
- freezer
    - 挂起(suspend) / 恢复 (resume) cgroup 中的进程
- memory
    - 用于控制cgroup中进程的占用以及生成内存占用报告
- net_cls
    - 使用等级识别符（classid）标记网络数据包，这让 Linux 流量控制器 tc (traffic controller) 可以识别来自特定 cgroup 的包并做限流或监控
- net_prio
    - 设置cgroup中进程产生的网络流量的优先级
- hugetlb
    - 限制使用的内存页数量
- pids
    - 限制任务的数量
- net_cls
    - 可以使不同cgroups下面的进程使用不同的namespace。
    - 每个subsystem会关联到定义的cgroup上，并对这个cgoup中的进程做相应的限制和控制。

hierarchy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
hierarchy树形结构的 CGroup 层级，每个子 CGroup 节点会继承父 CGroup 节点的子系统配置，每个 Hierarchy 在初始化时会有默认的 CGroup(Root CGroup)。

Task
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Task (任务) 在cgroups中，任务就是系统的一个进程。

四个组件的关系
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 系统在创建新的hierarchy之后，该系统的所有任务都会加入这个hierarchy的cgroup，称之为root cgroup，此cgroup在创建hierarchy自动创建，后面在该hierarchy中创建都是cgroup根节点的子节点
- 一个subsystem只能附加到一个hierarchy上面
- 一个hierarchy可以附加多个subsystem
- 一个task可以是多个cgroup的成员，但这些cgroup必须在不同的hierarchy
- 一个进程fork出子进程时，该子进程默认自动成为父进程所在的cgroup的成员，也可以根据情况将其移动到到不同的cgroup中
