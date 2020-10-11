启动流程
----------------------------------------

KVM交互
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 获取到 KVM 句柄
- 创建虚拟机，获取到虚拟机句柄
- 为虚拟机映射内存，还有设备/信号处理的初始化
- 创建 vCPU，并为 vCPU 分配内存空间
- 创建 vCPU 个数的线程并运行虚拟机
- 线程进入循环，并捕获虚拟机退出原因，做相应的处理

TCG 模式
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 启动
    - main
    - cpu_init
    - qemu_init_vcpu
    - qemu_tcg_init_vcpu
    - qemu_tcg_cpu_thread_fn
- 主函数
    - cpu_exec 处理中断异常，找到代码翻译块，执行
        - tb_find 在Hash表中查找，如果找不到则调用tb_gen_code创建一个TB
            - tb_gen_code 分配一个新的TB
                - gen_intermediate_code
                - tcg_gen_code 将TCG代码转换成主机代码。
        - cpu_loop_exec_tb
            - cpu_tb_exec  执行TB主机代码
            - tcg_qemu_tb_exec

用户态
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
用户态的大部分代码都在 linux-user 目录下

- linux-user/main.c main
    - linux-user/linuxload.c loader_exec
        - linux-user/elfload.c load_elf_binary
            - load_elf_image
            - load_elf_interp
    - linux-user/<arch>/cpu_loop.c cpu_loop
    - accel/tcg/cpu-exec.c cpu_exec
