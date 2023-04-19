# RESTFul案例

[toc]



#### 1、准备工作

要求：和传统 CRUD 一样，实现对员工信息的增删改查。

- 搭建环境

- 准备实体类

```java
package com.mvc.bean;

public class Employee {

   private Integer id;
   private String lastName;

   private String email;
   //1 male, 0 female
   private Integer gender;
   
   public Integer getId() {
      return id;
   }

   public void setId(Integer id) {
      this.id = id;
   }

   public String getLastName() {
      return lastName;
   }

   public void setLastName(String lastName) {
      this.lastName = lastName;
   }

   public String getEmail() {
      return email;
   }

   public void setEmail(String email) {
      this.email = email;
   }

   public Integer getGender() {
      return gender;
   }

   public void setGender(Integer gender) {
      this.gender = gender;
   }

   public Employee(Integer id, String lastName, String email, Integer gender) {
      super();
      this.id = id;
      this.lastName = lastName;
      this.email = email;
      this.gender = gender;
   }

   public Employee() {
   }
}
```

* 准备dao模拟数据

```java
package com.mvc.dao;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

import com.atguigu.mvc.bean.Employee;
import org.springframework.stereotype.Repository;


@Repository
public class EmployeeDao {

   private static Map<Integer, Employee> employees = null;
   
   static{
      employees = new HashMap<Integer, Employee>();

      employees.put(1001, new Employee(1001, "E-AA", "aa@163.com", 1));
      employees.put(1002, new Employee(1002, "E-BB", "bb@163.com", 1));
      employees.put(1003, new Employee(1003, "E-CC", "cc@163.com", 0));
      employees.put(1004, new Employee(1004, "E-DD", "dd@163.com", 0));
      employees.put(1005, new Employee(1005, "E-EE", "ee@163.com", 1));
   }
   
   private static Integer initId = 1006;
   
   public void save(Employee employee){
      if(employee.getId() == null){
         employee.setId(initId++);
      }
      employees.put(employee.getId(), employee);
   }
   
   public Collection<Employee> getAll(){
      return employees.values();
   }
   
   public Employee get(Integer id){
      return employees.get(id);
   }
   
   public void delete(Integer id){
      employees.remove(id);
   }
}
```

#### 2、功能清单

##### 1.请求方式中，什么时候用GET请求，什么时候用POST请求

在Web开发中，GET和POST是HTTP协议中最常用的请求方式，它们有不同的用途和特点。

GET请求用于获取资源，它是一种幂等的请求方式，也就是说多次请求返回的结果是相同的。GET请求的参数会附加在URL中，可以通过浏览器地址栏直接访问。因此，GET请求适合获取资源，例如查询数据、获取页面等操作。

POST请求用于提交数据，它不是幂等的，也就是说多次请求返回的结果可能不同。POST请求的参数会放在HTTP请求体中，而不是URL中，因此POST请求更加安全，适合提交数据，例如新增或修改数据等操作。

通常来说，如果请求仅仅是获取数据或者页面，使用GET请求；如果请求需要提交数据或者有副作用（比如修改服务器数据），则使用POST请求。但是，实际应用中，GET和POST请求的选择也要考虑具体业务场景、请求参数大小、安全性等因素。

| 功能               | URL 地址    | 请求方式 |
| ------------------ | ----------- | -------- |
| 访问首页           | /           | GET      |
| 查询全部数据       | /employee   | GET      |
| 删除               | /employee/2 | DELETE   |
| 跳转到添加数据页面 | /toAdd      | GET      |
| 执行保存           | /employee   | POST     |
| 跳转到更新数据页面 | /employee/2 | GET      |
| 执行更新           | /employee   | PUT      |

#### 3、具体功能：访问首页

##### 1.配置view-controller

```xml
<mvc:view-controller path="/" view-name="index"/>
```

##### 2.创建页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8" >
    <title>Title</title>
