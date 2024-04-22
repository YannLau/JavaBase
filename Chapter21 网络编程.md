P662---P

[toc]

# 网络相关概念

> 网络通信
>
> 1. 概念：两台设备之间通过网络实现数据传输
> 2. 网络通信：将数据通过网络从一台设备传输到另一台设备
> 3. java.net 包下提供了一系列的类或接口，供程序员使用，完成网络通信
>
> 网络
>
> 1. 概念：两台或多台设备通过一定物理设备连接起来构成了网络
> 2. 根据网络的覆盖范围不同，对网络进行分类：
> • 局域网：覆盖范围最小，仅仅覆盖一个教室或一个机房
> • 城域网：覆盖范围较大，可以覆盖一个城市
> •广域网：覆盖范围最大，可以覆盖全国，甚至全球，万维网是广域网的代表

> •域名
> 1. www.baidu.com
> 2. 好处：为了方便记忆，解决记ip的困难
> 3. 概念：将ip地址映射成域名，这里怎么映射上，HTTP协议
>
> •端口号
>
> 1. 概念：用于标识计算机上某个特定的网络程序
> 2. 表示形式：以整数形式，端口范围0~65535【2个字节表示端口 0~2^16-1］
> 3. 0~1024已经被占用，比如 ssh 22, ftp 21, smtp 25 http 80 https 443
> 4. 常见的网络程序端口号：
>    • tomcat :8080
>    • mysql:3306
>    • oracle:1521
>    • sqlserver:1433

在网络开发中，不要使用0-1024的端口 , 名花有主

> ＞ 协议（tcp/ip）
>
> ![image-20240418下午94338234](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午94338234.png)TCP/IP (Transmission Control Protocol/Internet Protocol)的简写，
> 中文译名为传输控制协议/因特网互联协议，又叫网络通讯协议，这个协议是Internet最基本的协议、Internet国际互联网络的基础，简单地说，就是由网
> 络层的IP协议和传输层的TCP协议组成的。［示意图］
>
> ![image-20240418下午94435581](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午94435581.png)

> TCP 和 UDP
> TCP协议：传输控制协议
>
> 1. 使用TCP协议前，须先建立TCP连接，形成传输数据通道
>
> 2. 传输前，采用“三次握手“方式，是可靠的
>     3.TCP协议进行通信的两个应用进程：客户端、服务端
> 3. 在连接中可进行大数据量的传输
> 4. 传输完毕，需释放已建立的连接，效率低
>
> UDP协议：用户数据协议
>
> 2. 将数据、源、目的封装成数据包，不需要建立连接
> 3. 每个数据报的大小限制在64K内，不适合传输大量数据
> 4. 因无需连接，故是不可靠的
> 5. 发送数据结束时无需释放资源（因为不是面向连接的），速度快
> 6. 举例：厕所通知：发短信

# InetAddress类

> 相关方法
>
> 1. 获取本机InetAddress对象 getLocalHost
> 2. 根据指定主机名/域名获取ip地址对象 getByName
> 3. 获取InetAddress对象的主机名 getHostName
> 4. 获取InetAddress对象的地址 getHostAddress

```java
public class Net_test {
    public static void main(String[] args) throws UnknownHostException {
        //1. 获取本机的InetAddress对象
        InetAddress localHost = InetAddress.getLocalHost();
        System.out.println(localHost);//Yann-LauMBP/192.168.10.193
        //2. 根据指定主机名 获取 InetAddress对象
        InetAddress host1 = InetAddress.getByName("Yann-LauMBP");
        System.out.println("host1= " + host1);
        //3. 根据域名返回 InetAddress对象,如www.baidu.com
        InetAddress host2 = InetAddress.getByName("www.baidu.com");
        System.out.println("host2= " + host2);
        host2 = InetAddress.getByName("www.google.com");
        System.out.println(host2);
        //4. 通过InetAddress对象,获取对应的地址
        String hostAddress = host2.getHostAddress();
        System.out.println(hostAddress);
        //5. 通过hostName对象 ,获取对应的主机名/或者域名
        String hostName = host2.getHostName();
        System.out.println("host2对应的主机名/域名 = " + hostName);
    }
}
```

# Socket 套接字/插座

> 基本介绍
>
> 1. 套接字（Socket）开发网络应用程序被广泛采用，以至于成为事实上的标准。
> 2. 通信的两端都要有Socket，是两台机器间通信的端点
> 3. 网络通信其实就是Socket的通信。
> 4. Socket允许程序把网络连接当成一个流，数据在两个Socket间通过IO传输。
> 5. 一般主动发起通信的应用程序属客户端，等待通信请求的为服务端

