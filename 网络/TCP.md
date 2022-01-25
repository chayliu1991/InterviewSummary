# TCP 和 UDP 分别对应的常见应用层协议

基于 TCP 的应用层协议有：HTTP、FTP、SMTP、TELNET、SSH

- HTTP：HyperText Transfer Protocol（超文本传输协议），默认端口80
- FTP: File Transfer Protocol (文件传输协议)，默认端口(20用于传输数据，21用于传输控制信息)
- SMTP: Simple Mail Transfer Protocol (简单邮件传输协议) ,默认端口25
- TELNET: Teletype over the Network (网络电传)，默认端口23
- SSH：Secure Shell（安全外壳协议），默认端口 22

基于 UDP 的应用层协议：DNS、TFTP、SNMP

- DNS : Domain Name Service (域名服务),默认端口 53
- TFTP: Trivial File Transfer Protocol (简单文件传输协议)，默认端口 69
- SNMP：Simple Network Management Protocol（简单网络管理协议），通过 UDP 端口 161 接收，只有 Trap 信息采用 UDP 端口162

# TCP 的三次握手

![](./img/tcp_3_handshakes.png)

- 第一次握手(SYN=1, seq=x)，发送完毕后，客户端就进入SYN_SEND 状态
- 第二次握手(SYN=1, ACK=1, seq=y, ACKnum=x+1)， 发送完毕后，服务器端就进入SYN_RCV 状态
- 第三次握手(ACK=1，ACKnum=y+1)，发送完毕后，客户端进入 ESTABLISHED 状态，当服务器端接收到这个包时，也进入ESTABLISHED 状态

为什么不能是两次？不能是四次？



# TCP 四次挥手

![](./img/tcp_4_waves.png)

- 第一次挥手(FIN=1，seq=u)，发送完毕后，客户端进入 FIN_WAIT_1 状态
- 第二次挥手(ACK=1，ack=u+1,seq =v)，发送完毕后，服务器端进入 CLOSE_WAIT 状态，客户端接收到这个确认包之后，进入 FIN_WAIT_2 状态
- 第三次挥手(FIN=1，ACK1,seq=w,ack=u+1)，发送完毕后，服务器端进入 LAST_ACK 状态，等待来自客户端的最后一个 ACK
- 第四次挥手(ACK=1，seq=u+1,ack=w+1)，客户端接收到来自服务器端的关闭请求，发送一个确认包，并进入 TIME_WAIT 状态，等待了某个固定时间（两个最大段生命周期，2MSL，2 Maximum Segment Lifetime）之后，没有收到服务器端的 ACK ，认为服务器端已经正常关闭连接，于是自己也关闭连接，进入 CLOSED 状态。服务器端接收到这个确认包之后，关闭连接，进入 CLOSED 状态

为什么需要四次呢？



Time-Wait 状态：

- 为了保证客户端发送的最后一个 ACK 报文段能够到达服务端。这个 ACK 报文段有可能丢失，因而使处在 LAST-ACK 状态的服务端就收不到对已发送的 FIN + ACK 报文段的确认。服务端会超时重传这个 FIN+ACK 报文段，而客户端就能在 2MSL 时间内收到这个重传的 FIN+ACK 报文段。接着客户端重传一次确认，重新启动 2MSL 计时器。最后，客户端和服务器都正常进入到 CLOSED状态
- 防止已失效的连接请求报文段出现在本连接中。客户端在发送完最后一个 ACK 报文段后，再经过时间 2MSL，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失。这样就可以使下一个连接中不会出现这种旧的连接请求报文段

# TCP 的粘包和拆包

TCP 是面向流，没有界限的一串数据。TCP 底层并不了解上层业务数据的具体含义，它会根据 TCP 缓冲区的实际情况进行包的划分，所以在业务上认为，一个完整的包可能会被 TCP 拆分成多个包进行发送，也有可能把多个小的包封装成一个大的数据包发送，这就是所谓的 TCP 粘包和拆包问题。

![](./img/sticking_and_unpacking.png)

为什么会产生粘包和拆包呢?

- 要发送的数据小于 TCP 发送缓冲区的大小，TCP 将多次写入缓冲区的数据一次发送出去，将会发生粘包
- 接收数据端的应用层没有及时读取接收缓冲区中的数据，将发生粘包
- 要发送的数据大于 TCP 发送缓冲区剩余空间大小，将会发生拆包
- 待发送数据大于 MSS（最大报文长度），TCP 在传输前将进行拆包。即 TCP 报文长度 - TCP头部长度 > MSS。

解决方案：

- 发送端将每个数据包封装为固定长度
- 在数据尾部增加特殊字符进行分割
- 将数据分为两部分，一部分是头部

# TCP 的流量控制



# 保活计时器

客户已主动与服务器建立了TCP连接。但后来客户端的主机突然发生故障。显然，服务器以后就不能再收到客户端发来的数据。因此，应当有措施使服务器不要再白白等待下去。这就需要使用保活计时器了。

服务器每收到一次客户的数据，就重新设置保活计时器，时间的设置通常是两个小时。若两个小时都没有收到客户端的数据，服务端就发送一个探测报文段，以后则每隔 75 秒钟发送一次。若连续发送 10 个探测报文段后仍然无客户端的响应，服务端就认为客户端出了故障，接着就关闭这个连接。

# 大量 CLOSE_WAIT 状态

服务器端收到客户端发送的 FIN 后，TCP协议栈就会自动发送 ACK，接着进入 CLOSE_WAIT状态。但是如果服务器端不执行socket的close()操作，那么就没法进入LAST_ACK,导致大量连接处于 CLOSE_WAIT 状态。所以，如果服务器出现了大量 CLOSE_WAIT 状态，一般是程序 Bug，或者关闭 socket 不及时。

