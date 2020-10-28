bind
========================================
bind函数原型为 ``int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);`` 其中sockfd为套接字的文件描述符，addr指向存放地址信息的结构体的首地址，addrlen存放地址信息的结构体的大小。

内核实现
----------------------------------------
bind的内核实现如下：

.. code-block:: c

    SYSCALL_DEFINE3(bind, int, fd, struct sockaddr __user *, umyaddr, int, addrlen)
    {
        struct socket *sock;
        struct sockaddr_storage address;
        int err, fput_needed;
        // 根据fd获取相应的socket结构体
        sock = sockfd_lookup_light(fd, &err, &fput_needed);
        if (sock) {
            err = move_addr_to_kernel(umyaddr, addrlen, &address);
            if (err >= 0) {
                err = security_socket_bind(sock, (struct sockaddr *)&address,
                               addrlen);//安全相关，暂不关注
                if (!err)
                    err = sock->ops->bind(sock, (struct sockaddr *)
                                  &address, addrlen);
            }
            fput_light(sock->file, fput_needed);
        }
        return err;
    }

其中sock->ops->bind指向的是inet_bind：

.. code-block:: c

    int inet_bind(struct socket *sock, struct sockaddr *uaddr, int addr_len)
    {
        struct sockaddr_in *addr = (struct sockaddr_in *)uaddr;
        struct sock *sk = sock->sk;
        struct inet_sock *inet = inet_sk(sk);
        struct net *net = sock_net(sk);
        unsigned short snum;
        int chk_addr_ret;
        int err;

        if (sk->sk_prot->bind) {
            err = sk->sk_prot->bind(sk, uaddr, addr_len);
            goto out;
        }
        err = -EINVAL;
        if (addr_len < sizeof(struct sockaddr_in))
            goto out;

        if (addr->sin_family != AF_INET) {
            /* Compatibility games : accept AF_UNSPEC (mapped to AF_INET)
             * only if s_addr is INADDR_ANY.
             */
            err = -EAFNOSUPPORT;
            if (addr->sin_family != AF_UNSPEC ||
                addr->sin_addr.s_addr != htonl(INADDR_ANY))
                goto out;
        }

        chk_addr_ret = inet_addr_type(net, addr->sin_addr.s_addr);

        err = -EADDRNOTAVAIL;
        if (!net->ipv4_sysctl_ip_nonlocal_bind &&
            !(inet->freebind || inet->transparent) &&
            addr->sin_addr.s_addr != htonl(INADDR_ANY) &&
            chk_addr_ret != RTN_LOCAL &&
            chk_addr_ret != RTN_MULTICAST &&
            chk_addr_ret != RTN_BROADCAST)
            goto out;

        snum = ntohs(addr->sin_port);
        err = -EACCES;

        // PROT_SOCK的值为1024，非root用户不能使用小于1024的端口号
        if (snum && snum < PROT_SOCK &&
            !ns_capable(net->user_ns, CAP_NET_BIND_SERVICE))
            goto out;

        lock_sock(sk);

        /* Check these errors (active socket, double bind). */
        err = -EINVAL;

        if (sk->sk_state != TCP_CLOSE || inet->inet_num)
            goto out_release_sock;

        inet->inet_rcv_saddr = inet->inet_saddr = addr->sin_addr.s_addr;
        if (chk_addr_ret == RTN_MULTICAST || chk_addr_ret == RTN_BROADCAST)
            inet->inet_saddr = 0;  /* Use device */

        if ((snum || !inet->bind_address_no_port) &&
            sk->sk_prot->get_port(sk, snum)) {
            // 绑定失败，绑定的地址已经有人在使用
            inet->inet_saddr = inet->inet_rcv_saddr = 0;
            err = -EADDRINUSE;
            goto out_release_sock;
        }

        if (inet->inet_rcv_saddr)
            sk->sk_userlocks |= SOCK_BINDADDR_LOCK;
        if (snum)
            sk->sk_userlocks |= SOCK_BINDPORT_LOCK;
        inet->inet_sport = htons(inet->inet_num);
        inet->inet_daddr = 0;
        inet->inet_dport = 0;
        sk_dst_reset(sk);
        err = 0;
    out_release_sock:
        release_sock(sk);
    out:
        return err;
    }