![image-20240220上午103631962](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220上午103631962.png)

> TCP网络通信编程
>
> ![image-20240220上午104157484](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220上午104157484.png)	

> 应用案例1(使用字节流)
>
> SocketTCP01Client.java
>
> SocketTCP01Server.java 
>
> 1. 编写一个服务器端，和一个客户端
>
> 2. 服务器端在 9999 端口监听
> 3. 客户端连接到服务器端，发送 “hello, server"，然后退出
> 4. 服务器端接收到 客户端发送的 信息，输出，并退出

![image-20240220上午104946012](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220上午104946012.png)

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketTCP01Server {
  public static void main(String[] args) throws IOException {
    //思路
    //1. 在本机的 9999 端口监听,等待连接
    //   细节:要求本机其他服务没有占用9999端口
    //   细节:这个ServerSocket可以通过accept()返回多个Socket[多并发:多个客户端来连接服务器]
    ServerSocket serverSocket = new ServerSocket(9999);
    System.out.println("服务端正在9999端口监听等待连接");
    //2. 当没有客户端连接9999端口时,程序会阻塞在accept,类似于scanner等待,等待连接
    //   如果有客户端连接，则会返回Socket对象，程序继续
    Socket socket = serverSocket.accept();
    System.out.println("服务器端socket= " + socket.getClass());
    //3. 通过Socket.getInputStream(),读取
    //客户端写入到数据通道的数据,显示
    InputStream inputStream = socket.getInputStream();
    //会等待写完，哪怕客户端sleep，这里也会等着。flush也不行
    String s = new String(inputStream.readAllBytes());
    System.out.println(s);
    
    //也可以这样读取
    //byte[] buf = new byte[1024];
    //int readLen = 0;
    //while ((readLen = inputStream.read(buf)) != -1) {
    //  System.out.println(new String(buf, 0, readLen));//根据读取到的实际长度，显示内容. 
    //}
    
    //4. 关闭流和socket
    inputStream.close();
    socket.close();
    serverSocket.close();
    System.out.println("服务端退出了...");
  }
}
```

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

public class SocketTCP01Client {
    public static void main(String[] args) throws IOException, InterruptedException {
        //思路
        //1. 连接服务端(ip,端口)
        //解读:连接本机的 9999 端口,如果连接成功,返回Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端socket返回 = " + socket.getClass());
        //2. 连接上后,生成Socket,通过Socket.getOutputStream()
        //   得到 和 socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //OutputStream是一个抽象类,后面的方法返回的类型也是它,但实际是一个class java.net.Socket$SocketOutputStream对象
        System.out.println("outputStream的运行类型是 " + outputStream.getClass());
        //3. 通过输出流,写入数据到 数据管道
        outputStream.write("hello,server!".getBytes());
        Thread.sleep(3000);
        outputStream.write("我丢你楼某".getBytes());
        //4. 关闭流对象和socket,必须关闭
        outputStream.close();
        socket.close();
        System.out.println("客户端退出了...");
    }
}
```

> 应用案例2(使用字节流) SocketTCP02.java
>
> 1. 编写一个服务器端和一个客户端
> 2. 服务器端在 9999 端口监听
> 3. 客户端连接到服务器端，发送 "hello, server"，并接收服务器端回发的"hello, client" , 再退出
> 4. 服务器端接收到 客户端发送的 信息，输出，并发送"hello, client"，再退出

![image-20240220下午125732640](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220下午125732640.png)

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketTCP02Server {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 在本机的 9999 端口监听,等待连接
        //   细节:要求本机其他服务没有占用9999端口
        //   细节:这个ServerSocket可以通过accept()返回多个Socket[多并发:多个客户端来连接服务器]
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("服务端正在9999端口监听等待连接");
        //2. 当没有客户端连接9999端口时,程序会阻塞在accept,类似于scanner等待,等待连接
        //   如果有客户端连接，则会返回Socket对象，程序继续
        Socket socket = serverSocket.accept();
        //System.out.println("服务器端socket= " + socket.getClass());
        //3. 通过Socket.getInputStream(),读取
        //     客户端写入到数据通道的数据,显示
        InputStream inputStream = socket.getInputStream();
        byte[] buffer = new byte[1024];
        int bytesRead;
        while ((bytesRead = inputStream.read(buffer)) != -1) {
            String s = new String(buffer,0,bytesRead);
            System.out.println(s);
        }
        //4. 获取socket相关联的输出流
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("hello,Client!".getBytes());
        //设置结束标记
        socket.shutdownOutput();
        //5. 关闭流和socket
        outputStream.close();
        inputStream.close();
        socket.close();
        serverSocket.close();
        System.out.println("Server关闭服务...");
    }
}

