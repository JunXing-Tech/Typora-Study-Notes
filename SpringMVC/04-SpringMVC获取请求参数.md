# SpringMVC获取请求参数

[toc]

#### 1、通过ServletAPI获取

将HttpServletRequest作为控制器方法的形参，此时HttpServletRequest类型的参数表示封装了当前请求的请求报文的对象。

```java
@Controller
public class ParamController{
    @RequestMapping("/testServletAPI")
 	//形参位置的request表示当前请求
    public String testServletAPI(HttpServletRequest request){
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        System.out.println("username:"+username+",password:"+password);
        return "success";
    }
}
```

```html
<a th:href="@{/testServletAPI(username='admin',password='123456')}"></a>
```

#### 2、通过控制器方法的形参获取请求参数

在控制器方法的形参位置，设置和请求参数同名的形参，当浏览器发送请求，匹配到请求映射时，在DispatcherServlet中就会将请求参数赋值给相应的形参。

```java
@RequestMapping("/testParam")
public String testParam(String username, String password){
    System.out.println("username:"+username+",password:"+password);
    return "success";
}
```



```html
<a th:href="@{/testParam(username='admin',password=123456)}">测试获取请求参数-->/testParam</a><br>
```

##### 1.关于形参的使用的情况

* 若请求所传输的请求参数中有多个同名的请求参数，此时可以在控制器方法的形参中设置字符串数组或者字符串类型的形参接收此请求参数。

* 若使用字符串数组类型的形参，此参数的数组中包含了每一个数据。

* 若使用字符串类型的形参，此参数的值为每个数据中间使用逗号拼接的结果。

```html
<form th:action="@{/testParam}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    爱好：<input type="checkbox" name="hobby" value="a">a
    <input type="checkbox" name="hobby" value="b">b
    <input type="checkbox" name="hobby" value="c">c<br>
    <input type="submit" value="Test">
</form>
```

```java
@RequestMapping("/testParam")
public String testParam(String username, String password, String[] hobby){
    //若请求参数中出现多个同名的请求参数，可以在控制器方法的参数位置设置字符串类型或字符串数组接收此请求参数
    //若使用字符串类型的形参，最终结果为请求参数的每一个值之间使用逗号进行拼接
    System.out.println("username:"+username+",password:"+password+",hobby"+Arrays.toString(hobby));
    return "success";
} 
```

#### 3、@RequestParam注解处理请求参数和控制器方法的形参的映射关系

```java
@RequestMapping("/testParam")
public String testParam(
    @RequestParam(value = "user_name", required = false, defaultValue = "hehe") String username,
    String password,
    String[] hobby
)
```

@RequestParam是将请求参数和控制器方法的形参创建映射关系。

@RequestParam注解一共有三个属性：

1. `value`：指定为形参赋值的请求参数的参数名
2. `required`：设置是否必须传输此请求参数，默认值为true
   1. 若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置defaultValue属性，则页面报错400：Required String parameter 'xxx' is not present；若设置为false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为null
3. `defaultValue`：不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值为""时，则使用默认值为形参赋值

#### 4、@RequestHeader注解处理请求头信息和控制器方法的形参的映射关系

```java
@RequestMapping("/testParam")
public String testParam(
    ...
    //@RequestHeader的value类型为"Host"且为必传，若没传则为defaultValue的值
    @RequestHeader(value = "Host", required = true, defaultValue = "haha") String host
)
```

@RequestHeader是==将请求头信息和控制器方法的形参创建映射关系==。

@RequestHeader注解一共有三个属性：value、required、defaultValue，==用法同@RequestParam。==

#### 5、@CookieValue注解处理cookie数据和控制器方法的形参的映射关系

```java
@RequestMapping("/testParam")
public String testParam(
    ...
	@CookieValue("JSESSIONID") String JSESSIONID
)
```

@CookieValue是将cookie数据和控制器方法的形参创建映射关系。

@CookieValue注解一共有三个属性：value、required、defaultValue，==用法同@RequestParam==。

#### 6、通过POJO（实体类型）获取请求参数

可以在控制器方法的形参位置设置一个实体类类型的形参，此时若浏览器传输的请求参数的参数名和实体类中的属性名一致，那么请求参数就会为此属性赋值

```html
<form th:action="@{/testBean}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    性别：<input type="radio" name="sex" value="男">男<input type="radio" name="sex" value="女">女<br>
    年龄：<input type="text" name="age"><br>
    邮箱：<input type="text" name="email"><br>
    <input type="submit">
</form>
```

```java
public class User{
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String sex;
    private String email;
    
    //有参、无参构造器
    //set、get方法
    //toString方法
}
```

```java
@RequestMapping("/testBean")
public String testBean(User user){
    System.out.println(user);
    return "success";
}
//最终结果-->User{id=null, username='张三', password='123', age=23, sex='男', email='123@qq.com'}
```

#### 7、解决获取请求参数的乱码问题（通过CharacterEncodingFilter处理获取请求参数的乱码问题）

> get请求的中文乱码问题可去tomcat的文件夹位置："D:\apache-tomcat-8.5.84-windows-x64\conf/server.xml"中<Connector/>中添加URIEncoding="UTF-8"可解决。

解决获取请求参数的乱码问题，可以使用SpringMVC提供的编码过滤器CharacterEncodingFilter，但是必须在web.xml中进行注册

```xml
<!--配置springMVC的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

==SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效==