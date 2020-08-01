沙箱
========================================

虚拟环境检测
---------------------------------------

检测进程名
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Vmware
    - Vmtoolsd
    - Vmwaretrat
    - Vmwareuser
    - Vmacthlp
- VirtualBox
    - vboxservice
    - vboxtray

检测注册表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- HKLM\SOFTWARE\Vmware Inc\Vmware Tools
- HKEY_CLASSES_ROOT\Applications\VMwareHostOpen.exe
- HKEY_LOCAL_MACHINE\SOFTWARE\Oracle\VirtualBox Guest Additions

硬盘文件检测
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- VMware
    - C:\windows\System32\Drivers\Vmmouse.sys
    - C:\windows\System32\Drivers\vmtray.dll
    - C:\windows\System32\Drivers\VMToolsHook.dll
    - C:\windows\System32\Drivers\vmmousever.dll
    - C:\windows\System32\Drivers\vmhgfs.dll
    - C:\windows\System32\Drivers\vmGuestLib.dll
- VirtualBox
    - C:\windows\System32\Drivers\VBoxMouse.sys
    - C:\windows\System32\Drivers\VBoxGuest.sys
    - C:\windows\System32\Drivers\VBoxSF.sys
    - C:\windows\System32\Drivers\VBoxVideo.sys
    - C:\windows\System32\vboxdisp.dll
    - C:\windows\System32\vboxhook.dll
    - C:\windows\System32\vboxoglerrorspu.dll
    - C:\windows\System32\vboxoglpassthroughspu.dll
    - C:\windows\System32\vboxservice.exe
    - C:\windows\System32\vboxtray.exe
    - C:\windows\System32\VBoxControl.exe

运行服务检测
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- VMTools
- Vmrawdsk
- Vmusbmouse
- Vmvss
- Vmscsi
- Vmxnet
- vmx_svga
- Vmware Tools

mac地址前缀
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 00:05:69 (Vmware)
- 00:0C:29 (Vmware)
- 00:1C:14 (Vmware)
- 00:50:56 (Vmware)
- 08:00:27 (VirtualBox)

硬件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- CPU核心数
- CPU温度
- CPUID指令
- MAC地址
- 内存大小
- 磁盘大小
- 注册表和文件路径
- 主板序列号 / 主机型号

文件系统
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- TEMP目录下的文件数量
- 最近打开的文件

虚拟指令
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 特定指令

沙箱环境检测
---------------------------------------
- 延迟运行
    - 自动化沙箱运行时间较短，可延迟一段时间后再运行
- 检测开机时间
    - 许多沙箱检测完毕后会重置系统，开机时间很短
- 检测配置
    - 沙箱环境的配置大多不高
- 检测进程 / 服务
- 代码运行时间

参考链接
---------------------------------------
- `反虚拟机和沙箱检测的一些小技巧  <https://www.freebuf.com/articles/system/202717.html>`_
- `CheckVM Sandbox <https://github.com/sharepub/CheckVM-Sandbox>`_