```

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

public class SocketTCP02Client {
    public static void main(String[] args) throws IOException, InterruptedException {
        //思路
        //1. 连接服务端(ip,端口)
        //解读:连接本机的 9999 端口,如果连接成功,返回Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        //System.out.println("客户端socket返回 = " + socket.getClass());
        //2. 连接上后,生成Socket,通过Socket.getOutputStream()
        //   得到 和 socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //OutputStream是一个抽象类,后面的方法返回的类型也是它,但实际是一个class java.net.Socket$SocketOutputStream对象
        //System.out.println("outputStream的运行类型是 " + outputStream.getClass());
        //3. 通过输出流,写入数据到 数据管道
        outputStream.write("hello,server!".getBytes());
        //Thread.sleep(3000);
        //outputStream.write("我丢你楼某".getBytes());
        // 设置结束标记
        socket.shutdownOutput();
        //4. 获取和socket关联的输入流,读取数据字节,并显示
        InputStream inputStream = socket.getInputStream();
        byte[] buffer = new byte[1024];
        int bytesRead = 0;
        while ((bytesRead = inputStream.read(buffer)) != -1) {
            String s = new String(buffer, 0, bytesRead);
            System.out.println(s);
        }
        //5. 关闭流对象和socket,必须关闭
        inputStream.close();
        outputStream.close();
        socket.close();
        System.out.println("客户端退出了...");
    }
}

```

> 应用案例3(使用字符流) SocketTCP03.java
>
> 1. 编写一个服务端，和一个客户端
> 2. 服务端在9999端口监听
> 3. 客户端连接到服务端，发送 “hello, server"，并接收服务端回发的"hello,client"再退出
> 4. 服务端接收到 客户端发送的信息，输出，并发送"hello,client”，再退出

![image-20240220下午22749648](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220下午22749648.png)

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketTCP03Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        Socket socket = serverSocket.accept();

        InputStream inputStream = socket.getInputStream();
        InputStreamReader isr = new InputStreamReader(inputStream);
        BufferedReader br = new BufferedReader(isr);
        String s = null;
//        while ((s = br.readLine()) != null && !s.isEmpty()) {
// 				如果使用while接收显示信息，需要使用 shutdownOutput
//            System.out.println(s);
//        }
        s = br.readLine();
        System.out.println(s);

        OutputStream outputStream = socket.getOutputStream();
        OutputStreamWriter osw = new OutputStreamWriter(outputStream);
        BufferedWriter bw = new BufferedWriter(osw);
        bw.write("Hello,Client!"); 
        bw.newLine();// 插入一个换行符，表示回复内容的结束
        bw.flush();


        bw.close();
        br.close();
        socket.close();
        serverSocket.close();
    }
}
```

```java
import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

public class SocketTCP03Client {
    public static void main(String[] args) throws IOException {

        Socket socket = new Socket(InetAddress.getLocalHost(),9999);

        OutputStream outputStream = socket.getOutputStream();
        OutputStreamWriter osw = new OutputStreamWriter(outputStream);
        BufferedWriter bw = new BufferedWriter(osw);

        bw.write("Hello,Server!");
        bw.newLine(); // 插入一个换行符，表示写入的内容结束,注意，要求对方使用对应配套的readLine()
        bw.flush();// 如果使用的字符流，需要手动刷新，否则数据不会写入数据通道


        InputStream inputStream = socket.getInputStream();
        InputStreamReader isr = new InputStreamReader(inputStream);
        BufferedReader br = new BufferedReader(isr);
        String s = null;
//        while ((s = br.readLine()) != null && !s.isEmpty()) {
//            System.out.println(s);
//        }
        s = br.readLine();
        System.out.println(s);

        br.close();
        bw.close();
        socket.close();
    }
}