</head>
<body>
    <h1>首页</h1>
    <a th:href="@{/employee}">访问员工信息</a>
</body>
</html>
```

#### 4、具体功能：查询所有员工数据

##### 1.控制器方法

```java
@RequestMapping(value = "/employee", method = RequestMethod.GET)
public String getEmployeeList(Model model){
    Collection<Employee> employeeList = employeeDao.getAll();
    model.addAttribute("employeeList", employeeList);
    return "employee_list";
}
```

##### 2.创建employee_list.html

```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Employee  Info</title>
    <script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
</head>
<body>

    <table border="1" cellpadding="0" cellspacing="0" style="text-align: center;" id="dataTable">
        <tr>
            <th colspan="5">Employee Info</th>
        </tr>
        <tr>
            <th>id</th>
            <th>lastName</th>
            <th>email</th>
            <th>gender</th>
            <th>options(<a th:href="@{/toAdd}">add</a>)</th>
        </tr>
        <tr th:each="employee : ${employeeList}">
            <td th:text="${employee.id}"></td>
            <td th:text="${employee.lastName}"></td>
            <td th:text="${employee.email}"></td>
            <td th:text="${employee.gender}"></td>
            <td>
                <a class="deleteA" @click="deleteEmployee" th:href="@{'/employee/'+${employee.id}}">delete</a>
                <a th:href="@{'/employee/'+${employee.id}}">update</a>
            </td>
        </tr>
    </table>
</body>
</html>
```

#### 5、具体功能：删除功能

##### 1.创建处理delete请求方式的表单

```html
<!-- 作用：通过超链接控制表单的提交，将post请求转换为delete请求 -->
<form id="deleteForm" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input type="hidden" name="_method" value="delete"/>
</form>
```

##### 2.删除超链接绑定点击事件

###### 引入vue.js

```html
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
```

###### 删除超链接

```html
<a @click="deleteEmployee" th:href="@{'/employee/'+${employee.id}}">delete</a>
```

###### 通过vue处理点击事件

```html
<script type="text/javascript">
    var vue = new Vue({
        el:"#dataTable",
        methods:{
            //event表示当前事件
            deleteEmployee:function (event) {
                //通过id获取表单标签
                var deleteForm = document.getElementById("deleteForm");
                //将触发事件的超链接的href属性为表单的action属性赋值
                deleteForm.action = event.target.href;
                //提交表单
                deleteForm.submit();
                //阻止超链接的默认跳转行为
                event.preventDefault();
            }
        }
    });
</script>
```

##### 3.控制器方法

```java
@RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
public String deleteEmployee(@PathVariable("id") Integer id){
    employeeDao.delete(id);
    return "redirect:/employee";
}
```

```xml
<!--开发对静态资源的访问-->
<mvc:default-servlet-handler />
```

<mvc:default-servlet-handler />标记用于在Spring MVC中配置默认的servlet处理程序，可以用于服务静态资源例如图片、CSS文件和JavaScript文件。当请求一个静态资源时，如果没有特定的处理程序来处理这个请求，那么这个请求将会被转发给默认的servlet处理程序来处理。这个标记可以确保静态资源能够被正确地加载和显示，从而提高Web应用程序的性能和用户体验。

##### 4.删除功能代码全览

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Employee Info</title>
</head>
<body>

<table border="1" cellpadding="0" cellspacing="0" style="text-align: center;" id="dataTable">
    <tr>
        <th colspan="5">Employee Info</th>
    </tr>
    <tr>
        <th>id</th>
        <th>lastName</th>
        <th>email</th>
        <th>gender</th>
        <th>options(<a th:href="@{/toAdd}">add</a>)</th>
    </tr>
    <!--数据循环-->
    <tr th:each="employee : ${employeeList}">
        <td th:text="${employee.id}"></td>
        <td th:text="${employee.lastName}"></td>
        <td th:text="${employee.email}"></td>
        <td th:text="${employee.gender}"></td>
        <td>
            <a @click="deleteEmployee" th:href="@{'/employee/'+${employee.id}}">delete</a>
            <a th:href="@{'/employee/'+${employee.id}}">update</a>
        </td>
    </tr>
</table>

<!-- 作用：通过超链接控制表单的提交，将post请求转换为delete请求 -->
<form id="deleteForm" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input type="hidden" name="_method" value="delete"/>
</form>

<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript">
    var vue = new Vue({
        el:"#dataTable",
        methods:{
            //event表示当前事件
            deleteEmployee:function (event) {
                //通过id获取表单标签
                var deleteForm = document.getElementById("deleteForm");
                //将触发事件的超链接的href属性为表单的action属性赋值
                deleteForm.action = event.target.href;
                //提交表单
                deleteForm.submit();
                //阻止超链接的默认跳转行为
                event.preventDefault();
            }
        }
    });
</script>
</body>
</html>
```

