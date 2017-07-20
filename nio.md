# java Nio原理
## Java NIO 由以下几个核心部分组成：
* Channels
* Buffers：与channel可以相互存取数据
* Selectors：允许单线程处理多个 Channel

## 常用的channel
* FileChannel 从文件中读写数据。
* DatagramChannel 能通过 UDP 读写网络中的数据。
* SocketChannel 能通过 TCP 读写网络中的数据。
* ServerSocketChannel 可以监听TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个 SocketChannel。