socket
========================================
socket函数的原型为 ``int socket(int domain, int type, int protocol);`` 。

其中domain即协议域，又称为协议族(family)。常用的协议族有AF_INET、AF_INET6、AF_LOCAL(或称AF_UNIX，Unix域socket)、AF_ROUTE等等。协议族决定了socket的地址类型，在通信中必须采用对应的地址，如AF_INET决定了要用IPv4地址(32位的)与端口号(16位的)的组合、AF_UNIX要使用一个绝对路径名作为地址。

支持的所有协议定义在 ``/include/linux/socket.h`` 中，具体内容如下：

.. code-block:: c

    /* Supported address families. */
    #define AF_UNSPEC   0
    #define AF_UNIX     1   /* Unix domain sockets      */
    #define AF_LOCAL    1   /* POSIX name for AF_UNIX   */
    #define AF_INET     2   /* Internet IP Protocol     */
    #define AF_AX25     3   /* Amateur Radio AX.25      */
    #define AF_IPX      4   /* Novell IPX           */
    #define AF_APPLETALK    5   /* AppleTalk DDP        */
    #define AF_NETROM   6   /* Amateur Radio NET/ROM    */
    #define AF_BRIDGE   7   /* Multiprotocol bridge     */
    #define AF_ATMPVC   8   /* ATM PVCs         */
    #define AF_X25      9   /* Reserved for X.25 project    */
    #define AF_INET6    10  /* IP version 6         */
    #define AF_ROSE     11  /* Amateur Radio X.25 PLP   */
    #define AF_DECnet   12  /* Reserved for DECnet project  */
    #define AF_NETBEUI  13  /* Reserved for 802.2LLC project*/
    #define AF_SECURITY 14  /* Security callback pseudo AF */
    #define AF_KEY      15      /* PF_KEY key management API */
    #define AF_NETLINK  16
    #define AF_ROUTE    AF_NETLINK /* Alias to emulate 4.4BSD */
    #define AF_PACKET   17  /* Packet family        */
    #define AF_ASH      18  /* Ash              */
    #define AF_ECONET   19  /* Acorn Econet         */
    #define AF_ATMSVC   20  /* ATM SVCs         */
    #define AF_RDS      21  /* RDS sockets          */
    #define AF_SNA      22  /* Linux SNA Project (nutters!) */
    #define AF_IRDA     23  /* IRDA sockets         */
    #define AF_PPPOX    24  /* PPPoX sockets        */
    #define AF_WANPIPE  25  /* Wanpipe API Sockets */
    #define AF_LLC      26  /* Linux LLC            */
    #define AF_IB       27  /* Native InfiniBand address    */
    #define AF_MPLS     28  /* MPLS */
    #define AF_CAN      29  /* Controller Area Network      */
    #define AF_TIPC     30  /* TIPC sockets         */
    #define AF_BLUETOOTH    31  /* Bluetooth sockets        */
    #define AF_IUCV     32  /* IUCV sockets         */
    #define AF_RXRPC    33  /* RxRPC sockets        */
    #define AF_ISDN     34  /* mISDN sockets        */
    #define AF_PHONET   35  /* Phonet sockets       */
    #define AF_IEEE802154   36  /* IEEE802154 sockets       */
    #define AF_CAIF     37  /* CAIF sockets         */
    #define AF_ALG      38  /* Algorithm sockets        */
    #define AF_NFC      39  /* NFC sockets          */
    #define AF_VSOCK    40  /* vSockets         */
    #define AF_KCM      41  /* Kernel Connection Multiplexor*/
    #define AF_QIPCRTR  42  /* Qualcomm IPC Router          */
    #define AF_SMC      43  /* smc sockets: reserve number for
                     * PF_SMC protocol family that
                     * reuses AF_INET address family
                     */
    #define AF_XDP      44  /* XDP sockets          */

    #define AF_MAX      45  /* For now.. */


type指定socket类型，常用的socket类型为:

.. code-block:: c

    enum sock_type {
        SOCK_STREAM = 1, // 字节流套接字
        SOCK_DGRAM  = 2, // 数据报文套接字
        SOCK_RAW    = 3, // 原始套接字
        SOCK_RDM    = 4,
        SOCK_SEQPACKET  = 5, // 有序分组套接字
        SOCK_DCCP   = 6,
        SOCK_PACKET = 10,
    };

