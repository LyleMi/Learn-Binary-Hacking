Logon
========================================
Windows的登陆操作是由 ``%SystemRoot%\System32\Winlogon.exe`` 处理的。用户的识别和验证功能是由一系列被称为 ``credential providers`` 的DLL提供的。

标准的Windows credential providers实现了默认的认证方式：密码与智能卡。在Windowws 10中引入了新的生物认证方式：Windows Hello，基于面容识别进行验证。用户也可以通过实现对应接口的方式来完成自己的验证机制。

由于 Winlogon.exe 系列进程比较敏感，这些进程都被设置为 LogonUI.exe 的子进程。当用户登录过程结束后，LogonUI相关进程都会结束。 Winlogon.exe 也可以调用网络相关DLL来进行需要网络交互的认证。

当用户名密码输入结束后，会被发送到 Local
Security Authentication Service process (Lsass.exe) 。而后在DLL中，完成具体的实现，例如对应的用户名和密码是否被存储在SAM中。如果是域登录的话，则对应发起网络请求。
