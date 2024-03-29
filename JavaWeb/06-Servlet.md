## Servlet

---

[TOC]

#### 什么是Servlet

1. Servlet是JavaEE的规范之一。是一种接口
2. Servlet是JavaWeb三大组件之一。三大组件是：Servlet程序，Filter过滤器、Listener监听器。
3. Servlet是运行在服务器的一个Java小程序，**它可以接受客户端发送过来的请求，并响应数据给客户端**

#### 手动实现Servlet程序

在WEB-INF中的web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <!--
            servlet-name标签 Servlet程序起一个别名（一般是类名）
            servlet-class 是Servlet程序的全类名
        -->
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.Servlet.HelloServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <!--
            servlet-name标签的作用是告诉服务器。当前配置的地址给哪个Servlet程序使用
            url-pattern标签配置访问地址
        -->
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/Hello</url-pattern>
    </servlet-mapping>
</web-app>
```

![image-20230309210542829](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230309210542829.png)

#### Servlet生命周期

1. 执行Servlet构造器方法
2. 执行init初始化方法
3. 执行service方法
4. 执行destroy销毁方法

第一、二步是在第一次访问的时候创建Servlet程序会调用

第三步每次访问都会调用

第四步在Web工程停止的时候调用

#### 请求的分发处理

```java
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        //类型转换
        HttpServletRequest httpServletRequest  = (HttpServletRequest) servletRequest;
        //获取请求方式
        String method = httpServletRequest.getMethod();
        //打印请求
        System.out.println(method);
        //
        if("GET".equals(method)){
            doGet();
        } else if ("POST".equals(method)) {
            doPost();
        }
    }

    public void doPost(){
        //处理POST请求信息
    }
    public void doGet(){
        //处理Get请求信息
    }
```

#### 通过继承HttpServlet实现Servlet程序

一般在实际项目开发中，都是使用继承HttpServlet实现Servlet程序。

##### 编写一个类去继承HttpServlet类

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

1. 根据业务需要重写doGet或doPost方法
2. 到web.xml中的配置Servlet程序的访问地址

#### Servlet继承体系

![image-20230311100257866](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230311100257866.png)

#### ServletConfig类使用介绍

* ServletConfig类是Servlet程序的配置信息类

* Servlet程序和ServletConfig对象都是由Tomcat负责创建，由程序员负责使用

* Servlet程序默认是第一次访问的时候创建，ServletConfig是每个Servlet程序创建时，就创建一个对应的ServletConfig对象
* 其他class程序也可使用ServletConfig`ServletCongif servletConfig = getServletConfig();`其他程序不可获取其中的值
* 如果你要重写init()方法，要在方法第一行写上`super.init(config);`

```java
@Override
    public void init(ServletConfig servletConfig) throws ServletException {
        //可以获取Servlet程序的别名servlet-name的值
        System.out.println(servletConfig.getServletName());
        //获取初始化init-param
        System.out.println(servletConfig.getInitParameter("username"));
        System.out.println(servletConfig.getInitParameter("url"));
        //获取ServletContext对象
        System.out.println(servletConfig.getServletContext());
    }
```

```xml
<init-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
</init-param>
```

#### ServletContext类

##### 什么是ServletContext

1. 是一个接口，表示Servlet上下文对象
2. 一个Web工程，只有一个ServletContext对象实例
3. ServletContext对象是一个域对象
4. ServletContext是在web工程部署启动的时候创建。在web工程停止的时候销毁。

域对象

> 域对象是可以像Map一样存取数据的对象，叫做域对象

这里的域指的是存取数据的操作范围为整个web工程

|        | 存数据         | 取数据         | 删除数据          |
| ------ | -------------- | -------------- | ----------------- |
| Map    | put()          | get()          | remove()          |
| 域对象 | setAttribute() | getAttribute() | removeAttribute() |

##### ServletContext类的四个作用

1.获取web.xml中配置的上下文参数context-param

2.获取当前的工程路径，格式：/工程名

3.获取工程部署后在服务器硬盘上的绝对路径

4.像Map一样存取数据

```xml
<!--context-param是上下文参数（整个Web工程皆可用）-->
<context-param>
	<param-name>username</para-name>
	<param-value>context</param-value>
