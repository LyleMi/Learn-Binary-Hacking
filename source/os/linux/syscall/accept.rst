accept
========================================
accept函数原型为 ``int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);`` 其中sockfd为套接字的文件描述符，addr指向存放地址信息的结构体的首地址，addrlen存放地址信息的结构体的大小。

accept的执行流程为：

- 创建一个socket结构体
- 获取一个未使用的文件描述符fd
- 创建一个file结构体，并和socket关联
- 从全连接队列中获取客户端发来的请求
- 根据请求获取之前新建的sock结构体返回
- 将请求中的sock结构体和开始分配的socket结构体关联
- 将文件描述符fd和文件结构体file关联，并返回fd供用户使用

内核实现如下：

.. code-block:: cpp

    SYSCALL_DEFINE3(accept, int, fd, struct sockaddr __user *, upeer_sockaddr,
            int __user *, upeer_addrlen)
    {
        return sys_accept4(fd, upeer_sockaddr, upeer_addrlen, 0);
    }

最终调用的是 SYSCALL_DEFINE4：

.. code-block:: cpp

    SYSCALL_DEFINE4(accept4, int, fd, struct sockaddr __user *, upeer_sockaddr,
            int __user *, upeer_addrlen, int, flags)
    {
        struct socket *sock, *newsock;
        struct file *newfile;
        int err, len, newfd, fput_needed;
        struct sockaddr_storage address;

        if (flags & ~(SOCK_CLOEXEC | SOCK_NONBLOCK))
            return -EINVAL;

        if (SOCK_NONBLOCK != O_NONBLOCK && (flags & SOCK_NONBLOCK))
            flags = (flags & ~SOCK_NONBLOCK) | O_NONBLOCK;

        // 根据fd获取对应的socket结构
        sock = sockfd_lookup_light(fd, &err, &fput_needed);
        if (!sock)
            goto out;

        err = -ENFILE;
        // 分配一个新的socket结构体
        newsock = sock_alloc();
        if (!newsock)
            goto out_put;

        newsock->type = sock->type;
        newsock->ops = sock->ops;

        __module_get(newsock->ops->owner);

        // 获取一个未使用的文件描述符，这个描述符也就是accept()返回的fd
        newfd = get_unused_fd_flags(flags);
        if (unlikely(newfd < 0)) {
            err = newfd;
            sock_release(newsock);
            goto out_put;
        }
        // 创建一个file结构体，同时将这个file结构体和刚刚创建的socket关联
        // file->private_data 指向socket
        newfile = sock_alloc_file(newsock, flags, sock->sk->sk_prot_creator->name);
        if (IS_ERR(newfile)) {
            err = PTR_ERR(newfile);
            put_unused_fd(newfd);
            sock_release(newsock);
            goto out_put;
        }

        err = security_socket_accept(sock, newsock);
        if (err)
            goto out_fd;

        // 调用inet_accept()执行主处理操作
        err = sock->ops->accept(sock, newsock, sock->file->f_flags);
        if (err < 0)
            goto out_fd;
        // 如果要获取对端连接信息，那么拷贝对应信息到用户空间
        if (upeer_sockaddr) {
            // 调用inet_getname()获取对端信息
            if (newsock->ops->getname(newsock, (struct sockaddr *)&address,
                          &len, 2) < 0) {
                err = -ECONNABORTED;
                goto out_fd;
            }
            err = move_addr_to_user(&address,
                        len, upeer_sockaddr, upeer_addrlen);
            if (err < 0)
                goto out_fd;
        }

        /* File flags are not inherited via accept() unlike another OSes. */
        // 将文件描述符fd和文件结构体file关联到一起
        fd_install(newfd, newfile);
        err = newfd;

    out_put:
        fput_light(sock->file, fput_needed);
    out:
        return err;
    out_fd:
        fput(newfile);
        put_unused_fd(newfd);
        goto out_put;
    }

inet_accept函数调用传输层的accept操作，并且返回新的连接控制块，新的连接控制块需要与新的socket进行关联，accept完成，将新socket的状态设置为已连接状态；

.. code-block:: cpp

    /*
     *    Accept a pending connection. The TCP layer now gives BSD semantics.
     */

    int inet_accept(struct socket *sock, struct socket *newsock, int flags,
            bool kern)
    {
        struct sock *sk1 = sock->sk;
        int err = -EINVAL;

        /* 执行传输层的accept操作 */
        struct sock *sk2 = sk1->sk_prot->accept(sk1, flags, &err, kern);

        if (!sk2)
            goto do_err;

        lock_sock(sk2);

        /* rps处理 */
        sock_rps_record_flow(sk2);
        WARN_ON(!((1 << sk2->sk_state) &
              (TCPF_ESTABLISHED | TCPF_SYN_RECV |
              TCPF_CLOSE_WAIT | TCPF_CLOSE)));

        /* 控制块连接到新的socket */
        sock_graft(sk2, newsock);

        /* 设置新socket的状态为连接 */
        newsock->state = SS_CONNECTED;
        err = 0;
        release_sock(sk2);
    do_err:
        return err;
    }
    EXPORT_SYMBOL(inet_accept);