protocol指协议，常用的协议类型为:

.. code-block:: c

    /* Standard well-defined IP protocols.  */
    enum {
      IPPROTO_IP = 0,       /* Dummy protocol for TCP       */
    #define IPPROTO_IP      IPPROTO_IP
      IPPROTO_ICMP = 1,     /* Internet Control Message Protocol    */
    #define IPPROTO_ICMP        IPPROTO_ICMP
      IPPROTO_IGMP = 2,     /* Internet Group Management Protocol   */
    #define IPPROTO_IGMP        IPPROTO_IGMP
      IPPROTO_IPIP = 4,     /* IPIP tunnels (older KA9Q tunnels use 94) */
    #define IPPROTO_IPIP        IPPROTO_IPIP
      IPPROTO_TCP = 6,      /* Transmission Control Protocol    */
    #define IPPROTO_TCP     IPPROTO_TCP
      IPPROTO_EGP = 8,      /* Exterior Gateway Protocol        */
    #define IPPROTO_EGP     IPPROTO_EGP
      IPPROTO_PUP = 12,     /* PUP protocol             */
    #define IPPROTO_PUP     IPPROTO_PUP
      IPPROTO_UDP = 17,     /* User Datagram Protocol       */
    #define IPPROTO_UDP     IPPROTO_UDP
      IPPROTO_IDP = 22,     /* XNS IDP protocol         */
    #define IPPROTO_IDP     IPPROTO_IDP
      IPPROTO_TP = 29,      /* SO Transport Protocol Class 4    */
    #define IPPROTO_TP      IPPROTO_TP
      IPPROTO_DCCP = 33,        /* Datagram Congestion Control Protocol */
    #define IPPROTO_DCCP        IPPROTO_DCCP
      IPPROTO_IPV6 = 41,        /* IPv6-in-IPv4 tunnelling      */
    #define IPPROTO_IPV6        IPPROTO_IPV6
      IPPROTO_RSVP = 46,        /* RSVP Protocol            */
    #define IPPROTO_RSVP        IPPROTO_RSVP
      IPPROTO_GRE = 47,     /* Cisco GRE tunnels (rfc 1701,1702)    */
    #define IPPROTO_GRE     IPPROTO_GRE
      IPPROTO_ESP = 50,     /* Encapsulation Security Payload protocol */
    #define IPPROTO_ESP     IPPROTO_ESP
      IPPROTO_AH = 51,      /* Authentication Header protocol   */
    #define IPPROTO_AH      IPPROTO_AH
      IPPROTO_MTP = 92,     /* Multicast Transport Protocol     */
    #define IPPROTO_MTP     IPPROTO_MTP
      IPPROTO_BEETPH = 94,      /* IP option pseudo header for BEET */
    #define IPPROTO_BEETPH      IPPROTO_BEETPH
      IPPROTO_ENCAP = 98,       /* Encapsulation Header         */
    #define IPPROTO_ENCAP       IPPROTO_ENCAP
      IPPROTO_PIM = 103,        /* Protocol Independent Multicast   */
    #define IPPROTO_PIM     IPPROTO_PIM
      IPPROTO_COMP = 108,       /* Compression Header Protocol      */
    #define IPPROTO_COMP        IPPROTO_COMP
      IPPROTO_SCTP = 132,       /* Stream Control Transport Protocol    */
    #define IPPROTO_SCTP        IPPROTO_SCTP
      IPPROTO_UDPLITE = 136,    /* UDP-Lite (RFC 3828)          */
    #define IPPROTO_UDPLITE     IPPROTO_UDPLITE
      IPPROTO_MPLS = 137,       /* MPLS in IP (RFC 4023)        */
    #define IPPROTO_MPLS        IPPROTO_MPLS
      IPPROTO_ETHERNET = 143,   /* Ethernet-within-IPv6 Encapsulation   */
    #define IPPROTO_ETHERNET    IPPROTO_ETHERNET
      IPPROTO_RAW = 255,        /* Raw IP packets           */
    #define IPPROTO_RAW     IPPROTO_RAW
      IPPROTO_MPTCP = 262,      /* Multipath TCP connection     */
    #define IPPROTO_MPTCP       IPPROTO_MPTCP
      IPPROTO_MAX
    };
