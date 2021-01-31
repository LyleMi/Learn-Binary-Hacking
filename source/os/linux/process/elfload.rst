进程加载
========================================

加载
----------------------------------------
从编译/链接和运行的角度看，应用程序和库程序的链接有两种方式。
一种是固定的、静态的链接，在编译时将要用到的库函数的代码从代码库中抽取出来，链接进应用软件中。另一种是动态链接，在编译阶段并不完成跟库函数的链接，到程序时才把链接库的映像装入用户空间并加以定位。

其中ELF的载入在Linux内核中完成，动态链接的实现在用户空间中由 ``ld-linux.so`` 来完成，解释器的启动由内核负责。

execve
----------------------------------------
在用户层面，shell进行会调用 ``fork()`` 系统调用创建一个新进程，新进程调用 ``execve()`` 系统调用执行指定的ELF文件。

在内核中， ``execve`` 系统调用的相应入口是 ``sys_execve`` ，在执行 ``sys_execve`` 之后，内核会调用 ``do_execve`` / ``search_binary_handle`` / ``load_elf_binary`` 等函数来完成加载。 ``do_execve`` 相关代码如下：

.. code-block:: cpp

    int do_execve(struct filename *filename,
        const char __user *const __user *__argv,
        const char __user *const __user *__envp)
    {
        return do_execve_common(filename, argv, envp);
    }
     
    static int do_execve_common(struct filename *filename,
                    struct user_arg_ptr argv,
                    struct user_arg_ptr envp)
    {
        // 选择最小负载的CPU，以执行新程序
        sched_exec();

        // 填充 linux_binprm 结构体
        retval = prepare_binprm(bprm);

        // 拷贝文件名、命令行参数、环境变量
        retval = copy_strings_kernel(1, &bprm->filename, bprm);
        retval = copy_strings(bprm->envc, envp, bprm);
        retval = copy_strings(bprm->argc, argv, bprm);

        // 调用 search_binary_handler 扫描formats链表，根据不同的文本格式，选择不同的load函数
        retval = exec_binprm(bprm);
    }

其中结构体 ``linux_binprm`` 部分定义是：

.. code-block:: cpp

    struct linux_binprm {
        char buf[BINPRM_BUF_SIZE];
    #ifdef CONFIG_MMU
        struct vm_area_struct *vma;
        unsigned long vma_pages;
    #else
        ...
        unsigned interp_flags;
        unsigned interp_data;
        unsigned long loader, exec;
    };

注册机制
----------------------------------------
Linux支持不同格式的可执行程序，这些程序用 ``linux_binfmt`` 来描述，其定义在 ``include/linux/binfmts.h`` 中。

.. code-block:: cpp

    /*
     * This structure defines the functions that are used to load the binary formats that linux accepts.
     */
    struct linux_binfmt {
        struct list_head lh;
        struct module *module;
        int (*load_binary)(struct linux_binprm *);
        int (*load_shlib)(struct file *);
        int (*core_dump)(struct coredump_params *cprm);
        unsigned long min_coredump; /* minimal dump size */
    } __randomize_layout;

所有的 ``linux_binfmt`` 对象都处于一个链表中，第一个元素的地址存放在 ``formats`` 变量中, 可以通过调用 ``register_binfmt()`` 和 ``unregister_binfmt()`` 函数在链表中插入和删除元素。在系统启动期间，为每个编译进内核的可执行格式都执行 ``register_binfmt()`` 函数。

当执行程序的时候，内核打开目标映像文件，并从目标文件的头部读入若干字节，并调用 ``search_binary_handler`` 遍历所有注册的 ``linux_binfmt`` 对象，对其调用 ``load_binary`` 方法来尝试加载，直到加载成功为止。

``search_binary_handler`` 的部分代码如下：

.. code-block:: cpp

    int search_binary_handler(struct linux_binprm *bprm)
    {
        // 遍历formats链表
        list_for_each_entry(fmt, &formats, lh) {
            if (!try_module_get(fmt->module))
                continue;
            read_unlock(&binfmt_lock);
            bprm->recursion_depth++;
     
            // 应用每种格式的load_binary方法
            retval = fmt->load_binary(bprm);
            read_lock(&binfmt_lock);
            put_binfmt(fmt);
            bprm->recursion_depth--;
            // ...
        }
        return retval;
    }

Load ELF
----------------------------------------
在ELF文件格式中，处理函数是 ``load_elf_binary`` 函数，流程如下：

- 填充并且检查目标程序ELF头部
    - 是否 ``\x7fELF`` 开头
    - 映像的类型是否为 ``ET_EXEC``
- ``load_elf_phdrs`` 加载目标程序的程序头表
    - 执行程序必须至少有一个段
    - 所有段大小之和不能超过64k
- 如果需要动态链接, 则寻找和处理解释器段
    - “解释器” 段的类型为 PT_INTERP ，可通过 ``readelf -l`` 查看
    - “解释器” 段实际上是一个字符串，即解释器的文件位置
    - 通常为 ``/lib/ld-linux.so.2`` / ``/lib64/ld-linux-x86-64.so.2``
- 检查并读取解释器的程序表头
- 装入目标程序的段 segment
- 填写程序的入口地址
    - 如果需要装入解释器
        - 通过 ``load_elf_interp`` 装入映像
        - 设置用户空间的入口地址为 ``load_elf_interp()`` 的返回值
        - ``load_elf_interp()`` 的返回值为解释器映像的入口地址
    - 如果不需要装入解释器
        - 入口地址设置为目标映像本身的入口地址
- ``create_elf_tables`` 填写目标文件的参数环境变量等必要信息
    - 准备 ``argc`` ``envc`` 等变量
- ``start_thread`` 宏修改 eip / esp ，准备进入新的程序入口

``load_elf_binary`` 的部分代码如下：

.. code-block:: cpp

    static int load_elf_binary(struct linux_binprm *bprm)
    {
        ...

        /* Now we do a little grungy work by mmapping the ELF image into
           the correct location in memory. */
        for(i = 0, elf_ppnt = elf_phdata;
            i < loc->elf_ex.e_phnum; i++, elf_ppnt++) {
            int elf_prot = 0, elf_flags, elf_fixed = MAP_FIXED_NOREPLACE;
            unsigned long k, vaddr;
            unsigned long total_size = 0;

            if (elf_ppnt->p_type != PT_LOAD)
                continue;
             ...
    }