</context-param>
```

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doGet(req, resp);
        //super.doPost(req, resp);
        //1.获取web.xml中配置的上下文参数context-param
        ServletContext context = getServletConfig().getServletContext();

        String username = context.getInitParameter("username_");
        System.out.println("123456789");
        System.out.println(username);
        //2.获取当前的工程路径，格式：/工程名
        System.out.println(context.getContextPath());
        //3.获取工程部署后在服务器硬盘上的绝对路径
        //" / " 被服务器解析地址为 http://ip:port:工程名
        //映射到IDEA代码的web目录
        System.out.println(context.getRealPath("/"));
    }
```

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doGet(req, resp);
        ServletContext servletContext = getServletContext();
        servletContext.setAttribute("key", "value");
        System.out.println("key 的值 = " + servletContext.getAttribute("key"));
    }
```

可在其他类中使用以下方法，使用以上的`key`值，总的来说**ServletContext对象只能有一个且整个Web工程都可用**

```java

ServletContext context = getServletContext();
        System.out.println(context.getAttribute("key"));
```

#### 什么是HTTP协议？

> 是指客户端和服务器之间通信时，发送的数据，需要遵守的规则，称为HTTP协议。HTTP协议中的数据又叫报文。

#### 请求的HTTP协议格式

> 客户端给服务器发送数据叫请求。服务器给客户端回传数据叫响应。
>
> 请求分为GET请求和POST请求两种。

##### GET请求

1. 请求行
   1. 请求的方式
   2. 请求的资源路径
   3. 请求的协议的版本号
2. 请求头
   1. key:value

![image-20230313172706579](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230313172706579.png)

##### POST请求

![image-20230313174815911](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230313174815911.png)

###### 常用请求头说明

如上二图

#### GET请求与POST请求的区分

##### GET请求

* from标签 `method = "get"`
* a标签
* link标签引入css
* Script标签引入JavaScript文件
* img标签引入图片
* iframe引入html页面
* 在浏览器地址栏中输入地址后回车

##### POST请求

* from标签 `method="post"`

#### 响应的HTTP协议格式

1. 响应行
   1. 响应的协议和版本号
   2. 响应状态码
   3. 响应状态描述符
2. 响应头
   1. key:value	不同的响应头，有不同含义
3. 响应体
   1. 回传给客户端的数据

![image-20230313181002114](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230313181002114.png)

#### 常用的响应码说明

200	表示请求成功

302	表示请求重定向

404	表示请求服务器已经收到了

500	表示服务器已经收到请求，但是服务器内部错误（代码错误）

##### MIME类型说明

![image-20230313181953268](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230313181953268.png)

#### HttpServletRequest类

作用

每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议信息解析好封装到Request对象中。然后传递到service方法（doGet和doPost）中给我们使用。我们可以通过HttpServletRequest对象，获取到所有请求的信息。

HttpServletRequest类常用方法

![image-20230314150915686](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230314150915686.png)

```java
public class RequestAPIServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(req.getRequestURI());

        System.out.println(req.getRequestURL());

        //可得到访问客户端的访问者的IP地址
        System.out.println(req.getRemoteHost());

        System.out.println(req.getHeader("User-Agent"));

        System.out.println(req.getMethod());
    }
}

```

#### 获取请求的参数值

```java
String username = req.getParameter("username");
String password = req.getParameter("password");
String[] hobby = req.getParameterValues(Arrays.asList(hobby));
```

#### 解决post请求中文乱码问题

```java
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {//可解决post请求中文乱码问题
        //在获取请求参数前调用才有效
        req.setCharacterEncoding("UTF-8");
    }
```

#### 请求的转发

> 请求转发是指，服务器收到请求后，从一个资源跳转到另一个资源的操作叫请求转发

![image-20230315100846945](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230315100846945.png)

Servlet1程序

```java
package com.Servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * JunXing
 * 2023/3/15 8:59
 * IntelliJ IDEA
 */
