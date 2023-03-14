## Tomcat

---

[TOC]

#### JavaWeb的概念

##### 什么是JavaWeb

> JavaWeb是指，所有通过Java语言编写可以通过浏览器访问的程序的总称，叫JavaWeb。JavaWeb是基于请求和响应来开发的。

##### 什么是请求

> 请求是指客户端给服务器发送数据，称为请求Request。

##### 什么是响应

> 响应是指服务器给客户端回传数据，称为响应Response。

##### 请求和响应的关系

> 请求和响应是成对出现的，有请求就有响应。

##### Web资源的分类

> Web资源按实现技术和呈现的效果不同，分为静态资源和动态资源。

#### Tomcat目录

bin

专门用来存放Tomcat服务器的可执行程序

conf

专门用来存放Tomcat服务器的配置文件

lib

专门用来存放Tomcat服务器的jar包

logs

专门用来存放Tomcat服务器运行时输出的日志信息

temp

专门用来存放Tomcat服务器运行时产生的临时数据

webapps

专门用来存放部署的Web工程

work

是Tomcat工作时的目录，用来存放Tomcat运行时jsp翻译为Servlet的源码和Session钝化的目录

#### Tomcat的一般操作

##### Tomcat的停止

找到Tomcat的bin目录下的showdown.bat双击，就可以停止Tomcat服务器

##### 修改Tomcat端口号

Tomcat目录下的conf目录，找到server.xml配置文件

<Connection port = "8080">

##### 如何访问Tomcat下的Web工程

###### 第一种

1.将Web工程放入Tomcat目录里的webapps文件中

2.在浏览器输入访问地址格式：http://ip:port/工程名/目录/文件名

###### 第二种

找到Tomcat下的conf目录\Catalina\localhost下，创建如下配置文件

```xml
<!--
	Context 表示一个工程的上下文
	path	表示工程的访问路径:/工程目录
	docBase	表示工程目录位置
-->
<Context path="工程名" docBase="工程存放路径"/>
ip:8080/path/工程文件名
```

#### 动态Web工程目录介绍

src目录存放自己编写的Java代码

web目录专门用来存放web工程的资源文件

WEB-INF目录是一个受服务器保护的目录，浏览器无法直接访问到此目录的内容

lib目录用来存放第三方的jar包

web.xml是整个动态web工程的配置部署描述文件，可以在这些配置很多web工程的组件

