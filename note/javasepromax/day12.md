

# 网络编程

* 网络编程可以让程序和网络上的其他设备中的程序进行数据交互

# 网络通信三要素

## 三要素概述，要素一：IP地址

* IP地址：设备在网络中的地址，是唯一的标识
* 端口：应用程序在设备中唯一的标识
* 协议：数据在网络中传输的规则，常见的协议有UDP协议和TCP协议

* ![image-20220903104041970](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903104041970.png)

* ![image-20220903104137853](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903104137853.png)

## IP地址操作类-InetAddress

* ![image-20220903105231600](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903105231600.png)

* ```java
  			  //获取本机地址
          InetAddress inetAddress = InetAddress.getLocalHost();
          System.out.println(inetAddress);
          System.out.println(inetAddress.getHostName());
          System.out.println(inetAddress.getHostAddress());
     	  //获取域名IP对象
          InetAddress inetAddress1 = InetAddress.getByName("www.baidu.com");
          System.out.println(inetAddress1.getHostName());
          System.out.println(inetAddress1.getHostAddress());
      
          //获取公网IP对象
          InetAddress inetAddress2 = InetAddress.getByName("14.215.177.39");
          System.out.println(inetAddress2.getHostName());
          System.out.println(inetAddress2.getHostAddress());
      
          //判断是否能通：ping 2秒之前测试是否可行，防止系统一直等待。
          System.out.println(inetAddress1.isReachable(2000));
   ```

## 要素二：端口号

* ![image-20220903111922937](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903111922937.png)

* IP地址查找到主机后，需要根据端口号（唯一标识正在计算机设备上运行的进程）查找对应的程序。每一个端口号只能对应一个程序（非动态端口），像酒店和房间

## 要素三：协议

* ![image-20220903113645699](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903113645699.png)

* ![image-20220903113814164](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903113814164.png)

# UDP通信

## UDP通信：快速入门

* ![image-20220903114125648](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903114125648.png)

* ```java
  public class ClientDemo {
      public static void main(String[] args) throws Exception {
          //一发一收，客户端
          System.out.println("=============客户端启动================");
          //创建数据包发送端口，端口号随机，(人)
          DatagramSocket datagramSocket = new DatagramSocket();
  
          //创建数据包(韭菜盘子)
          //byte buf[], int length,
          //                          InetAddress address, int port
          byte[] bytes = "发送了一颗小韭菜，请查收".getBytes();
          DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length, InetAddress.getLocalHost(), 8888);//韭菜
  
          //发送数据(人)
          datagramSocket.send(datagramPacket);//将盘中的数据扔出去
          datagramSocket.close();
          // 。
      }
  }
  
  ```

* ```java
  public class ServerDemo1 {
      public static void main(String[] args) throws Exception {
          System.out.println("=============服务端启动================");
          DatagramSocket datagramSocket = new DatagramSocket(8888);
  
          byte[] bytes = new byte[1024 * 6];
          DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length);
  
          datagramSocket.receive(datagramPacket);
          int len = datagramPacket.getLength();
          String s = new String(bytes, 0, len);
  
          SocketAddress socketAddress = datagramPacket.getSocketAddress();
          System.out.println("对方的ip" + datagramPacket.getAddress());
          System.out.println("对方的端口" + datagramPacket.getPort());
          System.out.println(socketAddress);
  
          System.out.println(s);
  
      }
  }
  ```

* ![image-20220903123649204](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903123649204.png)

## UDP通信：多发多收

* ```java
  System.out.println("=============客户端启动================");
          //创建数据包发送端口，端口号随机，(人)
          DatagramSocket datagramSocket = new DatagramSocket();
          Scanner sc = new Scanner(System.in);
  
          //创建数据包(韭菜盘子)
          //byte buf[], int length,
          //                          InetAddress address, int port
          while (true) {//通过不断的向盘子里面加入不同的韭菜,来达到多次发的效果。并且可以不同端口并发执行（编辑器拉里进行适当调整）
              System.out.println("请发送弹幕:");
              String common = sc.nextLine();
  
              if (common.equals("exit")){
                  System.out.println("程序结束");
                  datagramSocket.close();
                  System.exit(0);
              }
  
              byte[] bytes = common.getBytes();
              DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length, InetAddress.getLocalHost(), 8888);//韭菜
  
              //发送数据(人)
              datagramSocket.send(datagramPacket);//将盘中的数据扔出去
          }
  ```

