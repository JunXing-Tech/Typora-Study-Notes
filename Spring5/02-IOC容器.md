## IOC容器

---

[toc]

> 控制反转（Inversion of Control），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做**依赖注入**，还要一种方式叫依赖查找。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中。

#### IOC容器（概念和原理）

##### 什么是IOC

1. 控制反转，把对象创建和对象之间的调用过程，交给 Spring 进行管理 

2. 使用 IOC 目的：为了耦合度降低 

3. 入门案例就是 IOC 实现

##### IOC底层原理

xml解析、工厂模式、反射

##### IOC过程

第一步：xml配置文件，配置创建的对象

```xml
<bean id="dao" class="路径"></bean>
```

第二步：有service类和dao类，创建工厂类

```java
class UserFactory{
    public static UserDao getDao(){
        //xml解析
        String calssValue = class属性值;
        //通过反射创建对象
        //Class类是一个特殊类，它用于表示JVM运行时类或接口的信息。
		//Class类提供很多方法用于获取类的各种信息，比如获取类名、判断该类是否是一个接口还是普通类等等。
        Class clazz = Class.forName(classValue);
        //通过Class类的newInstance()方法创建对象
        return (UserDao)clazz.newInstance();
    }
}
```

此方式进一步降低了耦合度，比如，如果class的路径变了，那么工厂类不需要任何变化，只需要改变xml配置文件

##### IOC（BeanFactory接口）

IOC容器本质上可以理解为工厂

1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂

2. Spring提供IOC容器实现两种方式：（两个接口）

​				BeanFactory

​				IOC容器的基本实现，是Spring内部的使用接口，不提供开发人员进行使用

​				**加载配置文件时不会创建对象，在获取对象（使用）才会创建对象**

​				ApplicationContext

​				BeanFactory接口的子接口，提供更多强大的功能，一般由开发人员进行使用

​				**加载配置文件时就会把在配置文件对象进行创建**

一般来说，跟偏向于使用ApplicationContext，因为可以在服务器启动时就创建对象，而不是什么时候用才什么时候创建。把这种耗时耗资源的过程交给服务器去完成，我们直接用就行了。

###### ApplicationContext接口的实现类

```java
//ClassPathXmlApplicationContext 默认会去 classPath 路径下找。classPath 路径指的就是编译后的 classes 目录。
FileSystemXmlApplicationContext()

//FileSystemXmlApplicationContext 默认是去项目的路径下加载，可以是相对路径，也可以是绝对路径，若是绝对路径，“file:” 前缀可以缺省。 
ClassPathXmlApplicationContex()
```

#### IOC操作Bean管理（概念）

##### 什么是Bean管理？

Bean管理指的是两个操作

Spring创建对象

Spring注入属性

##### Bean管理操作的两种方式

1.基于xml配置文件方式实现

2.基于注解方式实现

##### IOC操作Bean管理（基于xml方式）

###### 1.基于xml配置文件方式创建对象

```xml
<bean id="user" class="com.Spring5.User"></bean>
```

1.在Spring配置文件中，使用bean标签，标签里添加对应属性，就可以实现对象创建

2.在bean标签有很多属性，介绍常用属性

<table>
    <tr>
        <td>
            id属性
        </td>
        <td>
            唯一标识
        </td>
    </tr>
    <tr>
        <td>
            class属性
        </td>
        <td>
            类全路径（包类路径）
        </td>
    </tr>
</table>

3.创建对象时，默认是执行无参数构造方法完成对象创建

###### 2.基于xml方式注入属性

DI：依赖注入，就是注入属性

###### 3.第一种注入方式：set方法注入

1.创建类，定义属性和对应的set方法

```java
public class Book {
     //创建属性
     private String bname;
     private String bauthor;
     //创建属性对应的 set 方法
     public void setBname(String bname) {
         this.bname = bname;
     }
     public void setBauthor(String bauthor) {
         this.bauthor = bauthor;
     }
}
```

2.在Spring配置文件配置对象创建，配置属性注入

```xml
<!--set方法注入属性--->
<bean id="book" class="com.Spring5.Book">
    <!--使用property完成属性注入
		name：类里面属性名称
		value：向属性注入的值
	-->
    <property name="bname" value="ming"></property>
    <property name="bauthor" value="xiaoming"></property>
</bean>
```

3.在Main方法加载于获取配置对象

```java
@Test
public void TestBook(){
    //1.加载Spring配置文件
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    //2.获取配置创建的对象
    Book book = context,getBean("book", Book.class);
    
    System.out.prinyln(book);
} 
```

###### 4.第二种注入方式：使用有参函数构造进行注入

1.创建类，定义属性，创建属性对应参数构造方法

