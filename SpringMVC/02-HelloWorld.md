# HelloWorld

[toc]

#### 1、开发环境

IDE：IDEA、构建工具：maven、服务器：tomcat、Spring相应版本

#### 2、创建maven工程

##### 1.添加web模块

##### 2.打包方式：war

##### 3.引入依赖

```xml
<dependencies>
    <!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>

    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>

    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>

    <!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>
</dependencies>
```

==注==：由于 Maven 的传递性，我们不必将所有需要的包全部配置依赖，而是配置最顶端的依赖，其他靠传递性导入。

#### 3、配置web.xml

注册SpringMVC的==前端控制器DispatcherServlet==有两种方式

##### 1.默认配置方式

此配置作用下，SpringMVC的配置文件默认位于WEB-INF下，默认名称为\<servlet-name>-servlet.xml，例如，以下配置所对应SpringMVC的配置文件位于WEB-INF下，文件名为springMVC-servlet.xml。

默认配置方式的缺点有:不够灵活、不够安全、不够明确、不够统一。

```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
        设置springMVC的核心控制器所能处理的请求的请求路径
        /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
        但是/不能匹配.jsp请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

###### 关于`<url-pattern>`标签

1. 在配置web.xml时，<url-pattern>标签应该填入“/”，表示将所有的请求都交给DispatcherServlet处理。这是因为SpringMVC的核心思想是基于URL映射，即将URL与Controller中的方法进行映射，由Controller来处理具体的业务逻辑。因此，所有的请求都需要经过DispatcherServlet进行分发，由它来确定具体的Controller来处理请求。

2. 也可以填入“/*”，但是这样会导致所有的请求都被DispatcherServlet处理，包括静态资源的请求，如图片、CSS、JavaScript等。这会导致DispatcherServlet的负载增加，同时也会影响系统性能。
3. \<url-pattern>标签中使用/和/*的区别：`/`所匹配的请求可以是/login或.html或.js或.css方式的请求路径，但是`/`不能匹配.jsp请求路径的请求。因此就可以避免在访问jsp页面时，该请求被DispatcherServlet处理，从而找不到相应的页面。`/*`则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用`/*`的写法。

##### 2.扩展配置方式

可通过init-param标签设置SpringMVC配置文件的位置和名称，通过load-on-startup标签设置SpringMVC前端控制器DispatcherServlet的初始化时间

```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
    <init-param>
        <!-- contextConfigLocation为固定值 -->
        <param-name>contextConfigLocation</param-name>
        <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources -->
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!-- 
 		作为框架的核心组件，在启动过程中有大量的初始化操作要做
		而这些操作放在第一次请求时才执行会严重影响访问速度
		因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
	-->
    <!--将前端控制器DispatcherServlet的初始化时间提前到服务器启动时-->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
        设置springMVC的核心控制器所能处理的请求的请求路径
        /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
        但是/不能匹配.jsp请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

###### 关于<load-on-startup>标签的作用与值

1. <load-on-startup>标签用来指定Servlet在Web应用启动时就加载，而不是在第一次请求时才加载。它的取值可以是整数，表示Servlet的加载顺序。数字越小，表示越先加载，值为0表示最先加载。
2. 在Spring MVC中，通常使用<load-on-startup>标签来配置DispatcherServlet的启动顺序。由于DispatcherServlet是整个Spring MVC框架的核心，因此需要在Web应用启动时就加载它，以确保整个框架的正常运行。
3. 例如，配置DispatcherServlet的<load-on-startup>标签为1，表示在Web应用启动时就加载DispatcherServlet，并且它的加载顺序优先于其他的Servlet。这样可以确保DispatcherServlet在整个Web应用中的优先级最高，避免出现由于Servlet启动顺序问题而导致的异常情况。

#### 4、创建请求控制器

由于前端控制器对浏览器发送的请求进行了统一的处理，但是具体的请求有不同的处理过程，因此需要创建处理具体请求的类，即请求控制器。**请求控制器中每一个处理请求的方法成为控制器方法。**

因为SpringMVC的控制器由一个**POJO（普通的Java类）**担任，因此需要通过@Controller注解将其标识为一个控制层组件，交给Spring的IoC容器管理，此时SpringMVC才能够识别控制器的存在。

> “Spring容器会自动将该类实例化”指的是，当一个类被标记为Spring框架中的某个注解（如@Controller、@Service、@Repository等）时，Spring容器会负责创建该类的实例，并将其纳入Spring容器的管理范围之内。在需要使用该类的地方，可以直接从Spring容器中获取该类的实例，而不用手动创建对象。具体来说，Spring容器会根据类上的注解和其他配置信息，使用Java反射机制创建该类的实例，并自动注入该类所依赖的其他Bean。这样，就可以在整个应用程序中共享同一个实例，从而实现了对象的复用和管理。

```java
@Controller
public class HelloController {
    ...
}
```

#### 5、创建springMVC的配置文件

==注意==：springMVC.xml中的名称空间一定要注意！！！

在springMVC .xml中主要分为两步：1.自动扫描包 2.配置Thymeleaf视图解析器

```xml
<!-- 自动扫描包 -->
<context:component-scan base-package="com.mvc.controller"/>

<!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
    
                    <!-- 视图前缀 -->
                    <property name="prefix" value="/WEB-INF/templates/"/>
    
                    <!-- 视图后缀 -->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8" />
                </bean>
            </property>
        </bean>
    </property>
</bean>

<!-- 
   处理静态资源，例如html、js、css、jpg
  若只设置该标签，则只能访问静态资源，其他请求则无法访问
  此时必须设置<mvc:annotation-driven/>解决问题
 -->
<mvc:default-servlet-handler/>

<!-- 开启mvc注解驱动 -->
<mvc:annotation-driven>
    <mvc:message-converters>
        <!-- 处理响应中文内容乱码 -->
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="defaultCharset" value="UTF-8" />
            <property name="supportedMediaTypes">
                <list>
                    <value>text/html</value>
                    <value>application/json</value>
                </list>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

#### 6、测试HelloWorld

##### 1.实现对首页的访问

```java
// @RequestMapping注解：处理请求和控制器方法之间的映射关系
// @RequestMapping注解的value属性可以通过请求地址匹配请求，/表示的当前工程的上下文路径
// localhost:8080/springMVC/
@RequestMapping("/")
public String index() {
    //设置视图名称
    return "index";
}
```

##### 2.通过超链接跳转到指定页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <h1>首页</h1>
    <!--在Thymeleaf中，使用th:href="@{/target}"表示生成一个超链接，该超链接的目标地址是@RequestMapping("/target")-->
    <a th:href="@{/target}">toTarget</a><br/>
</body>
</html>
```

##### 3.在请求控制器中创建处理请求的方法

```java
@RequestMapping("/target")
public String HelloWorld() {
    return "target";
}
```

#### 7、总结

浏览器发送请求，若请求地址符合前端控制器的<url-pattern>，该请求就会被前端控制器DispatcherServlet处理。前端控制器会读取SpringMVC的核心配置文件，通过扫描组件找到控制器，将请求地址和控制器中@RequestMapping注解的value属性值进行匹配，若匹配成功，该注解所标识的控制器方法就是处理请求的方法。处理请求的方法需要返回一个字符串类型的视图名称，该视图名称会被视图解析器解析，加上前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最终转发到视图所对应页面。





