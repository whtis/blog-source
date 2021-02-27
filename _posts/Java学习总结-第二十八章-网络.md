title: Java学习总结-第二十八章 网络
date: 2016-04-16 21:43:48
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
`基于套接字的通信`可以使程序通过指定的套接字进行通信。`套接字（socket）`是两个主机之间逻辑连接的端点，可以用于发送和接收数据。
Java支持流套接字和数据报套接字。`流套接字（stream socket）`使用传输控制协议（Transmission Control Protocol，TCP）进行数据传输，而`数据报套接字（datagram socket）`使用的是用户数据报协议（User Datagram Protocol，UDP）。

### 客户/服务器计算模式
- 服务器和客户一旦建立连接，客户和服务器就可以通过套接字进行通信。
- 创建`服务器套接字（server socket）`，并把它附加到一个端口上，服务器从端口监听连接。端口标识套接字上的TCP服务，端口号的范围从0到65536，但是0到1024号是为特权服务保留的端口。
	```java
	ServerSocket serverSocket = new ServerSocket(port);
	//监听
	Socket socket = serverSocket.accept();
	```
  **如果企图在已经使用的端口上创建服务器套接字，将会引起`java.net.BindException`异常。**
- 服务器的监听语句会一直等待，直到一个客户与服务器套接字建立连接。
  ```java
  Socket socket = new Socket(serverName,port);
  ```
  serverName是服务器的域名或IP地址。
- 通过得到socket的输入输出流，接可以进行数据传输了：
  ```java
  InputStream input = socket.getInputStream();
  OutputStream output = socket.getOutputStream();
  ```
  **建议使用二进制I/O在服务器和客户间进行数据传输，以便提高效率。**

### 网络地址类InetAddress
- 可以使用类InetAddress来求得客户的主机名和IP地址。
  ```java
  InetAddress inetAddress = socket.getInetAddress();
  String hostname = inetAddress.getHostName();
  String ip = inetAddress.getHostAddress();
  ```
- 可以使用静态方法getByName通过主机名或IP地址创建InetAddress的一个实例。
  ```java
  InetAddress address = InetAddress.getByName("www.google.com");
  ```

### 多客户服务
- 可以使用线程处理服务器上多个客户的同步问题。
  ```java
  while(true) {
  	Socket socket = serverSocket.accept();
  	Thread thread = new ThreadClass(socket);
  	thread.start();
  }
  ```
  服务器套接字可以有多个连接。while循环的每次迭代创建一个新的连接。

### 发送和接收对象
- 可以在套接字流上使用ObjectInputStream和ObjectOutputStream接收和发送对象。为了能够进行传输，这些对象必须实现`Serializable`接口。

### 从Web服务器上读取文件
- 可以创建一个`java.net.URL`对象，然后打开一个输入流：
  ```java
  URL url = new URL("www.whtis.com");
  InputStream inputStream = url.openStream();
  ```

### JEditorPanel类
- Swing提供了一个名叫javax.swing.JEditorPanel的GUI组件，它能够自动地显示普通文本文件、HTML文件和RTF文件。 
- 要显示文件的内容，可以使用setPage（URL）方法：
  ```java
  public void setPage(URL url) throws IOException
  ```
- 当单击编辑窗格中的一个超链接时，JEditorPanel产生`javax.swing.event.HyperLink Event`事件，通过该事件，我们可以得到超链接的URL，并使用setPage(url)方法显示它。

### 数据报套接字
- 我们编写的有些网络通信不要求TCP提供可靠的、点对点的通道，这种情况下，数据报通信效率更高。
- 数据是用`分组（packet，或称为包）`进行传输的。数据报套接字使用`用户数据报协议（User Datagram Protocol，UDP）`，该协议不能保证分组不会丢失、或者不会重复接收、或者接收的顺序与发送的顺序不同。
- `数据报（datagram）`是独立的，包含自身网络上的传输信息，它的到达、到达的时间和内容都没有保证。

#### DatagramPacket类和DatagramSocket类
- DatagramPacket表示数据报的分组。要为来自客户的传送创建DatagramPacket对象，可以使用构造方法`DatagramPacket(byte[] buf,int length,InetAddress host,int prot)`。要创建其他所有的DatagramPacket对象，使用构造方法`DatagramPacket(byte[] buf,int length)`。一旦创建了一个数据报分组，就可以使用getData()方法和setData()方法获取和设置分组中的数据。
- 数据报套接字类DatagramSocket表示发送和接收数据报分组的套接字。数据报套接字是分组传输服务的发送和接收点。每个在套接字上发送和接收的分组都是独立编址和路由的。
  + 要创建服务器上的数据报套接字，使用构造方法DatagramSocket(int port),它将套接字绑定到本地主机指定的端口上。
  + 要创建客户上的数据报套接字，使用构造方法DatagramSocket()，它将套接字绑定到本地主机任意一个可用的端口上。

---

## 复习小结
- 流套接字的端口号和数据报套接字的端口号是互不相关的。流套接字和数据报套接字能够同时使用同一个端口号。
- 数据报套接字使用`send(DatagramSocket)`发送分组，使用`receive(DatagramSocket)`接收分组。

---

## 编程练习
- 习题28.1 28.3 28.6 28.8 28.9 28.10源代码见我的Github： [chapter28](https://github.com/whtis/Java-Exercises/tree/master/chapter28/src)
- 习题28.6 
  + 服务器端的输入输出流应该和客户端的对应。
    * 客户端：
    ```java
    ObjectInputStream ois = new ObjectInputStream(socket.getInputStream());
    ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream());
    ```
    * 相应的，服务器端的顺序应该为：
    ```java
    ObjectOutputStream oos = new ObjectOutputStream(socket.getOutputStream());
    ObjectInputStream ois = new ObjectInputStream(socket.getInputStream());
    ```
  + 如何判断ObjectInputStream类中readObject()方法是否到达末尾:
    * readObject()方法读取数据流到末尾时会抛出`java.io.EOFException`异常，可以捕获到这个异常并由此判断对象数据读取完成。
    ```java
    try {
            File file = new File("Address.dat");
            if (!file.exists()) {
                return;
            }
            ObjectInputStream InFromFile = new ObjectInputStream(new FileInputStream(file));
            while (true) {
                lists.add(InFromFile.readObject());
            }
        } catch (EOFException e) {
            e.printStackTrace();
            jTextArea.append("Data read completed!");
        }
    ```
    * 可以在数据流中添加标识，当读取到标识后，则关闭输出流。例如：可以在写入数据完成后附加`null`作为标识。
  + 异常`java.io.StreamCorruptedException: invalid type code: AC`的出现原因及解决方案
  	* 原因：新建一个ObjecOutputStream类后，在第一次写入时，会附加一个头文件信息，随后的数据不会附加头文件。新建一个ObjectInputStream，在第一次读取时会寻找头文件，之后读取时就不会寻找头文件直接读取数据。如果写入数据时每次都附加头文件，而读取时仅第一次读取头文件，就会抛出上述异常。
  	* 解决方案：写文件和读文件要统一，写文件时不能每次都附带头文件，也就是用一个输出流写数据，不要每次写入对象都新建一个输出流。

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>