```java
public class Orders{
    //属性
    private String oname;
    private String address;
    //有参数构造
    public Orders(String oname, String address){
        this.oname = oname;
        this.address = address;
    }
    //测试
    public void ordersTest(){
        System.out.println(oname + ":" + address);
    }
}
```

2.在Spring配置文件

```xml
<beans>
    <!--有参数构造器注入属性-->
    <bean id="orders" class="com.Spring5.Orders"></bean>
    <constructor-arg name="oname" value="computer"></constructor-arg>
    <constructor-arg name="address" value="China"></constructor-arg>
    <!--index用于下标获取参数构造器的值，0表示参数构造器的第一个参数-->
    <constructor-arg index="" value=""></constructor-arg>
</beans>
```

3.在Main方法加载于获取配置对象

```java
@Test
public void TestOrders(){
    //1.加载Spring配置文件
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    //2.获取配置创建的对象
    Book book = context,getBean("orders", Orders.class);
    
    System.out.prinyln(orders);
    orders.ordersTest();
} 
```

###### 5. p名称空间注入

底层仍是set方式注入

使用p名称空间注入，可以简化基于xml配置方式

第一步添加p名称空间在配置文件中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       
       xmlns:p="http://www.springframework.org/schema/p"
       
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--配置 User 对象创建-->
        <bean id="user" class="com.partOne.User"></bean>
</beans>
```

第二步进行属性注入 ，在bean标签里进行操作

```xml
<bean id="book" class="com.Spring5.Book" p:bname="九阳神功" P:bauthor="无名氏"></bean>
```

#### IOC操作Bean管理（xml注入其他类型属性）

##### 1.字面量

###### 1.null值

在上文Book类中添加`private String address`和对应的set方法

```xml
<!--null 值-->
<!--<null/>表示赋予null给name所对应的属性-->
    <property name="address">
         <null/>
    </property>
</bean>
```

###### 2.属性值包含特殊字符

```xml
<!--属性值包含特殊符号
    1 把<>进行转义 &lt; &gt;
    2 把带特殊符号内容写到 CDATA
-->
<property name="address">
    <value><![CDATA[<<南京>>]]></value>
</property>

```

##### 2.注入属性-外部bean

通过Service层去调用Dao层这过程就称为外部Bean，外部Bean注入是跨包（Service包和Dao包）的

1.创建两个类service类和dao类

2.在service调用dao里面的方法

Service软件包中User Service类

```java
public class UserService{
    //创建UserDao类型属性，生成set方法
    private UserDao userDao;
    public void setUserDao(UserDao userDao){
        this.userDao = userDao;
    }
    public void add() {
        System.out.println("service add...");
        userDao.update();
    }
        /**原始方法
        *UserDao userDao = new UserDaoImpl();
        *userDao.update();
        */
}
```

Dao软件包中UserDao接口

```java
public interface UserDao{
    public void update();
}
```

Dao软件包中UserDaoImpl类

```java
public class UserDaoImpl implements UserDao{
    @Override
    public void update(){
        System.out.println("Dao update...");
    }
}
```

3.在Spring配置文件中进行配置

```xml
<!--1.service 和 dao 对象创建-->
<bean id="userService" class="com.Spring5.UserService">
    <!--2.注入userDao对象
		name属性：类里面的属性名称
		ref属性：创建userDao对象bean标签id值
	-->
    <property name="userDao" ref="userDaoImpl"></property>
</bean>
<bean id="userDaoImpl" class="com.Spring5.UserDaoImpl"></bean>
```

测试代码

```java
public class TestBean{
    @Test
    public void testAdd(){
        //加载Spring文件
        ApplicationContext context = new ClassPathXmlApplicationContext("bean2.xml");
        //获取配置创建的对象
        UserServlet userServlce = context.getBean("userService", UserService.class);
        
        userService.add();
    }
}
```

##### 3.注入属性-内部bean

内部Bean注入属性是在同一个软件包内

1.一对多关系：部门和员工 一个部门有多个员工，一个员工属于一个部门 部门是一，员工是多 

2.在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示

```java
//部门类
public class Dept {
    private String dname;
    public void setDname(String dname) {
        this.dname = dname;
    }
}

//员工类
public class Emp {
    private String ename;
    private String gender;
    //员工属于某一个部门，使用对象形式表示
    private Dept dept;
    public void setDept(Dept dept) {
        this.dept = dept;
    }
    public void setEname(String ename) {
        this.ename = ename;
    }
    public void setGender(String gender) {
        this.gender = gender;
    }
}
```

3.在spring配置文件中进行配置

```xml
<!--内部bean-->
<bean id="emp" class="com.Spring5.bean.Emp">
    <!--设置两个普通属性-->
    <property name="ename" value="lucy"></property>
 	<property name="gender" value="女"></property>
	<!--设置对象类型属性-->
    <property name="dept">
         <bean id="dept" class="com.atguigu.spring5.bean.Dept">
             <property name="dname" value="安保部">	</property>
         </bean>
     </property>
