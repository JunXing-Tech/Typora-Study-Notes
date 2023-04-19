# SpringMVC的视图

[toc]

#### 0、SpringMVC视图简介

SpringMVC中的视图是View接口，视图的作用渲染数据，将模型Model中的数据展示给用户

SpringMVC视图的种类很多，默认有转发视图和重定向视图

当工程引入jstl的依赖，转发视图会自动转换为JstlView

若使用的视图技术为Thymeleaf，在SpringMVC的配置文件中配置了Thymeleaf的视图解析器，由此视图解析器解析之后所得到的是ThymeleafView

#### 1、ThymeleafView

当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被SpringMVC配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过==转发的方式实现跳转==

```java
@Controller
public class ViewController{
    @RequestMapping("/testThymeleafView")
    public String testThymeleafView(){
        return "success";
    }
}
```

![](C:\Users\JunXing\Desktop\课程学习资源与文件管理\SpringMVC\笔记\img\img002.png)

#### 2、转发视图

pringMVC中默认的转发视图是InternalResourceView

SpringMVC中创建转发视图的情况：

当控制器方法中所设置的视图名称以"forward:"为前缀时，创建InternalResourceView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"forward:"去掉，剩余部分作为最终路径通过转发的方式实现跳转

例如"`forward:/`"，"`forward:/employee`"

```java
@RequestMapping("/testForward")
public String testForward(){
    return "forward:/testThymeleafView";
}

@RequestMapping("/testThymeleafView")
public String testThymeleafView(){
    return "success";
}
```

![image-20210706201316593](C:\Users\JunXing\Desktop\课程学习资源与文件管理\SpringMVC\笔记\img/img003.png)

使用forward关键字：使用forward关键字可以将==请求转发到另一个控制器==或者JSP页面进行处理。转发的URL不能暴露给客户端，客户端只能看到最终处理结果。

#### 3、重定向视图

SpringMVC中默认的重定向视图是RedirectView

当控制器方法中所设置的视图名称以"redirect:"为前缀时，创建RedirectView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"redirect:"去掉，剩余部分作为最终路径通过重定向的方式实现跳转

例如"`redirect:/`"（重定向到首页），"`redirect:/employee`"（重定向到指定地址）

```java
@RequestMapping("/testRedirect")
public String testRedirect(){
    return "redirect:/testThymeleafView";
}
```

![image-20210706201602267](C:\Users\JunXing\Desktop\课程学习资源与文件管理\SpringMVC\笔记\img/img004.png)

<u>重定向视图在解析时，会先将redirect:前缀去掉，然后会判断剩余部分是否以/开头，若是则会自动拼接上下文路径</u>

使用redirect关键字：使用redirect关键字可以将==请求重定向到另一个URL==进行处理。重定向的URL会暴露给客户端，客户端会看到浏览器地址栏中的URL发生了变化。

#### 4、总结

转发视图和重定向视图都是SpringMVC框架中用于处理控制器返回结果的视图类型，它们的作用不同，但也有一些联系。

转发视图用于将请求转发到另一个资源（JSP页面或另一个控制器）进行处理，转发的过程是在服务器端完成的，客户端浏览器并不知道这个过程，因此浏览器的地址栏不会发生变化。转发视图的使用可以帮助我们将请求的处理过程拆分成多个步骤，每个步骤可以由不同的控制器或者JSP页面来处理，从而提高代码的可维护性和可扩展性。

重定向视图用于将请求重定向到另一个URL进行处理，重定向的过程是在客户端浏览器完成的，客户端浏览器会重新发送请求到指定的URL，因此浏览器的地址栏会发生变化。重定向视图的使用可以帮助我们解决某些问题，例如防止表单重复提交、处理文件上传等。

<u>无任何前缀的转发是要求视图满足视图解析器的前缀和后缀。但是如果我们要访问的请求不符合我们当前的前缀和后缀，那么我们就得加上相对应的前缀了。</u>

#### 5、视图控制器view-controller

当控制器方法中，==仅仅用来实现页面跳转==，即只需要设置视图名称时，可以将处理器方法使用view-controller标签进行表示。

```xml
<!--
	path：设置处理的请求地址
	view-name：设置请求地址所对应的视图名称
	视图控制器的作用是将请求转发到特定的视图，而不需要编写控制器的代码。
	@RequestMapping("/")
    public String index(){
        return "index";
    }
	以上代码与<mvc:view-controller>标签作用相同
-->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
```

<u>当SpringMVC中设置任何一个view-controller时，其他控制器中的请求映射将全部失效，此时需要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签：</u>

```xml
<!--开启MVC的注解驱动-->
<mvc:annotation-driven />
```