```

> 应用案例4 
>
> 1. 编写一个服务端，和一个客户端
> 2. 服务器端在8888端口监听
> 3. 客户端连接到服务端，发送一张图片 e:\\\qie.png
> 4. 服务器端接收到 客户端发送的 图片，保存到src下，发送“收到图片"再退出
> 5. 客户端接收到服务端发送的“收到图片"，再退出
> 6. 该程序要求使用 StreamUtils.java
> 7. 说明：使用BufferedlnputStream 和 BufferedOutputStream 字节流

![image-20240220下午34709196](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220下午34709196.png)

以下代码有一个问题 , 可能涉及到系统原因

Client端无法接收到Server端发送的"收到图片"消息~等待解决!

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPFIleUploadServer {
    public static void main(String[] args) throws Exception {
        //1. 服务端代码在本机监听 8888 端口
        ServerSocket serverSocket = new ServerSocket(8888);
        System.out.println("服务端在8888端口监听中...");
        //2. 等待连接...
        Socket socket = serverSocket.accept();
        //3. 读取客户端传送过来的数据
        BufferedInputStream bis = new BufferedInputStream(socket.getInputStream());
        byte[] bytes = StreamUtil.streamToByteArray(bis);
        //4. 将得到 bytes 数组,写入到指定的路径,就得到一个文件了
        String filePath = "/Users/yannlau/Downloads/wmj.jpg";
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(filePath));
        bos.write(bytes);
        bos.close();

        //向客户端回复"收到图片!"
        //通过socket获取到输出流(字符)
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        bw.write("收到图片!");
        bw.flush();
        socket.shutdownOutput();//设置写入结束标记

        //关闭其他资源
        bis.close();
        bw.close();
        socket.close();
        serverSocket.close();
    }
}
```

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.InetAddress;
import java.net.Socket;

public class TCPFIleUploadClient {
    public static void main(String[] args) throws Exception {
        //客户端连接服务端 8888 端口, 得到 Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(),8888);
        //创建读取磁盘文件的输入流
        String filePath = "/Users/yannlau/Downloads/墙纸/wallhaven-6d7xmx.jpg";
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(filePath));
        //bytes就是文件对应的字节数组
        byte[] bytes = StreamUtil.streamToByteArray(bis);
        //通过socket获得输出流,将bytes数组传送给服务器
        BufferedOutputStream bos = new BufferedOutputStream(socket.getOutputStream());
        bos.write(bytes);// 将文件对应的文件数组的内容,写入数据通道
        bis.close();
        socket.shutdownOutput(); //设置写入数据的结束标记

        // ====接受从服务端返回的消息====
        InputStream is = socket.getInputStream();
        String s = StreamUtil.streamToString(is);
        System.out.println("Server Response: " + s);
        //关闭相关的流
        is.close();
        bos.close();
        socket.close();
    }
}
```

```java
import java.io.BufferedReader;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