</bean>
```

##### 4.注入属性-级联赋值

###### 1.第一种写法

```xml
<!--级联赋值-->
<bean id="emp" class="com.atguigu.spring5.bean.Emp">
     <!--设置两个普通属性-->
     <property name="ename" value="lucy"></property>
     <property name="gender" value="女"></property>
     <!--级联赋值-->
     <property name="dept" ref="dept"></property>
</bean>
<bean id="dept" class="com.atguigu.spring5.bean.Dept">
     <property name="dname" value="财务部"></property>
</bean>

```

###### 2.第二种写法

```java
//员工属于某一个部门，使用对象形式表示
private Dept dept;

//生成dept的get方法
public Dept getDept(){
    return dept;
}

public void setDept(Dept dept){
    this.dept = dept
}
```

```xml
<!--级联赋值-->
<bean id="emp" class="com.atguigu.spring5.bean.Emp">
    <!--设置两个普通属性-->
    <property name="ename" value="lucy"></property>
    <property name="gender" value="女"></property>
    <!--级联赋值-->
    <property name="dept" ref="dept"></property>
    <!--dept.dname表示向dept对象中设置dname的值-->
    <!--还需要在Emp类中生成getDept方法-->
    <property name="dept.dname" value="技术部"></property>
</bean>
<bean id="dept" class="com.atguigu.spring5.bean.Dept">
    <property name="dname" value="财务部"></property>
</bean>
```

#### IOC操作Bean管理（xml注入集合属性）

1、注入数组类型属性 

2、注入 List 集合类型属性 

3、注入 Map 集合类型属性

（1）创建类，定义数组、list、map、set 类型属性，生成对应 set 方法

```java
public class Stu{
    //数组类型属性
    private String[] courses;
    //List集合类型属性
    private List<String> list;
    //Map集合类型属性
    private Map<String, String> maps;
    //Set集合类型属性
    private Set<String> sets;
    
    //set和get方法
    public void setSets(Set<String> sets) {
         this.sets = sets;
     }
     public void setCourses(String[] courses) {
         this.courses = courses;
     }
     public void setList(List<String> list) {
         this.list = list;
     }
     public void setMaps(Map<String, String> maps) {
         this.maps = maps;
     }
}
```

（2）在 spring 配置文件进行配置

```xml
<!--集合类型属性注入--->
<bean  id="stu" class="com.spring5.collectiontype.Stu">
    <!--数组类型注入-->
    <property name="courses">
        <array>
            <value>java 课程</value>
            <value>数据库课程</value>
        </array>
    </property>
    <!--List类型属性注入-->
    <property name="list">
        <list>
            <value>xiaoming</value>
            <value>xiaohong</value>
        </list>
    </property>
    <!--Map类型属性注入-->
    <property name="maps">
        <map>
            <entry key="JAVA" value="java"></entry>
            <entry key="PHP" value="php"></entry>
        </map>
    </property>
    <!--Set类型属性注入-->
    <property>
        <set>
            <value>MySql</value>
            <value>Redies</value>
        </set>
    </property>
</bean>
```

4.在集合里面设置对象类型值

```xml
<!--创建多个 course 对象-->
<bean id="course1" class="com.atguigu.spring5.collectiontype.Course">
    <property name="cname" value="Spring5 框架"></property>
</bean>
<bean id="course2" class="com.atguigu.spring5.collectiontype.Course">
    <property name="cname" value="MyBatis 框架"></property>
</bean>
<!--注入 list 集合类型，值是对象-->
<property name="courseList">
    <list>
        <ref bean="course1"></ref>
        <ref bean="course2"></ref>
    </list>
</property>

```

5.把集合注入部分提取出来

（1）在 spring 配置文件中引入名称空间 util

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/util
       http://www.springframework.org/schema/util/spring-util.xsd">
</beans>
```

（2）使用 util 标签完成 list 集合注入提取

```xml
<!--1.提取List集合类型属性注入-->
<util:list id="bookList">
    <value>xiaolin</value>
    <value>xiaozhou</value>
    <value>xiaozeng</value>
</util:list>
<!--2.提取List集合类型属性注入使用-->
<bean id="book" class="com.spring5.collectiontype.Book">
    <property name="list" ref="bookList"></property>
</bean>
```

#### IOC操作Bean管理（FactoryBean）

[返回文首](#IOC容器)

