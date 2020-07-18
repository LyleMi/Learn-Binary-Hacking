身份认证
========================================

SID
----------------------------------------
- Windows使用SID来唯一表示安全主体 ，包括用户和组。Windows NT 6.x中， 系统服务也有SID标识。
- 查看账户的SID
    - whoami / user
    - PsGetSid
    - user2sid

- 从Windows Vista/Server 2008（NT 6.0）开始，每个服务程序都 有自己的SID，而账户的名称则为“NT SERVICE\<服务名称>”
- 查看服务SID及其账户名称
    - sc.exe showsid <service name>
    - psgetsid <sid>

相关程序
----------------------------------------

Winlogon
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 可信任的进程，负责管理与安全有关的用户交互
    - 协调登录过程，在登录时启动用户的第一个进程，处理注销过程，以管理其他各种与安全有关的操作，包括在登录时输入口令、更改口令、锁住/解锁工作站等。
    - 创建可用的桌面。
    - 向操作系统注册一个安全维护序列（SAS, Secure Attention  Sequence），默认为Ctrl+Alt+Delete。
    - 维护工作站状态。
    - 实现超时处理。
- 向GINA发送事件通知消息，提供可供GINA调用的各种接口函数。
- 保证其操作对其他进程不可见，从而防止登录密码等信息被截获。

GINA动态链接库
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 提供了Winlogon用户标识和验证用户的输出函数。
    - WlxActivateUserShell、WlxDisplaySASNotice、WlxInitialize、WlxLoggedOnSAS、WlxLoggedOutSAS、WlxLogoff、WlxNegotiate、WlxScreenSaverNotify、WlxShutdown、WlxStartApplication、WlxWkstaLockedSAS
- 微软提供的GINA是MSGINA.dll，但允许被用户替换来自行定 制系统的用户识别和身份验证。
    - [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Winlogon] "GinaDLL"="ginadll.dll"

身份验证程序包
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 身份验证程序包的任务
    - 验证用户；
    - 为用户新建LSA登录会话；
    - 返回绑定到用户安全令牌中的SID。
- 身份验证程序包位于DLL动态链接库中
    - 在系统启动期间被LSA所链接，接受输入的登录证书，通过验证 程序决定是否允许用户登录。
- Windows安装的身份验证程序包
    - 独立（工作组）环境：MSV1_0， %SystemRoot%\System32\Msv1_0.dll
    - 域环境：Kerberos， %SystemRoot%\System32\Kerberos.dll

LogonUI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
LogonUI，Logon user interface，登录用户接口

- 为了保护Winlogon进程不崩溃，LogonUI由Winlogon按需启动，真正加载Credential provider，为用户提供验证身份的图形化界面。
- LogonUI是用户模式下的进程，即 %SystemRoot%\System32\LogonUI.exe

CP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
CP，Credential provider，凭证提供者

- Credential provider是运行在LogonUI进程内的COM对象， 位于一些DLL文件中（触发SAS热键后由Winlogon按需启动 ），用来获取用户的用户名、密码、智能卡PIN码或者生物数据（比如指纹、视网膜信息）
- 标准的CP是 %SystemRoot%\System32\authui.dll 和 %SystemRoot%\System32\SmartcardCredentialProvider.dll.