public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1.获取请求的参数
        String username = req.getParameter("username");
        System.out.println("在Servlet1（柜台1）中查看参数（材料）" + username);

        //2.给材料盖一个章，并传递给Servlet2柜台查看
        req.setAttribute("key1", "柜台1的章");

        //3.问路：Servlet2柜台怎么走
        //请求转发必须要以斜杆打头 / 表示地址为：http://ip:port/工程名/
        //映射到IDEA代码的web目录
        //RequestDispatcher requestDispatcher = req.getRequestDispatcher("/s2");

        //请求转发可到WEB-INF目录下访问资源
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/WEB-INF/form.html");



        //走向Servlet2柜台2
        requestDispatcher.forward(req, resp);
    }
}
```

Servlet2程序

```java
package com.Servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * JunXing
 * 2023/3/15 8:59
 * IntelliJ IDEA
 */
public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //获取请求的参数（办事材料）查看
        String username = req.getParameter("username");
        System.out.println("在Servlet2（柜台2）中查看参数（材料）" + username);

        //查看柜台1是否有章
        Object key1 = req.getAttribute("key1");
        System.out.println("柜台1是否有章" + key1);

        //处理自己的业务
        System.out.println("Servlet2处理自己的业务");
    }
}
```

#### base标签的作用

![image-20230315103128921](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230315103128921.png)

```html
<!--
	base 标签设置页面相对路径工作时参照的地址
	href 属性就是参数的地址值
	资源路径中的资源名可省略，但资源名前的斜杆不可省略
-->
<base href="http://localhost:8080/工程名/资源路径">
```

Web中的相对路径和绝对路径

<img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230315103308504.png" alt="image-20230315103308504" style="zoom:80%;" />

Web中 / 斜杆的不同意义

 <img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230315103729261.png" alt="image-20230315103729261" style="zoom:80%;" />

#### HttpServletResponse类

##### HttpServletResponse类的作用

> HttpServletResponse类和HttpServletRequest类一样。每次请求进来时，Tomcat服务器都会创建一个Response对象传递给Servlet程序去使用。HttpServletRequest表示请求过来的信息。HttpServletResponse表示所有响应的信息，我们如果需要设置返回给客户端的信息，都可以通过HttpServletResponse对象来进行设置。

##### 两个输出流的说明

字节流	`getOutputStream();`

* 常用于下载（传递二进制数据）

字符流	`getWriter();`

* 常用于回传字符串（常用）

两个流同时只能使用一个。使用了字节流，就不能再使用字符流，反之亦然，否则就会报错。

```java
public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
        resp.getWriter();
        resp.getOutputStream();
        /**
         * 会报错
         * java.lang.IllegalStateException:getOutputStream() has already been called for this response
         */
    }
}
```

#### 如何往客户端回传数据

```java
public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        PrintWriter writer = resp.getWriter();
        writer.write("Response Content !");
    }
}
```

#### 解决响应的中文乱码

##### 法一

```java
public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //服务器默认字符集为：ISO-8859-1
        System.out.println(resp.getCharacterEncoding());

        //设置服务器字符集为UTF-8
        resp.setCharacterEncoding("UTF-8");

        //通过响应头，设置浏览器也使用UTF-8字符集
        resp.setHeader("Content-Type", "text/html; charset = UTF-8");

        PrintWriter writer = resp.getWriter();
        writer.write("Response Content !");
        writer.write("中文字符测试");
    }
}
```

##### 法二（推荐使用）

```java
public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
        //它会同时设置服务器和客户端都使用UTF-8字符集，还设置响应头
        //此方法一定要在获取流对象之前调用才有效
        resp.setContentType("text/html; charset = UTF-8");

        //获取流对象
        PrintWriter writer = resp.getWriter();

        writer.write("Response Content !");
        writer.write("中文字符测试");
    }
}
```

#### 请求重定向

##### 方案一

<img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230315141535476.png" alt="image-20230315141535476" style="zoom:80%;" />

```java
public class Response1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        System.out.println("重定向输出测试");

        //设置响应状态码302，表示重定向（已搬迁）
        resp.setStatus(302);

        resp.setHeader("Location", "http://localhost:8080/JavaWeb_war_exploded/res2");
    }
}
```

```java
public class Response2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("response2 result!");
    }
}
```

##### 方案二（推荐使用）

```java
public class Response1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //方案二
        resp.sendRedirect("http://localhost:8080/JavaWeb_war_exploded/res2");
    }

}
```

