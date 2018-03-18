## bio定义
同步阻塞io模型。

## bio实现文件读写
```java
/**
    * BIO模式
    * FileReader逐个字符读取文件,FileReader extends InputStreamReader
    * 读取文件中内容到字符数组中
    * 如何确定字符数组长度：
    * FileReader不能自定义编码读取
    * 此方法也可以用于读取二进制文件,只不过读取出来有很多乱码
    * @param fileName
    * @return
    * @throws IOException
    */
public static char[] readByOneCharWithDefaultEncoding(String fileName) throws IOException{
    File file = new File(fileName);
    FileReader fileReader = new FileReader(file); // 不能自定义编码,内部默认采用系统的编码
    System.out.println("当前采用编码: " + fileReader.getEncoding()); 
    char[] charcters = new char[1024];
    int result = fileReader.read();  // 逐个字符读取，不能按行读取
    int i = 0;
    while(result != -1 && i < 1024){
        char temp = (char)result;
        charcters[i] = temp;
        i++;
        result = fileReader.read();
    }
    fileReader.close();
    return charcters;
}
```

## bio服务器
```java
//BIO服务端源码
public final class ServerNormal {
	//默认的端口号
	private static int DEFAULT_PORT = 12345;
	//单例的ServerSocket
	private static ServerSocket server;
	//根据传入参数设置监听端口，如果没有参数调用以下方法并使用默认值
	public static void start() throws IOException{
		//使用默认值
		start(DEFAULT_PORT);
	}
	//这个方法不会被大量并发访问，不太需要考虑效率，直接进行方法同步就行了
	public synchronized static void start(int port) throws IOException{
		if(server != null) return;
		try{
			//通过构造函数创建ServerSocket
			//如果端口合法且空闲，服务端就监听成功
			server = new ServerSocket(port);
			System.out.println("服务器已启动，端口号：" + port);
			//通过无线循环监听客户端连接
			//如果没有客户端接入，将阻塞在accept操作上。
			while(true){
				Socket socket = server.accept();
				//当有新的客户端接入时，会执行下面的代码
				//然后创建一个新的线程处理这条Socket链路
				new Thread(new ServerHandler(socket)).start();
			}
		}finally{
			//一些必要的清理工作
			if(server != null){
				System.out.println("服务器已关闭。");
				server.close();
				server = null;
			}
		}
	}
}
```

## bio的服务处理handler
```java
/**
 * 客户端线程
 */
public class ServerHandler implements Runnable{
	private Socket socket;
	public ServerHandler(Socket socket) {
		this.socket = socket;
	}
	@Override
	public void run() {
		BufferedReader in = null;
		PrintWriter out = null;
		try{
			in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			out = new PrintWriter(socket.getOutputStream(),true);
			String expression;
			String result;
			while(true){
				//通过BufferedReader读取一行
				//如果已经读到输入流尾部，返回null,退出循环
				//如果得到非空值，就尝试计算结果并返回
				if((expression = in.readLine())==null) break;
				System.out.println("服务器收到消息：" + expression);
				try{
					result = Calculator.cal(expression).toString();
				}catch(Exception e){
					result = "计算错误：" + e.getMessage();
				}
				out.println(result);
			}
		}catch(Exception e){
			e.printStackTrace();
		}finally{
			//一些必要的清理工作
			if(in != null){
				try {
					in.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
				in = null;
			}
			if(out != null){
				out.close();
				out = null;
			}
			if(socket != null){
				try {
					socket.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
				socket = null;
			}
		}
	}
}
```

## bio客户端
```java
/**
 * 阻塞式I/O创建的客户端
 */
public class Client {
	//默认的端口号
	private static int DEFAULT_SERVER_PORT = 12345;
	private static String DEFAULT_SERVER_IP = "127.0.0.1";
	public static void send(String expression){
		send(DEFAULT_SERVER_PORT,expression);
	}
	public static void send(int port,String expression){
		System.out.println("算术表达式为：" + expression);
		Socket socket = null;
		BufferedReader in = null;
		PrintWriter out = null;
		try{
			socket = new Socket(DEFAULT_SERVER_IP,port);
			in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			out = new PrintWriter(socket.getOutputStream(),true);
			out.println(expression);
			System.out.println("___结果为：" + in.readLine());
		}catch(Exception e){
			e.printStackTrace();
		}finally{
			//一下必要的清理工作
			if(in != null){
				try {
					in.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
				in = null;
			}
			if(out != null){
				out.close();
				out = null;
			}
			if(socket != null){
				try {
					socket.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
				socket = null;
			}
		}
	}
}
```

