BLE
========================================

简介
----------------------------------------
蓝牙低功耗（Bluetooth Low Energy，BLE），是对经典蓝牙BR/EDR技术的补充，多用于小数据率、离散传输的应用。虽然BLE和经典蓝牙都是蓝牙标准，但是BLE并不兼容经典蓝牙BR/EDR。相比于经典蓝牙，它具有覆盖范围更广，安全性更高，功耗低等特点。

架构
----------------------------------------

物理层
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
蓝牙物理层（RF层），包括BR/EDR、LE以及AMP三种，主要负责在物理channel上收发蓝牙packet。对BR/EDR和LE RF来说，还会接收来自Baseband的控制命令来控制RF频率的选择和timing。而AMP PHY，则是使用802.11（WIFI）的规范。

基带
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Link Controller和Basebandresource management组成了蓝牙的基带（baseband）。Link Controller负责链路控制，主要是根据当前物理channel的参数、逻辑channel的参数、逻辑transport的参数将数据payload组装成bluetoothpacket。另外，通过LinkControl Protocol（对LE来说是LL LayerProtocol），可以实现流控、ack、重传等机制。Baseband resourcemanagement，主要用于管理RF资源。

Link Manager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Link Manager主要负责创建、修改、释放蓝牙逻辑连接（Logical Link），同时也负责维护蓝牙设备之间物理连接（Physical Link）的参数。它的功能主要是通过Link Management Protocol（LMP，for BR/EDR）和Link Layer Protocol（LL，for LE）完成。

Device Manager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Device Manager主要负责控制蓝牙设备的通用行为（蓝牙数据传输除外的行为）,包括：搜索附近的蓝牙设备，连接到其他的蓝牙设备。

HCI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
蓝牙系统分为Bluetooth Controller和Bluetooth Host两个大的模块，它们之间通过HCI（Host Controller Interface）接口以HCI协议进行通信。

L2CAP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
L2CAP位于Bluetooth Host中，包括两个子模块：Channel Manager主要负责创建、管理、释放L2CAP channel。L2CAP Resource Manager负责统一管理、调度L2CAP channel上传递的PDU（Packet Data Unit），以确保那些高QoS的packet可以获得对物理信道的控制权。

SMP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SMP（Security Manager Protocol）是一个点对点的协议，基于专用的L2CAP channel，用于生成加密（encryption）和识别（identity）用的密匙（keys）。

SDP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SDP（Service Discover Protocol）也是一个点对点的协议，基于专用的L2CAP channel，用于发现其它蓝牙设备能提供哪些profile以及这些profile有何特性。在了解清楚了其他蓝牙设备的profile以及特性之后，本蓝牙设备可以发起对自己感兴趣的蓝牙profile的连接动作。

AMP Manager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
基于L2CAP channel，和对端的AMP manager交互，用于发现对方是否具备AMP功能，以及收集用于建立AMP物理链路的信息。

GAP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
GAP（Generic Access Profile）是一个基础的蓝牙profile，用于提供蓝牙设备的通用访问功能，包括设备发现、连接、鉴权、服务发现等等。

GAP 是所有其它应用模型的基础，它定义了在 Bluetooth 设备间建立基带链路的通用方法。还定义了一些通用的操作，这些操作可供引用 GAP 的应用模型以及实施多个应用模型的设备使用。GAP 确保了两个 蓝牙设备（不管制造商和应用程序）可以通过 Bluetooth 技术交换信息，以发现彼此支持的应用程序。

工作流程
----------------------------------------
- 蓝牙启动
- 扫描设备
- 设备配对
- 数据传输

设备配对
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 生成初始密钥
    - 初始密钥Kinit长度为128bit, 由E22算法生成
    - 首先提出通信的设备为主设备(Master)，用A表示
    - 被动通信的设备为从设备(Slave)，用B表示
    - E22算法的输入
        - 从设备的物理地址(BD_ADDR)
        - PIN码及其长度
        - 128 bit的随机数(IN_RAND)，由A产生，并明文传输给B
- 生成链路密钥
    - A产生128位的随机数LK_RANDA，B产生随机数LK_RANDB
    - Kinit与LK_RANDA，发送给B
    - Kinit与LK_RANDB，发送给A
    - 用E21加密LK_RANDA、LK_RANDB、BD_ADDRA、BD_ADDRB，结果异或得到Kab
- 双方认证
    - 使用挑战-应答
    - A为应答，B为请求
    - A产生128 bit的随机数AU_RANDA，明文传输至B
    - A、B使用AU_RANDA、Kab、BD_ANDRB加密运算生成32位的SRESA和SRESB
    - B将SRESB传给A，A比较SRESA和SRESB，相同则通过

协议栈
----------------------------------------

Physical Layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
蓝牙使用2.4GHz频道，自适应跳频

Link Layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 控制设备的射频状态
- Standby
- Advertising
- Scanning
- Initiating
- Connection

HCI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 主机控制接口层
- Host和Controller的通信协议
- 软硬件接口

Generic Access Profile (GAP)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 控制设备连接和广播
- role
    - Broadcaster：设备发送Advertising Events
    - Observer: 设备在接受Advertising Events
    - Peripheral：设备接受Link Layer连接
    - Central: 设备发起Link Layer连接
- 通信模式
    - Broadcast Mode and Observation Procedure: 单向无连接通信
    - Discovery modes and procedure: 设备发现
    - Connection modes and procedure: 设备连接
    - Bonding modes and procedure: 设备配对
- Logical Link Control and Adaptation Protocol (L2CAP Protocol)
- Security Manager (SM)
    - 配对
    - 认证
    - 加密
- Attribute Protocol (ATT)
- Generic Attribute Profile (GATT)
    - 接受和处理主从设备的指令信息，并打包成合适的profile
    - Services
        - 将数据分为独立逻辑项，包含一个或多个Characteristic
        - 每个Service有一个UUID标识
    - Characteristic
        - 最小的逻辑数据单元

攻击
----------------------------------------

MITM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
B修改自身为A的地址，以A的身份和C通信
B修改自身为C的地址，以C的身份和A通信

离线PIN码
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
暴力攻击PIN码

中继攻击
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
以中继的方式放大蓝牙信号

DoS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
发起大量的鉴权/文件传送等请求，使设备不能正常工作

分析工具
----------------------------------------

嗅探工具
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- cc2540 / cc2541
    - 只能监听一个信道
    - 低价替补方案
- MRF51822
    - 数据包捕获不稳定
- Frontline BPA 600
- Ellsys BEX400
- Ubertooth
    - 支持有限
- HackRF
    - 针对蓝牙，使用复杂

扫描器
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- bleah
- bluelog
- btCrawler
- Kismet
- BLE Sniffer
- Blue Scanner

攻击
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- BtleJuice
- Blueranger
- Bluebugging
- Peripheral hijacking

模拟器
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- hackmelock

手机端
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Bluez
- LightBlue
- nRF Connect
- Ramble
- hackmelock

安全问题
----------------------------------------

基础
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 配置管理
- 身份鉴别
- 认证授权
- 会话管理
- 输入验证
- 错误处理

业务安全
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 账户管理
- 云端密码下发
- 远程开门
- 开锁记录

网络传输
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 明文传输
- 加密漏洞
- 数据包逆向
- OTA固件拦截

蓝牙通信
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 协议分析
- 明文传输
- 信号重放
- 信号拦截
- 操作指令

参考链接
----------------------------------------
- `BLE安全现状与挑战 <https://mp.weixin.qq.com/s/wrOZ13q0dmg11X5U1VISTQ>`_
