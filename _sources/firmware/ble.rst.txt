BLE
===================================

工作流程
-----------------------------------
- 蓝牙启动
- 扫描设备
- 设备配对
- 数据传输

设备配对
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
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
-----------------------------------

Physical Layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
蓝牙使用2.4GHz频道，自适应跳频

Link Layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 控制设备的射频状态
- Standby
- Advertising
- Scanning
- Initiating
- Connection

HCI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 主机控制接口层
- Host和Controller的通信协议
- 软硬件接口

Generic Access Profile (GAP)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
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
-----------------------------------

MITM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
B修改自身为A的地址，以A的身份和C通信
B修改自身为C的地址，以C的身份和A通信

离线PIN码
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
暴力攻击PIN码

中继攻击
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
以中继的方式放大蓝牙信号

DoS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
发起大量的鉴权/文件传送等请求，使设备不能正常工作

分析工具
-----------------------------------

嗅探工具
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- bleah
- bluelog
- btCrawler
- Kismet
- BLE Sniffer
- Blue Scanner

攻击
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- BtleJuice
- Blueranger
- Bluebugging
- Peripheral hijacking

模拟器
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- hackmelock

手机端
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Bluez
- LightBlue
- nRF Connect
- Ramble
- hackmelock

安全问题
-----------------------------------

基础
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 配置管理
- 身份鉴别
- 认证授权
- 会话管理
- 输入验证
- 错误处理

业务安全
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 账户管理
- 云端密码下发
- 远程开门
- 开锁记录

网络传输
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 明文传输
- 加密漏洞
- 数据包逆向
- OTA固件拦截

蓝牙通信
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 协议分析
- 明文传输
- 信号重放
- 信号拦截
- 操作指令
