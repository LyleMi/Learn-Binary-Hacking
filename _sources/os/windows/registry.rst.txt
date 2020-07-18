注册表
========================================

简介
----------------------------------------
注册表（Registry）是包含操作系统和其它软件的所有设置和配置相关数据的目录。

注册表的逻辑结构类似于磁盘上的文件系统。注册表包含了键（key）和值（value）。其中键类似文件系统中的目录，而值就像文件，键可以包含子键（subkey）和值。值中存储着配置数据，数据有多种类型。最顶层的键称为根键（root key）。

涉及到注册表的有：

1. 在启动时，boot loader从注册表中读取数据。
2. 内核启动时，内核驱动从注册表中读取配置。
3. 在登陆时，读取网络驱动设置、墙纸、菜单、启动项等。
4. 在应用启动时，读取各种配置文件。

数据类型
----------------------------------------
- REG_NONE
    - 没有值
- REG_SZ
    - 固定长度 Unicode
- REG_EXPAND_SZ
    - 可变长度 Unicode
- REG_BINARY
    - 任意长度二进制数据
- REG_DWORD
    - 32 bit 数字
- REG_DWORD_BIG_ENDIAN
    - 32 bit 数字，大端
- REG_LINK
    - Unicode 符号链接，指向另一个注册表项
- REG_MULTI_SZ
    - Unicode 字符串
- REG_RESOURCE_LIST
    - 硬件资源描述
- REG_FULL_RESOURCE_DESCRIPTOR
    - 硬件资源描述
- REG_RESOURCE_REQUIREMENTS_LIST
    - 资源需求
- REG_QWORD
    - 64 bit 数字

root key
----------------------------------------
- HKEY_CURRENT_USER：和当前登陆用户有关的数据
    - AppEvents：事件关联
    - Console：控制台设定
    - Control Panel：控制面板
    - Environment：环境变量
    - EUDC：用户定义字符
    - Identities：Windows Mail账户信息
    - Keyboard Layout：键盘布局
    - Network：网络驱动信息
    - Printers：打印机
    - Software：软件
    - Volatile Environment：可变的环境变量
- HKEY_USERS：所有用户有关的数据
- HKEY_CLASSES_ROOT
    - 文件后缀关联
    - COM（Component Object Model）注册
    - UAC
- HKEY_LOCAL_MACHINE：系统相关的数据
    - HKLM\BCD00000000
    - HKLM\COMPONENTS
    - HKLM\HARDWARE
    - HKLM\SAM
    - HKLM\SECURITY
    - HKLM\SOFTWARE
    - HKLM\SYSTEM
- HKEY_PERFORMANCE_DATA：性能相关的数据
- HKEY_CURRENT_CONFIG：当前配置文件
