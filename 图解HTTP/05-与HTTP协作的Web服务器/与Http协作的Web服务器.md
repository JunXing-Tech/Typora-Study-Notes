## 第 5 章	与HTTP协作的Web服务器

[toc]

**本文是在 “《图解HTTP》作者：上野宣” 一书的基础上做补充和说明，仅供学习与交流目的**

> 一台 Web 服务器可搭建多个独立域名的 Web 网站，也可作为通信路 径上的中转服务器提升传输效率。

#### 5.1 用单台虚拟主机实现多个域名

> HTTP/1.1 规范允许一台 HTTP 服务器搭建多个 Web 站点。

比如，提供 Web 托管服务（Web Hosting Service）的供应商，可以用一台服务器为多位客户服务，也可以以每位客户持有的域名运行各自不同的网站。这是因为利用了虚拟主机（Virtual Host，又称虚拟服务器）的功能。

即使物理层面只有一台服务器，但只要使用虚拟主机的功能，则可以假想已具有多台服务器。

**如图：**						<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615104854649.png" alt="image-20230615104854649" style="zoom:67%;" />

客户端使用 HTTP 协议访问服务器时，会经常采用类似` www.hackr.jp `这样的主机名和域名。 

在互联网上，域名通过 DNS 服务映射到 IP 地址（域名解析）之后访 问目标网站。可见，当请求发送到服务器时，已经是以 IP 地址形式 访问了。 

所以，如果一台服务器内托管了` www.tricorder.jp` 和` www.hackr.jp` 这 两个域名，当收到请求时就需要弄清楚究竟要访问哪个域名。

**如图：**									<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615105009440.png" alt="image-20230615105009440" style="zoom:67%;" />

若`www.tricirder.jp`和`www.hackr.jp`同时部署在同一个服务器上（相同的IP地址），使用DNS服务器解析域名后，两者的访问IP地址会相同。

在相同的 IP 地址下，由于虚拟主机可以寄存多个不同主机名和域名 的 Web 网站，因此在发送 HTTP 请求时，必须在 Host 首部内完整指定主机名或域名的 URI。

**这一节我觉得讲的不是很明白，下面是讲述在HTTP/1.1规范中，虚拟主机的相关内容（对原文的补充）：**

1. Host 头部字段：HTTP/1.1 规范要求在请求头部中包含 Host 字段。Host 字段指定了客户端请求的目标域名或 IP 地址。例如，`Host: www.example.com`。
2. 根据 Host 字段路由请求：当服务器接收到请求时，它会检查请求头部中的 Host 字段，以确定客户端请求的是哪个域名。根据 Host 字段的值，服务器可以将请求路由到相应的虚拟主机。
3. 多个虚拟主机共享单个 IP 地址：由于 HTTP/1.1 规范支持虚拟主机功能，因此多个域名可以共享单个 IP 地址。这使得在单个服务器上托管多个域名成为可能，而无需为每个域名分配一个唯一的 IP 地址。
4. 虚拟主机配置：服务器管理员需要配置虚拟主机，以便为每个域名指定相应的文档根目录、日志文件位置等。这样，当服务器接收到请求时，它可以将请求路由到正确的虚拟主机。

服务器通过检查 Host 字段来路由请求，并根据配置的虚拟主机为每个域名提供相应的服务。这大大提高了服务器资源的利用率和灵活性。

#### 5.2 通信数据转发程序 ：代理、网关、隧道

HTTP 通信时，除客户端和服务器以外，还有一些用于通信数据转发的应用程序，例如代理、网关和隧道。它们可以配合服务器工作。

这些应用程序和服务器可以将请求转发给通信线路上的下一站服务器，并且能接收从那台服务器发送的响应再转发给客户端。

**代理**

**代理是一种有转发功能的应用程序**，它扮演了位于服务器和客户端“中间人”的角色，接收由客户端发送的请求并转发给服务器，同时也接收服务器返回的响应并转发给客户端。

**网关**

**网关是转发其他服务器通信数据的服务器**，接收从客户端发送来的请求时，它就像自己拥有资源的源服务器一样对请求进行处理。有时客户端可能都不会察觉，自己的通信目标是一个网关。

**隧道**

隧道是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的**应用程序**。

##### 5.2.1 代理

> 代理可以在两者之间进行数据传输、缓存、过滤或修改请求/响应等操作。代理可以实现匿名访问、访问控制、负载均衡等功能。

**如图：**			<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615110636499.png" alt="image-20230615110636499" style="zoom:67%;" />

代理服务器的基本行为就是接收客户端发送的请求后转发给其他服务器。代理不改变请求 URI，会直接发送给前方持有资源的目标服务器。

持有资源实体的服务器被称为源服务器。从源服务器返回的响应经过代理服务器后再传给客户端。

**如下图，每次在通过代理服务器转发请求或响应时，会追加写入Via首部信息**

​					<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615110834947.png" alt="image-20230615110834947" style="zoom:67%;" />

在 HTTP 通信过程中，可级联多台代理服务器。请求和响应的转发会经过数台类似锁链一样连接起来的代理服务器。**转发时，需要附加 Via 首部字段以标记出经过的主机信息**。