public class StreamUtil {
    public static byte[] streamToByteArray(InputStream is) throws Exception {
      	//创建输出流对象
        ByteArrayOutputStream bos = new ByteArrayOutputStream(); 
        byte[] b = new byte[1024]; //字节数组
        int len;
        while((len=is.read(b))!=-1){ //循环读取
            bos.write(b,0,len);
        }
        byte[] array = bos.toByteArray();
        bos.close();
        return array;
    }
    public static String streamToString(InputStream is) throws Exception {
        BufferedReader reader = new BufferedReader(new InputStreamReader(is));
        StringBuilder builder = new StringBuilder();
        String line;
        while((line=reader.readLine())!=null){
            builder.append(line+"\n");
        }
        return builder.toString();
    }
}
```

## netstat指令

![image-20240418下午114443569](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午114443569.png)

> 1. netstat -an 可以查看当前主机网络情况，包括端口监听情况和网络连接情况
> 2. netstat -an | more 可以分页显示
> 3. netstat  -anb | more 可以显示程序名字,需要管理员运行
> 4. 要求在dos控制台下执行

> • TCP网络通讯不为人知的秘密
>
> 当客户端连接到服务端后，实际上客户端也是通过一个端口和服务端进行通讯的，这个端口是TCP/IP 来分配的，！是不确定的，是随机的。

# UDP 网络通信编程(了解)

> 1. 类 DatagramSocket 和 DatagramPacket［数据包/数据报］实现了基于 UDP协议网络程序。
> 2. UDP数据报通过数据报套接字 DatagramSocket 发送和接收，系统不保证UDP数据报一定能够安全送到目的地，也不能确定什么时候可以抵达。
> 3. DatagramPacket 对象封装了UDP数据报，在数据报中包含了发送端的IP地址和端口号以及接收端的IP地址和端口号。
> 4. UDP协议中每个数据报都给出了完整的地址信息，因此无须建立发送方和接收方的连接

> 基本流程
>
> 1. 核心的两个类/对象 DatagramSocket与DatagramPacket
> 2. 建立发送端，接收端 (没有服务端和客户端概念)
> 3. 发送数据前 , 建立数据包/报 DatagramPacket对象
> 4. 调用DatagramSocket的发送、接收方法
> 5. 关闭DatagramSocket

![image-20240220下午83344711](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220下午83344711.png)

![image-20240220下午83740456](/Users/yannlau/Library/Application Support/typora-user-images/image-20240220下午83740456.png)

> 应用案例
>
> 1. 编写一个接收端A，和一个发送端B
> 2. 接收端A在9999端口等待接收数据（receive）
> 3. 发送端B向接收端A发送 数据“hello，明天吃火锅~"
> 4. 接收端A接收到 发送端B发送的数据，回复“好的，明天见”，再退出
> 5. 发送端接收回复的数据，再退出

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPReceiverA {
    public static void main(String[] args) throws IOException {
        //1. 创建一个 DatagramSocket对象,准备在9999端口接收数据
        DatagramSocket datagramSocket = new DatagramSocket(9999);
        //2. 构建一个 DatagramPacket对象,准备接收数据
        //在前面讲解UDP 协议时，老师说过一个数据包最大 64k
        byte[] buf = new byte[64*1024];
        DatagramPacket packet = new DatagramPacket(buf, buf.length);
        //3. 调用 接收方法,将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        // 老师提示：当有数据包发送到 本机的9999端口时，就会接收到数据
        // 如果没有数据包发送到 本机的9999端口，就会阻塞等待。
        System.out.println("接收端A 等待接收数据...");
        datagramSocket.receive(packet);
        //4. 可以把packet 进行拆包,取出数据 ,并显示
        int length = packet.getLength();//实际接收到的数据长度
        byte[] data = packet.getData();//接受到数据
        String s = new String(data,0,length);
        System.out.println(s);
        //A端回复信息
        byte[] bytes = "好的明天见!".getBytes();
        DatagramPacket packet1 =
                new DatagramPacket(bytes,0,bytes.length, InetAddress.getLocalHost(),9998);
        datagramSocket.send(packet1);

        //5. 关闭资源
        datagramSocket.close();
        System.out.println("A端退出!");
    }
}
```

```java
import java.io.IOException;
import java.net.*;

public class UDPSenderB {
    public static void main(String[] args) throws IOException {
        //1. 创建 DatagramSocket对象,准备在9998端口接收数据
        DatagramSocket socket = new DatagramSocket(9998);
        //2. 将需要发送的数据,封装到DatagramPacket对象
        byte[] bytes = "hello 明天吃火锅~".getBytes();
        //说明:封装的 DatagramPacket对象 data 内容字节数组，data. Length
        //主机（IP〕，端口
        DatagramPacket datagramPacket =
                new DatagramPacket(bytes, 0, bytes.length, InetAddress.getLocalHost(), 9999);
        socket.send(datagramPacket);

        //B端接受A的回复
        byte[] bytes1 = new byte[1024*64];
        DatagramPacket datagramPacket1 = new DatagramPacket(bytes1,bytes1.length);
        socket.receive(datagramPacket1);
        int length = datagramPacket1.getLength();
        byte[] data = datagramPacket1.getData();
        String s = new String(data,0,length);
        System.out.println(s);

        //关闭资源
        socket.close();
        System.out.println("B端退出!");
    }
}
```

# Homework

> 本章作业 1
>
> （1） 使用字符流的方式，编写一个客户端程序和服务器端程序.
> （2） 客户端发送 “name”，服务器端接收到后，返回“我是 nova"  nova 是你自己的名字.
> （3） 客户端发送 “hobby"，服务器端接收到后，返回“编写java程序"
> （4） 不是这两个问题，回复 “你说啥呢"

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class Homework01Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        //服务器主机的9999端口等待连接中...
        Socket socket = serverSocket.accept();
        //接收消息,字符流
        InputStream inputStream = socket.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));
        String s = br.readLine();

        String answer = "";
        if (s.equals("name")) {
            answer = "我是YannLau";
        } else if (s.equals("hobby")) {
            answer="编写Java程序";
        } else {
            answer = "你说的啥子?";
        }

        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(outputStream));
        bw.write(answer);
        bw.newLine();
        bw.flush();

        br.close();
        bw.close();
        socket.close();
        serverSocket.close();
    }
}
```

```java
import java.io.*;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

