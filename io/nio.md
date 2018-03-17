# java Nio原理
## io和NIO的区别
IO：面向流，阻塞IO
NIO：面向缓冲，非阻塞IO，选择器
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