**如下图，通过设置组织内部的代理服务器可做到针对特定URI访问的控制**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615111132220.png" alt="image-20230615111132220" style="zoom:67%;" />

使用代理服务器的理由有：利用**缓存技术（稍后讲解）减少网络带宽的流量**，组织内部针对特定网站的访问控制，以获取访问日志为主要目的，等等。

###### 代理的两种基准

代理有多种使用方法，按两种基准分类。一种是是否使用缓存，另一 种是是否会修改报文。

**缓存代理**

代理转发响应时，**缓存代理（Caching Proxy）**会预先将资源的副本 （缓存）保存在代理服务器上。

当代理再次接收到对相同资源的请求时，就可以不从源服务器那里获 取资源，而是将之前缓存的资源作为响应返回。

**透明代理**

转发请求或响应时，不对报文做任何加工的代理类型被称为**透明代理 （Transparent Proxy）**。反之，对报文内容进行加工的代理被称为非透明代理。

##### 5.2.2 网关

> 网关类似于代理，但在不同的网络中起作用。它充当两个或多个独立网络之间的中间服务器。当两个网络使用不同的协议时，网关可以将请求从一个网络转换为另一个网络所需的协议格式。网关通常用于实现不同网络之间的互联、协议转换、安全防火墙等功能。

**如下图，利用网关可以由HTTP请求转化为其他协议通信**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615111639886.png" alt="image-20230615111639886" style="zoom:67%;" />

网关的工作机制和代理十分相似。而网关能使通信线路上的服务器提供非 HTTP 协议服务。

利用网关能提高通信的安全性，因为可以在客户端与网关之间的通信 线路上加密以确保连接的安全。比如，网关可以连接数据库，使用 SQL语句查询数据。另外，在 Web 购物网站上进行信用卡结算时， 网关可以和信用卡结算系统联动。

##### 5.2.3 隧道

> 隧道是通过在两个通信节点之间创建一个加密的通道来传输数据的一种方式。隧道通过封装原始数据包，在传输过程中对数据进行加密和解密，以确保数据的安全性和完整性。隧道可以用于通过不受信任的网络连接安全地传输数据，例如在互联网上建立安全的虚拟专用网络（VPN）连接。

隧道可按要求建立起一条与其他服务器的通信线路，届时使用 SSL等 加密手段进行通信。隧道的目的是确保客户端能与服务器进行安全的 通信。

隧道本身不会去解析 HTTP 请求。也就是说，请求保持原样中转给之 后的服务器。隧道会在通信双方断开连接时结束。

**如下图，通过隧道的传输，可以和远距离的服务器安全通信。隧道本身是透明的，客户端不用在意隧道的存在**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615112744839.png" alt="image-20230615112744839" style="zoom:67%;" />

###### Java代码实现代理、网关、隧道功能整合的简单示例

