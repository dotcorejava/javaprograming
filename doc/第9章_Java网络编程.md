# 第9章 Java网络编程

## TCP/IP 网络模型
- 网络接口层  
包括用于协作IP数据在已有网络介质上传输的协议。它定义像地址解析(Address Resolution Protocol,ARP)这样的协议,提供TCP/IP协议数据结构和实际物理硬件之间的接口。
- 网际层  
对应于OSI七层参考模型的网络层。本层包含IP协议、RIP协议(Routing Information Protocol,路由信息协议),负责数据的包装、寻址和路由。同时还包含网间控制报文协议(Internet Control Message Protocol,ICMP)用于提供网络诊断信息。
- **传输层**  
对应于OSI七层参考模型的传输层,提供两种端到端的通信服务。其中TCP协议(Transmission Control Protocol)提供可靠的数据流运输服务,UDP协议(Use Datagram Protocol)提供不可靠的用户数据包服务。
- 应用层  
对应于OSI七层参考模型的应用层和表达层。因特网的应用层协议包括Finger、Whois、FTP(文件传输协议)、Gopher、HTTP(超文本传输协议)、Telent(远程终端协议)、SMTP(简单邮件传送协议)、IRC(因特网中继会话)和NNTP(网络新闻传输协议)等。

## UDP 协议网络程序
UDP(User Datagram Protocol)是一种无连接的协议,每个数据包都是一个独立的信息,包括完整的源地址或目的地址。由于UDP在网络上是以任何可能的路径传往目的地的,因此能否到达目的地、到达目的地的时间及内容的正确性都是不能被保证的。使用UDP时,每个数据包中都给出了完整的地址信息,无须再建立发送方和接收方的连接。但使用UDP传输数据是有大小限制的,每个被传输的数据包必须限定在64KB之内。UDP是一个不可靠的协议,发送方所发送的数据包不一定以相同的次序到达接收方。在java.net包中提供了两个类:DatagramSocket和DatagramPacket,用于支持数据包通信。DatagramSocket类用于在程序之间建立传送数据包的通信连接,DatagramPacket类用于表示一个数据包。
```
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class HelloWorld {
        public static void main(String[] args) throws InterruptedException {
                Thread server = new Thread(new Server());
                server.start();
                Thread.sleep(1000);

                Thread client = new Thread(new Client());
                client.start();
        }

}

class Server implements Runnable {

        @Override
        public void run() {
                try {
                        DatagramSocket socket = new DatagramSocket(1090);
                        byte[] data = new byte[1024];
                        while (true) {
                                DatagramPacket dataPackage = new DatagramPacket(data, data.length);
                                socket.receive(dataPackage);
                                String dataString = new String(dataPackage.getData());
                                System.out.println(dataString);
                                if (dataString.substring(0, 4).equals("exit")) {
                                        socket.close();
                                        System.out.println("客户端退出，断开连接");
                                        break;
                                }
                                dataPackage = new DatagramPacket("服务端已受到信息".getBytes(), "服务端已受到信息".getBytes().length,
                                                dataPackage.getSocketAddress());
                                socket.send(dataPackage);
                        }
                } catch (IOException e) {
                        e.printStackTrace();
                }

        }

}

class Client implements Runnable {

        @Override
        public void run() {
                byte[] data = new byte[1024];
                // String sendString = "客户端信息";
                String sendString = "exit";
                try {
                        InetAddress address = InetAddress.getByName("127.0.0.1");

                        // DatagramPacket datagramPacket = new DatagramPacket(data, data.length,
                        // address, 1090);
                        DatagramPacket datagramPacket = new DatagramPacket(sendString.getBytes(),
                                        sendString.getBytes().length, address, 1090);
                        DatagramSocket socket = new DatagramSocket();
                        socket.send(datagramPacket);

                        datagramPacket = new DatagramPacket(data, data.length);
                        socket.receive(datagramPacket);
                        System.out.println(new String(datagramPacket.getData()));
                        socket.close();

                } catch (IOException e) {
                        e.printStackTrace();
                }
        }
}
```

