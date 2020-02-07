防御措施
----------------------------------------

kptr_restrict
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在Linux内核漏洞利用中常常使用 ``commit_creds`` 和 ``prepare_kernel_cred`` 来完成提权，其地址可以从/proc/kallsyms中读取。

因此Linux启用了kptr_restrict，其值和对应的功能如下：

- 0: root和普通用户都可以读取
- 1: root用户有权限读取, 普通用户没有权限
- 2: 内核将符号地址打印为全0, root和普通用户都没有权限

该值可以通过 ``sysctl kernel.kptr_restrict`` 查看和修改

SMEP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SMEP（Supervisor Mode Execution Protection）是一种减缓内核利用的cpu策略，禁止内核态到用户态内存页的代码执行，每一页都有smep标识来标明是否允许ring0的代码执行。