public class Homework01Client {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);

        OutputStream outputStream = socket.getOutputStream();

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(outputStream));
        Scanner scanner = new Scanner(System.in);
        System.out.print("请输入你的问题-> ");
        String message = scanner.nextLine();
        bw.write(message);
        bw.newLine();
        bw.flush();

        InputStream inputStream = socket.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));
        String s = br.readLine();
        System.out.println(s);

        br.close();
        bw.close();
        socket.close();
        System.out.println("客户端退出...");
    }
}
```

> 本章作业 2
>
> （1） 编写一个接收端A，和一个发送端B，使用UDP协议完成
> （2） 接收端在8888端口等待接收数据（receive）
> （3） 发送端向接收端 发送 数据“四大名著是哪些"
> （4） 接收端接收到 发送端发送的 问题后，返回“四大名著是＜＜红楼梦>>…“，否则返回what?
> （5） 接收端和发送端程序退出

```java

```

```java

```

> 本章作业3
>
> （1） 编写客户端程序和服务器端程序
> （2） 客户端可以输入 一个 音乐 文件名，比如 高山流水，服务端 收到音乐名后，可以给客户端 返回这个 音乐文件，如果服务器没有这个文件，返回一个默认的音乐即可.
> （3） 客户端收到文件后，保存到本地 e:\\
> （4） 该程序可以使用 StreamUtils.java
>
> 本质: 其实就是指定下载文件的应用

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class Homework03server {
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(9999);
        Socket socket = serverSocket.accept();

        InputStream inputStream = socket.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));

        String question = br.readLine();
//        String question = StreamUtil.streamToString(socket.getInputStream());
        byte[] bytes;
        if ("三全音".equals(question)) {
            InputStream inputStream1 = new FileInputStream("/Users/yannlau/Downloads/iPhone三全音  YouTube.mp3");
            bytes = StreamUtil.streamToByteArray(inputStream1);
        } else {
            InputStream inputStream2 = new FileInputStream("/Users/yannlau/Downloads/谁live.mp3");
            bytes = StreamUtil.streamToByteArray(inputStream2);
        }

        OutputStream outputStream = socket.getOutputStream();
        BufferedOutputStream bos = new BufferedOutputStream(outputStream);
        bos.write(bytes);
        socket.shutdownOutput();

        br.close();
        bos.close();
        socket.close();
        System.out.println("服务器退出程序~~~");
    }
}

```

```java
import java.io.*;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

public class Homework03client {
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket(InetAddress.getLocalHost(),9999);
        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(outputStream));
        System.out.print("请输入你要求的下载内容->");
        Scanner scanner = new Scanner(System.in);
        String request = scanner.nextLine();

        bw.write(request);
        bw.newLine();
        bw.flush();

        byte[] bytes;
        bytes = StreamUtil.streamToByteArray(socket.getInputStream());
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("/Users/yannlau/Downloads/2.17/22.mp3"));
        bos.write(bytes);
        System.out.println("下载完成!");

        bw.close();
        bos.close();
        socket.close();
        System.out.println("客户端退出程序~~~");
    }
}
```

# 字符流多行输入

java IO流和socket网络编程结合时，如何实现字符流多行读取readline

ChatGPT

在Java的IO流和Socket网络编程结合时，要实现字符流多行读取（`readLine`），可以使用`BufferedReader`类。`BufferedReader`类提供了`readLine`方法，它可以方便地读取一行文本数据。以下是一个简单的示例：

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(8888);
            System.out.println("Server is running and waiting for connection...");

            Socket clientSocket = serverSocket.accept();
            System.out.println("Client connected.");

            BufferedReader reader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));

            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println("Received from client: " + line);
            }

            // Close resources
            reader.close();
            clientSocket.close();
            serverSocket.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

在客户端，可以使用`BufferedWriter`来发送多行文本数据：

```java
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;
import java.net.Socket;

public class Client {
    public static void main(String[] args) {
        try {
            Socket socket = new Socket("localhost", 8888);

            BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

            // Sending multiple lines
            writer.write("Hello, Server!");
            writer.newLine();
            writer.write("How are you?");
            writer.newLine();
            writer.write("Goodbye!");

            // Flush to ensure data is sent
            writer.flush();

            // Close resources
            writer.close();
            socket.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

注意事项：

1. `BufferedReader`的`readLine`方法会阻塞，直到读取到一行数据或者输入流结束。
2. 在客户端使用`BufferedWriter`的`newLine`方法来插入换行符，以确保服务器端能够通过`readLine`正确读取多行文本。