* ```java
   System.out.println("=============服务端启动================");
          DatagramSocket datagramSocket = new DatagramSocket(8888);
    
          byte[] bytes = new byte[1024 * 6];//客户端最多发6kb数据！
          DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length);
    
          while (true) {//不断的收数据，如果去掉while只能接收一个类别韭菜，因为不能重复的接收
              datagramSocket.receive(datagramPacket);
              int len = datagramPacket.getLength();
              String s = new String(bytes, 0, len);
    
  //            SocketAddress socketAddress = datagramPacket.getSocketAddress();
  //            System.out.println("对方的ip" + datagramPacket.getAddress());
  //            System.out.println("对方的端口" + datagramPacket.getPort());
  //            System.out.println(socketAddress);
  
              System.out.println("IP:" + datagramPacket.getAddress() + "端口号为:" + datagramPacket.getPort()
                      + "发来了: " + s );
          }
  ```

* ![image-20220903193806968](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903193806968.png)

* ![image-20220903193846934](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903193846934.png)

## UDP通信-广播，组播

* ![image-20220903202719471](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903202719471.png)

* ```java
  System.out.println("=============客户端启动================"); 
  
  DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length, InetAddress.getByName("224.0.1.1"), 9999);//韭菜
  ```

* ```java
   System.out.println("=============服务端启动================");
  //        DatagramSocket datagramSocket = new DatagramSocket(9999);
          MulticastSocket datagramSocket = new MulticastSocket(9999);
          datagramSocket.joinGroup(InetAddress.getByName("224.0.1.1"));//注意查看所需的是啥参数！
  ```

# TCP通信-快速入门

## 一发一收

* ![image-20220903203444743](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220903203444743.png)

* ```java
   public static void main(String[] args) throws Exception {
          System.out.println("=============客户端启动================");
    
          //创建与服务端连接的管道
          Socket s = new Socket("127.0.0.1", 8888);
    
          //创建字节输出流管道输出消息数据(输出最好使用打印流)
          OutputStream os = s.getOutputStream();
          PrintStream ps = new PrintStream(os);
    
          //向服务端输出数据
          ps.println("这里是客户端，同时发送了一个消息：约吗？");//换行和不换行有区别
          ps.flush();//刷新缓冲，更新数据
    
          //不需要关闭管道，否则会出现数据传输中途中断的行为
      }
  ```

* ```java
   System.out.println("=============服务端启动================");
          try {
              //创建服务器
              ServerSocket ss = new ServerSocket(8888);
    
              //等待接收客户端socket连接请求，建立socket通信管道
              Socket s = ss.accept();
    
              //得到字节输入流，但要转换为缓冲字符输入流
              InputStream inputStream = s.getInputStream();
              BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
    
              //输出存储的字符，按照行读取信息
              String s1;
              if ((s1 = bufferedReader.readLine()) != null){//只接收一次
                  System.out.println(s.getRemoteSocketAddress() + "发来: " + s1);
              }
    
          } catch (IOException e) {
              e.printStackTrace();
          }
  ```

* ![image-20220904101143644](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904101143644.png)

* ![image-20220904101237106](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904101237106.png)

# TCP通信-多发多收消息

* ```java
  Scanner scanner = new Scanner(System.in);
          while (true) {
              System.out.println("请说： ");
              ps.println(scanner.nextLine());//换行和不换行有区别
              ps.flush();//刷新缓冲，更新数据
          }
  ```

* ```java
  String s1;
  while ((s1 = bufferedReader.readLine()) != null){//不能同时接收多个客户端的发送请求，因为会一直等待第一个客户端发送的请求。
      System.out.println(s.getRemoteSocketAddress() + "发来: " + s1);
  }
  ```

# TCP通信-同时接受多个客户端消息

* 创建多线程

