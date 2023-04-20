# HttpMessageConverter

[toc]

HttpMessageConverter，报文信息转换器，将请求报文转换为Java对象，或将Java对象转换为响应报文

HttpMessageConverter提供了两个注解和两个类型：@RequestBody，@ResponseBody，RequestEntity，ResponseEntity

#### 1、@RequestBody

@RequestBody可以获取请求体，需要在控制器方法设置一个形参，使用@RequestBody进行标识，当前请求的请求体就会为当前注解所标识的形参赋值

```html
<form th:action="@{/testRequestBody}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit" value="测试@RequestBody">
</form>
```

```java
@RequestMapping("/testRequestBody")
public String testRequestBody(@RequestBody String requestBody){
    System.out.println("requestBody:"+requestBody);
    return "success";
}
```

输出结果：requestBody:username=admin&password=123456

#### 2、RequestEntity

RequestEntity封装整个请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，可以通过getHeaders()获取请求头信息，通过getBody()获取请求体信息

```java
@RequestMapping("/testRequestEntity")
public String testRequestEntity(RequestEntity<String> requestEntity){
    System.out.println("requestHeader:"+requestEntity.getHeaders());
    System.out.println("requestBody:"+requestEntity.getBody());
    return "success";
}
```

输出结果：

requestHeader:[host:"localhost:8080", connection:"keep-alive", content-length:"27", cache-control:"max-age=0", sec-ch-ua:"" Not A;Brand";v="99", "Chromium";v="90", "Google Chrome";v="90"", sec-ch-ua-mobile:"?0", upgrade-insecure-requests:"1", origin:"http://localhost:8080", user-agent:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36"]

requestBody:username=admin&password=123

#### 3、通过HttpServletResponse响应浏览器数据

```java
@RequestMapping("/testResponse")
public void testResponse(HttpServletResponse response) throws IOException{
    response.getWriter().print("Hello, response");
}
```

```html
<a th:href="@{/testResponse}">通过servletAPI的response对象响应浏览器数据</a>
```

##### request和response介绍

request是代表HTTP请求信息的对象，response是代表HTTP响应信息的对象。

当浏览器发请求访问服务器中的某一个[Servlet](https://so.csdn.net/so/search?q=Servlet&spm=1001.2101.3001.7020)时，服务器将会调用Servlet中的service方法来处理请求。在调用service方法之前会创建出request和response对象。

其中request对象中封装了浏览器发送给服务器的请求信息（请求行、请求头、请求实体等），response对象中将会封装服务器要发送给浏览器的响应信息（状态行、响应头、响应实体），在service方法执行完后，服务器再将response中的数据取出，按照HTTP协议的格式发送给浏览器。

每次浏览器访问服务器，服务器在调用service方法处理请求之前都会创建request和response对象。（即，服务器每次处理请求都会创建request和response对象）

在请求处理完，响应结束时，服务器会销毁request和response对象。

#### 4、@ResponseBody

@ResponseBody用于标识一个控制器方法，可以将==该方法的返回值直接作为响应报文的响应体响应到浏览器==

```java
@RequestMapping("/testResponseBody")
@ResponseBody
public String testResponseBody(){
    return "success";
}
```

结果：浏览器页面显示success

#### 5、SpringMVC处理json

##### @ResponseBody处理json的步骤

1. 导入jackson的依赖

```xml
<dependency>    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

2. 在SpringMVC的核心配置文件中开启mvc的注解驱动，此时在HandlerAdaptor中会自动装配一个消息转换器：MappingJackson2HttpMessageConverter，可以将响应到浏览器的Java对象转换为Json格式的字符串

```xml
<mvc:annotation-driven />
```

3. 在处理器方法上使用@ResponseBody注解进行标识

4. 将Java对象直接作为控制器方法的返回值返回，就会自动转换为==Json格式的字符串==

```java
//浏览器无法直接接收一个User对象，报错为：HttpMessageNotWritableException
@RequestMapping("/testResponseUser")
@ResponseBody
public User testResponseUser(){
    return new User(1001,"admin","123456",23,"男");
}
```

浏览器的页面中展示的结果：{"id":1001,"username":"admin","password":"123456","age":23,"sex":"男"}

#### 6、SpringMVC处理ajax

##### 1.请求超链接

```html
<div id="app">
	<a th:href="@{/testAxios}" @click="testAxios">SpringMVC处理Ajax</a><br>
</div>
```

##### 2.通过vue和axios处理点击事件

```html
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
<script type="text/javascript">
    var vue = new Vue({
        el:"#app",
        methods:{
            testAjax:function (event) {
                axios({
                    method:"post",
                    url:event.target.href,
                    params:{
                        username:"admin",
                        password:"123456"
                    }
                }).then(function (response) {
                    alert(response.data);
                });
                event.preventDefault();
            }
        }
    });
</script>
```

##### 3.控制器方法

```java
@RequestMapping("/testAxios")
@ResponseBody
public String testAxios(String username, String password){
    System.out.println("username:"+username+",password:"+password);
    return "hello,axios";
}
```

#### 7、@RestController注解

@RestController注解是springMVC提供的一个==复合注解==，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中的每个方法添加了@ResponseBody注解

#### 8、ResponseEntity

ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文