文件系统
========================================

系统支持
----------------------------------------
Windows支持以下几种文件系统格式：

- CDFS
    - 适用于CD的只读文件系统
- UDF
    - 适用于DVD的只读文件系统
- FAT12, FAT16, FAT32
    - File Allocation Table，文件分配表文件系统
- NTFS
    - New Technology File System，Windows NT以及之后 Windows的标准文件系统

FAT
----------------------------------------
FAT(File Allocation Table)是“文件分配表”的意思。顾名思义，就是用来记录文件所在位置的表格，

FAT-16
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
FAT16使用了16位的空间来表示每个扇区(Sector)配置文件的状态，故称之为FAT16。FAT-16磁盘分区最大只能到2GB。

FAT-32
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
FAT16使用了32位的空间来表示每个扇区(Sector)配置文件的状态，最大支持32GB分区，单个文件最大支持4GB。

exFAT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
exFAT（Extended File Allocation Table File System）又称扩展FAT或扩展文件分配表。

NTFS
----------------------------------------
NTFS（New Technology File System）是Windows NT内核的系列操作系统支持的、一个特别为网络和磁盘配额、文件加密等管理安全特性设计的磁盘格式，提供长文件名、数据保护和恢复，能通过目录和文件许可实现安全性，并支持跨越分区。 

NTFS支持功能

- 访问控制（Access control）
- 磁盘配额（Disk quotas）
- 加密文件系统（Encrypting File System, EFS）
- 支持多重数据流（Multiple data streams）
- 支持硬链接和联结点（Hard links and junction points）
- 基于Unicode的命名方式

ReFS
----------------------------------------
ReFS（Resilient File System，弹性文件系统） 是在Windows Server 2012中新引入的一个文件系统。只能应用于存储数据，不能引导系统。ReFS是与NTFS大部分兼容的，其主要目的 是为了保持较高的稳定性，可以自动验证数据是否损坏，并尽力恢复数据。