* ```java
  public class ServerReaderThread extends Thread{
      private Socket socket;
  
      public ServerReaderThread(Socket socket){
          this.socket = socket;
      }
  
      @Override
      public void run() {
          //得到字节输入流，但要转换为缓冲字符输入流
          try {
          InputStream inputStream = socket.getInputStream();
          BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
  
          //输出存储的字符
  
              String s1;
              while ((s1 = bufferedReader.readLine()) != null) {//
                  System.out.println(socket.getRemoteSocketAddress() + "发来: " + s1);
              }
          } catch (Exception e) {
              System.out.println(socket.getRemoteSocketAddress() + "下线了");}
      }
  }
  ```

* ```java
  while (true) {
      Socket s = ss.accept();
      System.out.println(s.getRemoteSocketAddress() + "上线了");
      new ServerReaderThread(s).start();//主线程接收得到的管道，分发给子线程运行！
                  }
  ```

* ![image-20220904105955192](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904105955192.png)

# TCP通信-使用线程池优化

* ![image-20220904110237537](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904110237537.png)

* ```java
  private static ExecutorService pool = new ThreadPoolExecutor(2, 3, 2, TimeUnit.SECONDS,new ArrayBlockingQueue<>(1), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());//创建线程池进行优化
  
      public static void main(String[] args) {
          System.out.println("=============服务端启动================");
              //创建服务器
          try {
              ServerSocket ss = new ServerSocket(7777);
  
              //与客户端管道相连,返回管道对象
              while (true) {
                  Socket s = ss.accept();
                  System.out.println(s.getRemoteSocketAddress() + "上线了");
                  pool.execute(new ServerReaderPool(s));//放在堵塞队列中等待线程执行！
                  }
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  ```

* ![image-20220904113638918](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904113638918.png)

# TCP通信-即时通信

* ```java
  public class SocketClientDemo1 {
      public static void main(String[] args) throws Exception {
          System.out.println("=============客户端启动================");
  
          //创建与服务端连接的管道
          Socket s = new Socket("127.0.0.1", 7777);
  
          new ClientReceive(s).start();//创建线程接收群聊消息！
  
          //创建字节输出流管道输出消息数据(输出最好使用打印流)
          OutputStream os = s.getOutputStream();
          PrintStream ps = new PrintStream(os);
  
          //向服务端输出数据
          Scanner scanner = new Scanner(System.in);
          while (true) {
              System.out.println("请说： ");
              ps.println(scanner.nextLine());//换行和不换行有区别
              ps.flush();//刷新缓冲，更新数据
          }
  
          //不需要关闭管道，否则会出现数据传输中途中断的行为
      }
  }
  ```

* ```java
  while (true) {
                  Socket s = ss.accept();
                  socketList.add(s);//存储Socket对象，便于得到群发的管道
                  System.out.println(s.getRemoteSocketAddress() + "上线了");
                  pool.execute(new ServerReaderPool(s));
                  }
  ```

* ```java
  String s1;
  while ((s1 = bufferedReader.readLine()) != null) {//
      System.out.println(s1);
      SendAll(s1);//接收到消息，群发（独立功能创建方法）
  }
  private void SendAll(String s1) throws Exception {
          for (Socket socket : SocketServer.socketList) {
              PrintStream ps = new PrintStream(socket.getOutputStream());
              ps.println(s1);
              ps.flush();
          }
      }
  ```


* ![image-20220904183820728](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904183820728.png)

# TCP通信实战-模拟BS系统（之前都是cs系统）

* ![image-20220904185238685](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\image-20220904185238685.png)

* ```java
   // 1.注册端口
  ServerSocket ss = new ServerSocket(8080);
  // 2.创建一个循环接收多个客户端的请求。
  while(true){
      Socket socket = ss.accept();//与浏览器相互连接
      // 3.交给一个独立的线程来处理！
      pool.execute(new ServerReaderRunnable(socket));
  ```

* ```java
   // 浏览器 已经与本线程建立了Socket管道
              // 响应消息给浏览器显示
              PrintStream ps = new PrintStream(socket.getOutputStream());
              // 必须响应HTTP协议格式数据，否则浏览器不认识消息
              ps.println("HTTP/1.1 200 OK"); // 协议类型和版本 响应成功的消息！
              ps.println("Content-Type:text/html;charset=UTF-8"); // 响应的数据类型：文本/网页
  
              ps.println(); // 必须发送一个空行
  
              // 才可以响应数据回去给浏览器
              ps.println("<span style='color:red;font-size:90px'>《最牛的149期》 </span>");
              ps.close();
  ```