下述Java代码涉及Java网络编程知识，可仅作了解。

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class SimpleProxyGatewayTunnel {
    public static void main(String[] args) {
        try {
            // 创建代理服务器的 ServerSocket，监听指定端口
            ServerSocket serverSocket = new ServerSocket(8080);
            
            while (true) {
                // 等待客户端连接
                Socket clientSocket = serverSocket.accept();
                
                // 创建与目标服务器的连接
                Socket serverSocket = new Socket("www.example.com", 80);
                
                // 创建线程处理客户端请求和服务器响应的转发
                Thread requestThread = new Thread(() -> {
                    try {
                        InputStream clientInput = clientSocket.getInputStream();
                        OutputStream serverOutput = serverSocket.getOutputStream();
                        
                        // 从客户端读取请求数据并转发给目标服务器
                        byte[] buffer = new byte[4096];
                        int bytesRead;
                        while ((bytesRead = clientInput.read(buffer)) != -1) {
                            serverOutput.write(buffer, 0, bytesRead);
                            serverOutput.flush();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                });
                
                Thread responseThread = new Thread(() -> {
                    try {
                        InputStream serverInput = serverSocket.getInputStream();
                        OutputStream clientOutput = clientSocket.getOutputStream();
                        
                        // 从目标服务器读取响应数据并转发给客户端
                        byte[] buffer = new byte[4096];
                        int bytesRead;
                        while ((bytesRead = serverInput.read(buffer)) != -1) {
                            clientOutput.write(buffer, 0, bytesRead);
                            clientOutput.flush();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                });
                
                // 启动转发线程
                requestThread.start();
                responseThread.start();
                
                // 创建隧道连接
                Thread tunnelThread = new Thread(() -> {
                    try {
                        // 创建与目标服务器的隧道连接
                        Socket tunnelSocket = new Socket("www.example.com", 443);
                        
                        InputStream clientInput = clientSocket.getInputStream();
                        OutputStream tunnelOutput = tunnelSocket.getOutputStream();
                        
                        // 从客户端读取数据并转发给目标服务器的隧道连接
                        byte[] buffer = new byte[4096];
                        int bytesRead;
                        while ((bytesRead = clientInput.read(buffer)) != -1) {
                            tunnelOutput.write(buffer, 0, bytesRead);
                            tunnelOutput.flush();
                        }
                        
                        // 关闭隧道连接
                        tunnelSocket.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                });
                
                // 启动隧道线程
                tunnelThread.start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 5.3 保存资源的缓存

> 缓存是指代理服务器或客户端本地磁盘内保存的资源副本。利用缓存可减少对源服务器的访问，因此也就节省了通信流量和通信时间。

缓存服务器是代理服务器的一种，并归类在缓存代理类型中。换句话说，当代理转发从服务器返回的响应时，代理服务器将会保存一份资源的副本。

**如下图，当缓存服务器存在时，可缓存的文件在转发响应时，复制资源后保存在缓存服务器上**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615113521164.png" alt="image-20230615113521164" style="zoom:67%;" />

**如下图，如请求的资源如果已经被缓存则直接由缓存服务器返回给客户端**

<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230615113627338.png" alt="image-20230615113627338" style="zoom:67%;" />

缓存服务器的优势在于利用缓存可避免多次从源服务器转发资源。因此客户端可就近从缓存服务器上获取资源，而源服务器也不必多次处理相同的请求了。

##### 5.3.1 缓存的有效期限

即便缓存服务器内有缓存，也不能保证每次都会返回对同资源的请求。因为这关系到被缓存资源的有效性问题。 

当遇上源服务器上的资源更新时，如果还是使用不变的缓存，那就会演变成返回更新前的“旧”资源了。 

即使存在缓存，也会因为客户端的要求、缓存的有效期等因素，向源服务器确认资源的有效性。若判断缓存失效，缓存服务器将会再次从源服务器上获取“新”资源。

**如图：**		<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230618083321716.png" alt="image-20230618083321716" style="zoom:67%;" />

##### 5.3.2 客户端的缓存

缓存不仅可以存在于缓存服务器内，还可以存在客户端浏览器中。以 Internet Explorer 程序为例，把客户端缓存称为临时网络文件 （Temporary Internet File）。 

浏览器缓存如果有效，就不必再向服务器请求相同的资源了，可以直 接从本地磁盘内读取。 

另外，和缓存服务器相同的一点是，当判定缓存过期后，会向源服务器确认资源的有效性。若判断浏览器缓存失效，浏览器会再次请求新资源。

**如图：**								<img src="https://cdn.staticaly.com/gh/JunXing-Tech/Image-Host@main/imageHost/image-20230618083622789.png" alt="image-20230618083622789" style="zoom:67%;" />

客户端的缓存可以分为以下几种类型：

1. **页面缓存**：将完整的网页内容保存在客户端的缓存中，当用户再次访问相同的网页时，可以直接从缓存中加载，减少服务器的负载和网络传输。
2. **图片缓存**：将网页中的图片保存在客户端的缓存中，下次再次加载相同图片时可以直接从缓存中获取，减少网络请求和下载时间。
3. **脚本和样式表缓存**：将网页中的脚本和样式表文件保存在客户端的缓存中，下次再次加载相同的脚本和样式表时可以直接从缓存中获取，提高页面加载速度。
4. **数据缓存**：将一些常用的数据保存在客户端的缓存中，例如用户的个人信息、搜索历史等，可以提高用户体验和减少服务器的负载。

客户端的缓存可以通过设置HTTP响应头中的**Cache-Control和Expires字段来控制缓存的行为**，例如设置缓存的过期时间、是否允许缓存等。同时，客户端也可以通过清除缓存或者设置不缓存来管理和控制缓存的内容。

**以下内容了解即可**

> **在 HTTP 出现之前的协议 **
>
> 在 HTTP 普及之前，也就是从互联网的诞生期至今，曾出现过各式 各样的协议。在 HTTP 规范确立之际，制定者们参考了那些协议的 功能。也有某些协议现在已经彻底退出了人们的视线。接下来，我 们会简单介绍一下这些协议。 
>
> **FTP（File Transfer Protocol） **
>
> 传输文件时使用的协议。该协议历史久远，可追溯到 1973 年前 后，比 TCP/IP 协议族的出现还要早。虽然它在 1995 年被 HTTP 的 流量（Traffic）超越，但时至今日，仍被广泛沿用。 
>
> **NNTP（Network News Transfer Protocol） **
>
> 用于 NetNews 电子会议室内传送消息的协议。在 1986 年前后出 现，属于比较古老的一类协议。现在，利用 Web 交换信息已成主 流，所以该协议已经不怎么使用了。 
>
> **Archie **
>
> 搜索 anonymous FTP 公开的文件信息的协议。1990 年前后出现，现 在已经不常使用。
>
> **WAIS（Wide Area Information Servers） **
>
> 以关键词检索多个数据库使用的协议。1991 年前后出现。由于现 在已经被 HTTP 协议替代，也已经不怎么使用了。 
>
> **Gopher **
>
> 查找与互联网连接的计算机内信息的协议。1991 年前后出现，由 于现在已经被 HTTP 协议替代，也已经不怎么使用了。
