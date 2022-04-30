虚拟化安全
========================================

在Windows 10中，微软在 Hyper-V 中加入了一系列新的虚拟化安全机制。

- Device Guard
    - 提供 Hypervisor Code Integrity (HVCI) 机制
- Hyper Guard
    - 保护关键的 内核 与 Hypervisor 相关代码与数据结构
- Credential Guard
    - 防止敏感凭证被非授权访问泄露
- Application Guard
    - 为Edge提供沙箱
- Host Guardian and Shielded Fabric
    - 为虚拟机提供一个虚拟的 TPM (v-TPM)