```java
@Controller
public class EmployeeController {

    @Autowired
    private EmployeeDao employeeDao;//自动装配依赖

    @RequestMapping(value = "/employee", method = RequestMethod.GET)
    public String getAllEmployee(Model model){
        Collection<Employee> employeeList = employeeDao.getAll();//获取数据

        model.addAttribute("employeeList", employeeList);//转发

        return "employee_list";
    }

    @RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
    public String deleteEmployee(@PathVariable("id") Integer id){
        employeeDao.delete(id);
        return "redirect:/employee";
    }
}
```

#### 6、具体功能：跳转到添加数据页面

##### 1.配置view-controller

```xml
<mvc:view-controller path="/toAdd" view-name="employee_add"></mvc:view-controller>
```

##### 2.创建employee_add.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Add Employee</title>
</head>
<body>

<form th:action="@{/employee}" method="post">
    lastName:<input type="text" name="lastName"><br>
    email:<input type="text" name="email"><br>
    gender:<input type="radio" name="gender" value="1">male
    <input type="radio" name="gender" value="0">female<br>
    <input type="submit" value="add"><br>
</form>
    
</body>
</html>
```

#### 7、具体功能：执行保存

##### 1.控制器方法

```java
@RequestMapping(value = "/employee", method = RequestMethod.POST)
public String addEmployee(Employee employee){
    employeeDao.save(employee);
    return "redirect:/employee";
}
```

#### 8、具体功能：跳转到更新数据页面

##### 1.修改超链接

```html
<a th:href="@{'/employee/'+${employee.id}}">update</a>
```

##### 2.控制器方法

```java
@RequestMapping(value = "/employee/{id}", method = RequestMethod.GET)
public String getEmployeeById(@PathVariable("id") Integer id, Model model){
    Employee employee = employeeDao.get(id);
    model.addAttribute("employee", employee);
    return "employee_update";
}
```

##### 3.创建employee_update.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Update Employee</title>
</head>
<body>

<form th:action="@{/employee}" method="post">
    <input type="hidden" name="_method" value="put">
    <input type="hidden" name="id" th:value="${employee.id}">
    lastName:<input type="text" name="lastName" th:value="${employee.lastName}"><br>
    email:<input type="text" name="email" th:value="${employee.email}"><br>
    <!--
        th:field="${employee.gender}"可用于单选框或复选框的回显
        若单选框的value和employee.gender的值一致，则添加checked="checked"属性
    -->
    gender:<input type="radio" name="gender" value="1" th:field="${employee.gender}">male
    <input type="radio" name="gender" value="0" th:field="${employee.gender}">female<br>
    <input type="submit" value="update"><br>
</form>

</body>
</html>
```

#### 9、具体功能：执行更新

##### 1.控制器方法

```java
@RequestMapping(value = "/employee", method = RequestMethod.PUT)
public String updateEmployee(Employee employee){
    employeeDao.save(employee);
    return "redirect:/employee";
}
```