## TCP 协议网络程序
TCP(Tranfer Control Protocol)是一种面向连接的、保证可靠传输的协议。通过TCP协议传输,得到的是一个顺序的、无差错的数据流。发送方和接收方成对的两个套接字之间必须建立连接,一旦两个套接字连接起来,它们就可以进行双向数据传输,双方都可以进行发送或接收操作。与UDP不同,TCP对传输数据的大小没有限制。TCP是一个可靠的协议,其确保接收方完全正确地获取发送方所发送的全部数据。在java.net包中提供了两个类:Socket和ServerSocket,分别用于表示双向连接的客户端和服务器端。
```
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class HelloWorld {
        public static void main(String[] args) throws InterruptedException {
                Thread server = new Thread(new Server());
                server.start();
                Thread.sleep(1000);

                Thread client = new Thread(new Client());
                client.start();
        }

}

class Server implements Runnable {

        @Override
        public void run() {
                ServerSocket serverSocket = null;
                try {
                        serverSocket = new ServerSocket(8090);
                        while (true) {
                                Socket socket = serverSocket.accept();

                                OutputStream out = socket.getOutputStream();
                                BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(out));
                                writer.write("服务端消息\n");
                                // writer.write("exit服务端消息\n");
                                writer.flush();

                                InputStream in = socket.getInputStream();
                                BufferedReader reader = new BufferedReader(new InputStreamReader(in));
                                String message = reader.readLine();
                                System.out.println(message);


                                reader.close();
                                in.close();
                                writer.close();
                                out.close();
                                socket.close();

                                if (message.substring(0, 4).equalsIgnoreCase("exit")) {
                                        System.out.println("客户端退出，断开连接");
                                        break;
                                }

                        }
                        serverSocket.close();
                } catch (IOException e) {
                        e.printStackTrace();
                }
        }

}

class Client implements Runnable {

        @Override
        public void run() {
                Socket socket = null;
                try {
                        while (true) {
                                socket = new Socket("127.0.0.1",8090);
                                InputStream in = socket.getInputStream();
                                BufferedReader reader = new BufferedReader(new InputStreamReader(in));
                                String message = reader.readLine();
                                System.out.println(message);
                                if (message.substring(0, 4).equalsIgnoreCase("exit")) {
                                        reader.close();
                                        in.close();
                                        System.out.println("服务端退出，断开连接");
                                        break;
                                }

                                OutputStream out = socket.getOutputStream();
                                BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(out));
                                writer.write("客户端消息\n");
                                writer.flush();
                                writer.close();
                                out.close();
                                reader.close();
                                in.close();
                        }
                        socket.close();
                } catch (IOException e) {
                        e.printStackTrace();
                }
        }
}
```

## HTTP 协议网络程序
HTTP是一个属于应用层的面向对象的协议,适用于分布式超媒体信息系统。HTTP协议是基于请求/响应范式的。一个客户机与服务器建立连接后,发送一个请求给服务器,请求的格式为统一资源标识符、协议版本号,后面是MIME信息(包括请求修饰符、客户机信息和可能的内容)。服务器接收到请求后,给予相应的响应信息,其格式为信息的协议版本号、一个成功或错误的代码,后面是MIME信息(包括服务器信息、实体信息和可能的内容)。
```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;
import java.util.List;
import java.util.Map;

public class HelloWorld {
        public static void main(String[] args) {
                try {
                        URL url = new URL("https://dotcorejava.github.io/javaprograming");
                        URLConnection conn = url.openConnection();
                        System.out.printf("ConnectTimeout: %dms\n", conn.getConnectTimeout());
                        System.out.printf("ReadTimeout: %dms\n", conn.getReadTimeout());
                        Map<String, List<String>> headMap = conn.getHeaderFields();
                        for (Map.Entry<String, List<String>> entry : headMap.entrySet()) {
                                System.out.println(entry.getKey() + ":" + entry.getValue());
                        }

                        BufferedReader reader = new BufferedReader(
                                        new InputStreamReader((InputStream) conn.getContent()));
                        String line = "";
                        while ((line = reader.readLine()) != null) {
                                System.out.println(line);
                        }
                } catch (IOException e) {
                        e.printStackTrace();
                }
        }
}
```

## 练习
### 填空题
1. OSI参考模型由7层组成,它们分别是物理层、数据链路层、网络层、传输层、__、__和 __。
2. 与OSI参考模型不同,TCP/IP参考模型只有4层,从下向上依次是网络接口层、__、传输层和 __。
3. 套接字之间的连接过程可以分为三个步骤:__、 __、 __。
4. UDP是一种__的协议,而TCP是一种__的、保证可靠传输的协议。
5. HTTP是一个属于应用层的__协议,适用于分布式超媒体信息系统。

### 选择题
1. 下列哪个类用于在程序之间建立传送数据包的通信连接__。  
A. DatagramPacket  
B. DatagramSocket  
C. Socket  
D. ServerSocket
2. 下列哪个类用于表示一个数据包__。  
A. DatagramPacket  
B. DatagramSocket  
C. Socket  
D. ServerSocket
3. 下列哪个类用于表示TCP客户端套接字。  
A. DatagramPacket  
B. DatagramSocket  
C. Socket  
D. ServerSocket
4. 下列哪个类用于表示TCP服务端套接字。  
A. DatagramPacket  
B. DatagramSocket  
C. Socket  
D. ServerSocket
5. 下列哪个类提供了检查HTTP头的方法__。  
A. URL  
B. URLConnection  
C. Socket  
D. ServerSocket

### 问答题
1. 简述UDP和TCP协议的异同。
2. 简述基于TCP协议下的客户/服务器端套接字的工作原理。

### 编程练习
完成一个基于UDP或TCP协议的网络通信程序。