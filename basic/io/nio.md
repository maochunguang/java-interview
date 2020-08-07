# java Nio原理
## io和NIO的区别
IO：面向流，阻塞IO
NIO：面向缓冲，非阻塞IO，选择器
NIO (New I/O): NIO是一种同步非阻塞的I/O模型，在Java 1.4 中引入了NIO框架，对应 java.nio 包，提供了 Channel , Selector，Buffer等抽象。NIO中的N可以理解为Non-blocking，不单纯是New。它支持面向缓冲的，基于通道的I/O操作方法。 NIO提供了与传统BIO模型中的 Socket 和 ServerSocket 相对应的 SocketChannel 和 ServerSocketChannel 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发
## Java NIO 由以下几个核心部分组成：
* Channels
* Buffers：与channel可以相互存取数据
* Selectors：允许单线程处理多个 Channel

## 常用的channel
* FileChannel 从文件中读写数据。
* DatagramChannel 能通过 UDP 读写网络中的数据。
* SocketChannel 能通过 TCP 读写网络中的数据。
* ServerSocketChannel 可以监听TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个 SocketChannel。
* DatagramChannel是一个能收发UDP包的通道。因为UDP是无连接的网络协议，所以不能像其它通道那样读取和写入。它发送和接收的是数据包。

## nio实现文件读写

## nio实现简单服务器

## nio实现客户端

