# @RequestMapping注解

[toc]

#### 1、@RequestMapping注解的功能

从注解名称上我们可以看到，@RequestMapping注解的作用就是==将请求和处理请求的控制器方法关联起来，建立映射关系==。

SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。

#### 2、@RequestMapping注解的位置

@RequestMapping标识==一个类==：设置映射请求的请求路径的初始信息。

@RequestMapping标识==一个方法==：设置映射请求请求路径的具体信息。

```java
//当@RequestMapping注解同时放在类和方法上时，它们的路径会进行拼接，即类上的路径和方法上的路径会拼接成最终的请求路径。这样可以更加灵活地对请求路径进行控制和管理。
@Controller
@RequestMapping("/test")
public class RequestMappingController {
	//此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping(){
        return "success";
    }
}
```

#### 3、@RequestMapping注解的value属性

@RequestMapping注解的value属性通过==请求的请求地址==匹配请求映射。

@RequestMapping注解的value属性是一个==字符串类型的数组==，表示==该请求映射能够匹配多个请求地址所对应的请求==。

`String[] value() default{};`

@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射。

```html
<a th:href="@{/testRequestMapping}">测试@RequestMapping的value属性-->/testRequestMapping</a><br>
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
```

```java
//至少符合@RequestMapping的value值中的一个
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
    return "success";
}
```

#### 4、@RequestMapping注解的method属性

@RequestMapping注解的method属性通过==请求的请求方式（get或post）匹配请求映射==。

@RequestMapping注解的method属性是一个RequestMethod类型的==数组==，表示该请求映射能够匹配多种请求方式的请求。

若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错405：Request method 'POST' not supported

```html
<form th:action="@{/test}" method="post">
    <input type="submit">
</form>
```

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"},
        method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
    return "success";
}
```

##### 1.@RequestMapping注解结合请求方式的派生注解

1.对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解。

* 处理get请求的映射-->@GetMapping

* 处理post请求的映射-->@PostMapping

* 处理put请求的映射-->@PutMapping

* 处理delete请求的映射-->@DeleteMapping

```java
@GetMapping("/testGetMapping")
public String testGetMapping(){
    return success;
}
```

##### 2.测试form表单是否能够发送put和delete请求方式的请求

```java
@RequestMapping(value = "/testPut", method = RequestMethod.PUT)
public String testPut(){
    return "success";
}
```

```html
<form th:action = "@{/testPut}" method = "put">
    <input type="submit" value="">
</form>
<!--报错为：Request method 'GET' not supported-->
```

1.常用的请求方式有get，post，put，delete

目前浏览器==只支持get和post==，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理。

若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter，在RESTful部分会提及。

#### 5、@RequestMapping注解的params属性（了解）

@RequestMapping注解的params属性通过请求的==请求参数==匹配请求映射

@RequestMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

```java
@RequestMapping(value = "/path", params = {"param1=value1", "param2!=value2"})
```

* "param"：要求请求映射所匹配的请求必须携带param请求参数

* "!param"：要求请求映射所匹配的请求必须不能携带param请求参数

* "param=value"：要求请求映射所匹配的请求必须携带param请求参数且param=value

* "param!=value"：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

==需要注意的是==，params属性指定的限制条件必须全部满足才能匹配成功。如果没有指定params属性，则表示请求没有任何限制条件。

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
        ,method = {RequestMethod.GET, RequestMethod.POST}
        ,params = {"username","password!=123456"}
)
public String testRequestMapping(){
    return "success";
}
@RequestMapping(
    value = "/testParamsAndHeaders",
    params = {"username"}
)    
public String testParamsAnd Headers(){
    return "success";
}
```

```html
<!--无请求参数"username"时，浏览器会报错 "HTTP Status 400 - Parameter conditions "username" not met for actual request parameters"-->
<a th:href = "@{/testParamsAndHeaders}"></a>
<a th:href = "@{/testParamsAndHeaders(username="admin",password=123456}"></a>
```

若当前请求满足@RequestMapping注解的value和method属性，但是不满足params属性，此时页面回报错==400：Parameter conditions "username, password!=123456" not met for actual request parameters: username={admin}, password={123456}==

#### 6、@RequestMapping注解的headers属性（了解） 

> @RequestMapping注解的headers属性可以用于指定请求头的限制条件

```java
@RequestMapping(value = "/path", headers = {"Content-Type=application/json", "Accept=application/json"})
```

@RequestMapping注解的headers属性通过请求的==请求头信息==匹配请求映射

@RequestMapping注解的headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

* "header"：要求请求映射所匹配的请求必须携带header请求头信息

* "!header"：要求请求映射所匹配的请求必须不能携带header请求头信息

* "header=value"：要求请求映射所匹配的请求必须携带header请求头信息且header=value

* "header!=value"：要求请求映射所匹配的请求必须携带header请求头信息且header!=value

若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面显示404错误，即资源未找到

#### 7、SpringMVC支持ant风格的路径

即在路径中使用通配符和正则表达式来匹配多个URL路径。这种方式可以方便地处理复杂的URL匹配需求，如模糊匹配、多级匹配等。

？：表示任意的单个字符

*：表示任意的0个或多个字符

\**：表示任意的一层或多层目录

==注意==：在使用\**时，只能使用/**/xxx的方式

#### 8、SpringMVC支持路径中的占位符（重点）

* 原始方式：`/deleteUser?id=1`

* rest方式：`/deleteUser/1`

SpringMVC路径中的占位符常用于RESTful风格中，当请求路径中将某些数据通过路径的方式传输到服务器中，就可以在相应的@RequestMapping注解的value属性中通过占位符{xxx}表示传输的数据，在通过@PathVariable注解，将占位符所表示的数据赋值给控制器方法的形参

```html
<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a><br>
```

```java
@RequestMapping("/testRest/{id}/{username}")
public String testRest(@PathVariable("id") String id, @PathVariable("username") String username){
    System.out.println("id:"+id+",username:"+username);
    return "success";
}
//最终输出的内容为-->id:1,username:admin
```

