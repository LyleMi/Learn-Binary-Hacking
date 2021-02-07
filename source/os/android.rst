Android
========================================

设备识别码
----------------------------------------

IMEI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
国际移动设备识别码（IMEI ：International Mobile EquipmentIdentity），即通常所说的手机序列号、手机“串号”，用于在移动电话网络中识别每一部独立的手机等移动通信设备，相当于移动电话的身份证，IMEI号共有15~17位数字。国际移动设备识别码一般贴于机身背面与外包装上，同时也存在于手机存储器中。

MEID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
移动设备识别码(MEID：Mobile Equipment Identifier)是支持CDMA手机等移动通信设备唯一的识别码。

Android ID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Android_ID是运行安卓系统的手机等设备随机生成一串64位的号码，每一个Android_ID都是独一无二。Android8.0及更高版本中，Android_ID的值通过签名密钥等确定范围。在低于Android8.0的版本中，Android_ID在用户第一次启动设备时随机生成。在恢复出厂设置、或重装系统后，Android_ID会被重置。获取Android_ID不需要开启任何系统权限，可直接通过运行相应代码获取。

设备序列号
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
设备序列号（SN：SerialNumber）通常用于验证产品的出厂生产的正规性和合法性，也称作机器码、认证码、注册码，通常可在系统信息中的序列号一栏看到，不可更改。App通常可以使用命令、调用READ_PHONE_STATE权限相关函数读取SN值，但是因操作系统、手机型号版本等不同存在差异。

IMSI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
国际移动用户识别码（IMSI：International Mobile SubscriberIdentification Number）是区别移动用户的标志，储存在SIM卡中，在全网和全球范围内唯一。

ICCID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
手机SIM卡序列号（ICCID：ICC identity）是集成电路卡标识，是唯一标识SIM卡的序列号。ICCID存储在SIM卡中，完整的ICCID为19或20个字符。

参考链接
----------------------------------------
- `手机设备识别码类型分析 <https://mp.weixin.qq.com/s/Ly8XIfKanX3bgeZLe0QyeA>`_
- `Android Security <https://github.com/anantshri/Android_Security>`_ suplimentary material for Android Training's done by Anant Shrivastava
