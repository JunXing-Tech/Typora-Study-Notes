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

​				1.BeanFactory

```markdown
IOC容器的基本实现，是Spring内部的使用接口，不提供开发人员进行使用

**加载配置文件时不会创建对象，在获取对象（使用）才会创建对象**
```

​				2.ApplicationContext

```markdown
BeanFactory接口的子接口，提供更多强大的功能，一般由开发人员进行使用

**加载配置文件时就会把在配置文件对象进行创建**
```

一般来说，更偏向于使用==ApplicationContext==，因为可以在服务器启动时就创建对象，而不是什么时候用才什么时候创建。把这种耗时、耗资源的过程交给服务器去完成，我们直接用就行了。

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

1. Spring创建对象

2. Spring注入属性

在 Spring 中，可以通过以下两种方式注入属性：

* 使用 @Value 注解：通过在类的字段上使用 @Value 注解，可以将一个字符串值注入到该字段中。例如：

```java
public class MyClass {
    @Value("my property value")
    private String myProperty;
}
```

* 使用 XML 或 Java 配置文件：通过配置文件，在 bean 的定义中指定属性的值。例如：

```xml
<bean id="myBean" class="com.example.MyClass">
    <property name="myProperty" value="my property value"/>
</bean>
```



##### Bean管理操作的两种方式

1. 基于xml配置文件方式实现

2. 基于注解方式实现

##### IOC操作Bean管理（基于xml方式）

###### 基于xml配置文件方式创建对象

```xml
<bean id="user" class="com.Spring5.User"></bean>
```

在Spring配置文件中，使用bean标签，标签里添加对应属性，就可以实现对象创建

在bean标签有很多属性，介绍常用属性：

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

创建对象时，默认是执行无参数构造方法完成对象创建

###### 基于xml方式注入属性

DI：依赖注入，就是注入属性

###### 第一种注入方式：set方法注入

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

3.在Main方法加载获取配置对象

```java
@Test
public void TestBook(){
    //1.加载Spring配置文件
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    //2.获取配置创建的对象
    Book book = context.getBean("book", Book.class);
    
    System.out.prinyln(book);
} 
```

###### 第二种注入方式：使用有参函数构造进行注入

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
    <bean id="orders" class="com.Spring5.Orders">
        <!--<constructor-arg>是在Spring框架中用于注入构造函数参数的标签-->
        <constructor-arg name="oname" value="computer"></constructor-arg>
        <constructor-arg name="address" value="China"></constructor-arg>
        <!--index用于下标获取参数构造器的值，0表示参数构造器的第一个参数-->
        <constructor-arg index="" value=""></constructor-arg>
    </bean>
</beans>
```

3.在Main方法加载于获取配置对象

```java
@Test
public void TestOrders(){
    //1.加载Spring配置文件
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    //2.获取配置创建的对象
    Book book = context.getBean("orders", Orders.class);
    
    System.out.prinyln(orders);
    orders.ordersTest();
} 
```

###### p名称空间注入

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
<bean id="book" class="com.Spring5.Book" p:bname="九阳神功" p:bauthor="无名氏"></bean>
```

#### IOC操作Bean管理（xml注入其他类型属性）

##### 1.字面量

###### null值

在上文Book类中添加`private String address`和对应的set方法

```xml
<!--null 值-->
<!--<null/>表示赋予null给name所对应的属性-->
    <property name="address">
         <null/>
    </property>
</bean>
```

###### 属性值包含特殊字符

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

通过Service层去调用Dao层这过程就称为外部Bean，外部Bean注入是跨软件包（Service包和Dao包）注入的方式

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
        UserService userService = context.getBean("userService", UserService.class);
        
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
         <bean id="dept" class="com.spring5.bean.Dept">
             <property name="dname" value="安保部"></property>
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
<bean id="dept" class="com.spring5.bean.Dept">
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
<bean id="emp" class="com.spring5.bean.Emp">
    <!--设置两个普通属性-->
    <property name="ename" value="lucy"></property>
    <property name="gender" value="女"></property>
    <!--级联赋值-->
    <property name="dept" ref="dept"></property>
    <!--dept.dname表示向dept对象中设置dname的值-->
    <!--还需要在Emp类中生成getDept方法-->
    <property name="dept.dname" value="技术部"></property>
</bean>
<bean id="dept" class="com.spring5.bean.Dept">
    <property name="dname" value="财务部"></property>
</bean>
```

#### IOC操作Bean管理（xml注入集合属性）

##### 1、注入数组类型、 List 集合类型、Map 集合类型属性 

1.创建类，定义数组、list、map、set 类型属性，生成对应 set 方法

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
    public void test(){
        System.out.println(Arrays.toString(courses));
        System.out.println(list);
        System.out.println(maps);
        System.out.println(sets);
    }
}
```

2.在 spring 配置文件进行配置

```xml
<!--集合类型属性注入--->
<bean  id="stu" class="com.spring5.collectiontype.Stu">
    <!--数组类型注入-->
    <property name="courses">
        <array>
            <value>Java课程</value>
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

测试方法

```java
public class TestSpring5Demo1{
    @Test
    public void testCollection(){
        ApplicationContext context =
            new ClassPathXmlApplicationContext("bean1.xml");
        Stu stu = context.getBean("stu", Stu.class);
        stu.test();
    }
}
```

##### 2.在集合里面设置对象类型值

Course.java

```java
public class Course{
    private String cname;//课程名称
    public void setCname(String cname){
        this.cname = cname;
    }
}
```

在Students类中进行相应的设置

```java
private List<Course> courseList;

public void setCourseList(List<Course> courseList){
    this.courseList = courseLIst;
}
```

```xml
<!--创建多个 course 对象-->
<bean id="course1" class="com.spring5.collectiontype.Course">
    <property name="cname" value="Spring5 框架"></property>
</bean>
<bean id="course2" class="com.spring5.collectiontype.Course">
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

##### 3.把集合注入部分提取出来成为公共数据

（1）在 spring 配置文件中引入名称空间 util

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
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

```java
@Test
public void testCollection2(){
    ApplicationContext context = new ClassPathApplicationContext("bean2.xml");
    Book book = context.getBean("book", Book.class);
    book.test();
}

public class Book{
    private List<String> list;
    public void setList(List<String> list){
    	this.list = list;    
    }
    
    public void test(){
        System.out.println(list);
    }
}
```

#### IOC操作Bean管理（FactoryBean）

1、Spring 有两种类型 bean：一是普通 bean，二是工厂 bean（FactoryBean）

* 普通 bean：在配置文件中定义 bean 类型就是返回类型

* 工厂 bean：在配置文件定义 bean 类型可以和返回类型不一样

##### 如何创建工厂bean

1. 创建类，让这个类作为工厂 bean，实现接口 FactoryBean 

2. 实现接口里面的方法，在实现的方法中定义返回的 bean 类型

```java
public class MyBean implements FactoryBean<Course>{
    //定义返回bean
    @Override
    public Course getObject() throws Exception{
        Course course = new Course();
        course.setCname("abc");
        //在Mybean中返回course
		return course;
    }
    @Override
     public Class<?> getObjectType() {
         return null;
     }
     @Override
     public boolean isSingleton() {
         return false;
     }
}
```

```xml
<bean id="myBean"class="com.spring5.factorybean.MyBean"></bean>                   
```

```java
@Test
public void test3() {
    ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");
    Course course = context.getBean("myBean", Course.class);
    System.out.println(course);
    }

```

##### Spring5中普通bean和工厂bean有什么异同？

* 普通bean是直接创建并初始化的，而工厂bean是通过一个工厂方法来创建实例；
* 普通bean可以通过依赖注入方式获取实例，而工厂bean需要通过getBean()方法显式地获取实例；
* 普通bean在IOC容器启动时就会被创建，而工厂bean只有在调用getBean()方法时才会实例化；
* 工厂bean可以在创建实例之前执行一些定制的逻辑或者根据不同条件返回不同的实例；

#### IOC操作Bean管理（bean作用域）

##### 在 Spring 里面，设置创建 bean 实例是单实例还是多实例？

在 Spring 里面，默认情况下，bean 是单实例对象

```java
@Test
public void testCollection2(){
    ApplicationContext xontext = new ClassPathXmlApplcationContext("bean2.xml");
    Book book1 = context.getBean("book", Book.class);
    Book book2 = context.getBean("book", Book.class);
    
    //book.test()
    System.out.println(book1);
    System.out.println(book2);
}
```

```markdown
在上面的代码块中的两个输出可得如下结果：
com.spring5.collectiontype.Book@5d11346a
com.spring5.collectiontype.Book@5d11346a
可知，在输出book1或是book2，它们的对象地址是一致的
```

##### 如何设置单实例还是多实例？

在 spring 配置文件 bean 标签里面有属性**（scope）**用于设置单实例还是多实例

scope 属性值常用有两个值：

1. 默认值，singleton，表示是单实例对象 

2. prototype，表示是多实例对象

```xml
<bean id="book" class="com.spring5.collectiontype.Book" scope="prototype">
    <property name="list" ref="bookList"></property>
</bean>
```

```markdown
com.spring5.collectiontype.Book@5d11346a
com.spring5.collectiontype.Book@7a36aefa
二者地址不同
```

##### singleton 和 prototype 区别

* singleton 为单实例，prototype 为多实例 

* 设置 scope 值是 singleton 时候，加载 spring 配置文件时候就会创建单实例对象 

* 设置 scope 值是 prototype 时候，不是在加载 spring 配置文件时候创建对象，在==调用 getBean 方法==时候创建多实例对象
* 当bean的scope属性值为singleton时，Spring IoC容器只会创建一个==共享的实例==，并在需要时将该实例分配给所有请求它的对象。这意味着无论在应用程序的任何地方请求该bean，都会接收到同一实例的引用。
* scope属性值为prototype时，每次从Spring IoC容器中请求该bean时，都会创建一个==新的实例==。这意味着每个请求都获得一个独立的bean实例。

> 因此，如果您需要在整个应用程序中共享一个bean实例，则应使用singleton作为scope属性值；如果您需要每次请求时创建一个新的bean实例，则应使用prototype作为scope属性值。

#### IOC操作Bean管理（bean生命周期）

生命周期 

> 从对象创建到对象销毁的过程

##### bean 生命周期

1. 通过构造器创建 bean 实例（无参数构造） 

2. 为 bean 的属性设置值和对其他 bean 引用（调用 set 方法） 

3. 调用 bean 的初始化的方法（需要进行配置初始化的方法） 

4. bean 可以使用了（对象获取到了） 

5. 当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）

演示 bean 生命周期

```java
public class Orders {
    //无参数构造
    public Orders() {
        System.out.println("第一步 执行无参数构造创建 bean 实例");
    }
    private String oname;
    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步 调用 set 方法设置属性值");
    }
    //创建执行的初始化的方法
    public void initMethod() {
        System.out.println("第三步 执行初始化的方法");
    }
    //创建执行的销毁的方法
    public void destroyMethod() {
        System.out.println("第五步 执行销毁的方法");
    }
}
```

`init-method="initMethod"（初始化方法名）`

`destory-method="destroyMethod"（销毁f'fa）`

```xml
<bean id="orders" class="com.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">
    <property name="oname" value="手机"></property>
</bean>
```

```java
@Test
public void testBean3() {
    // ApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");
    //ClassPathXmlApplicationContext 可以让使用close方法是不需要强转
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");
    Orders orders = context.getBean("orders", Orders.class);
    System.out.println("第四步 获取创建 bean 实例对象");
    System.out.println(orders);
    
    //在String生命周期中需要执行close方法手动销毁，并会执行相应的配置的销毁方法
    //手动让 bean 实例销毁
    context.close();
}
```

```markdown
第一步	执行无参数构造创建bean实例
第二步	调用set方法设置属性值
第三步	执行初始化的方法
第四部	获取创建bean实例对象
第五步	执行销毁的方法
```

##### bean的后置处理器

bean生命周期有七步

1. 通过构造器创建 bean 实例（无参数构造） 

2. 为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）

3. 把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization ()

4. 调用 bean 的初始化的方法（需要进行配置初始化的方法）

5. 把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization()

6. bean 可以使用了（对象获取到了）

7. 当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）

演示添加后置处理器效果

（1）创建类，实现接口 BeanPostProcessor，创建后置处理器

```java
public class MyBeanPost implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) 
        throws BeansException {
        System.out.println("在初始化之前执行的方法");
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) 
        throws BeansException {
        System.out.println("在初始化之后执行的方法");
        return bean;
    }
}
```

```xml
<!--配置后置处理器-->
<bean id="myBeanPost" class="com.spring5.bean.MyBeanPost"></bean>
```

```markdown
第一步	执行无参数构造创建bean实例
第二步	调用set方法设置属性值
**在初始化之前执行的方法**
第三步	执行初始化的方法
**在初始化之后执行的方法**
第四部	获取创建bean实例对象
第五步	执行销毁的方法
```

bean后置处理器的作用

> Bean后置处理器在Spring框架中的作用是允许开发人员在容器实例化、配置和初始化bean之前或之后，对bean进行自定义修改或操作。这为开发人员提供了一种机制来解决各种问题，例如添加额外的初始化逻辑、修改属性值等。通过Bean后置处理器，开发人员可以灵活而方便地扩展Spring框架的功能，同时也可以实现自己的定制需求。

#### IOC 操作 Bean 管理（xml 自动装配）

##### 什么是自动装配 

> 根据指定装配规则（属性名称或者属性类型），Spring 自动将匹配的属性值进行注入

##### 演示自动装配过程

```java
//autowire软件包中的Emp.java
public class Emp{
    private Dept dept;
    public void setDept(Dept dept){
        this.dept = dept;
    }
    
    @Override
    public String toString(){
        return "Emp{ + 
            	"dept" + dept +
    			"}";
    }
    
    public void test(){
        System.out.println(dept);
    }
}

//autowire软件包中的Dept.java
public class Dept{
    @Override
    public String toString(){
        return "Dept{}";
    }
}
```

##### 1.根据属性名称自动注入

```xml
<!--实现自动装配
    bean 标签属性 autowire，配置自动装配
    autowire 属性常用两个值：
    byName 根据属性名称注入 ，注入值 bean 的 id 值和类属性名称一样
    byType 根据属性类型注入
-->
<bean id="emp" class="com.spring5.autowire.Emp" autowire="byName">
<!--<property name="dept" ref="dept"></property>-->
</bean>
<bean id="dept" class="com.spring5.autowire.Dept"></bean>
```

##### 2.根据属性类型自动注入

```xml
<!--实现自动装配
    bean 标签属性 autowire，配置自动装配
    autowire 属性常用两个值：
    byName 根据属性名称注入 ，注入值 bean 的 id 值和类属性名称一样
    byType 根据属性类型注入
-->
<bean id="emp" class="com.spring5.autowire.Emp" autowire="byType">
<!--<property name="dept" ref="dept"></property>-->
</bean>
<bean id="dept" class="com.spring5.autowire.Dept"></bean>
```

#### IOC操作Bean管理（外部属性文件）

##### 直接配置数据库信息 

1. 配置德鲁伊连接池 

2. 引入德鲁伊连接池依赖 jar 包

```xml
<!--直接配置连接池-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/userDb"></property>
    <property name="username" value="root"></property>
    <property name="password" value="root"></property>
</bean>
```

##### 引入外部属性文件配置数据库连接池 

1. 创建外部属性文件，properties 格式文件，写数据库信息

```properties
//jdbc.properties

prop.driverClass=com.mysql.jdbc.Driver
prop.url=jdbc:mysql://localhost:3306/userDb
prop.userName=root 
prop.password=root
```

2. 把外部properties属性文件引入到spring配置文件中

引入context名称空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```

3. 在spring配置文件使用标签引入外部属性标签

```xml
<!--引入外部属性文件-->
<!--context:property-placeholder是Spring框架中一个XML命名空间的元素，用于加载属性文件中的属性到Spring容器中，以供其他组件引用-->
<context:property-placeholder location="classpath:jdbc.properties"/>

<!--配置连接池-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${prop.driverClass}"></property>
    <property name="url" value="${prop.url}"></property>
    <property name="username" value="${prop.userName}"></property>
    <property name="password" value="${prop.password}"></property>
</bean>
```

#### IOC操作Bean管理（基于注解方式）

##### 什么是注解

* 注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值...) 

* 使用注解，注解作用在类上面，方法上面，属性上面 

* 使用注解目的：简化 xml 配置

##### Spring 针对 Bean 管理中创建对象提供注解

* @Component 

@Component是最普通的注解，可以用于标注任意类，表示这个类会被Spring容器扫描并纳入管理

* @Service

@Service通常用于标注服务层组件，表示这个类是一个服务层组件，它通常包含了一些业务逻辑

* @Controller 

@Controller通常用于标注控制层组件，表示这个类是一个控制层组件，它通常包含了一些处理请求的方法。

* @Repository 

@Repository通常用于标注数据访问层组件，表示这个类是一个数据访问层组件，它通常包含了一些数据访问相关的方法。

上面四个注解功能是一样的，都可以用来创建 bean 实例

##### 基于注解方式实现对象创建

第一步 引入依赖

```jar
spring-aop-5.2.6.RELEASE.jar
```

第二步 开启组件扫描

```xml
<!--开启组件扫描
    1 如果扫描多个包，多个包使用逗号隔开
    2 扫描包上层目录
-->
<context:component-scan base-package="com.spring5"></context:component-scan>
```

第三步 创建类，在类上面添加创建对象注解

```java
//在注解里面 value 属性值可以省略不写，
//默认值是类名称，首字母小写
//UserService -- userService
@Component(value = "userService") //<bean id="userService" class=".."/>
    public class UserService {
        public void add() {
        System.out.println("service add.......");
    }
}
```

##### 开启组件扫描细节配置

```xml
<!--示例 1
    use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
	use-default-filters是一个属性，它用于指示Spring是否应该使用默认过滤器来扫描组件
    context:include-filter ，设置扫描哪些内容
	type="annotation"，按注解类型包含组件
	不再扫描软件包中全部的类，而是扫描带有特定注解的类（Controller）
-->
<context:component-scan base-package="com.spring5 use-default-filters="false" ">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

```xml
<!--示例 2
    下面配置扫描包所有内容
    context:exclude-filter： 设置哪些内容不进行扫描
-->
<context:component-scan base-package="com.atguigu">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

##### 基于注解方式实现属性注入

###### @Autowired：根据属性类型进行自动装配 

> 对于 `final` 属性，Spring 无法使用 setter 方法或者直接赋值的方式来注入依赖。

第一步 把 service 和 dao 对象创建，在 service 和 dao 类添加创建对象注解 

第二步 在 service 注入 dao 对象，在 service 类添加 dao 类型属性，在属性上面使用注解

```java
//DAO包下
public interface UserDao{
    public void add();
}

public class UserDaoImpl implements UserDao{
    @Override
    public void add(){
        System.out.println("dao add");
    }
}
```

```java
@Service
public class UserService {
    //定义 dao 类型属性
    //不需要添加 set 方法
    //添加注入属性注解
    @Autowired 
    private UserDao userDao;
    
    public void add() {
        System.out.println("service add.......");
        userDao.add();
    }
}
```

###### @Qualifier：根据属性名称进行注入 

这个@Qualifier 注解的使用，和上面@Autowired 一起使用

> `@Qualifier` 注解用于解决 Spring 自动装配中的歧义性问题，它配合 `@Autowired` 注解使用，可以指定要注入的 bean 的名称或 ID。

> 当一个接口有多个实现类时，使用 `@Autowired` 注解注入接口类型的 bean 时，Spring 无法判断应该注入哪个实现类的 bean，此时就会抛出 NoUniqueBeanDefinitionException 异常。为了解决这个问题，可以使用 `@Qualifier` 注解指定要注入的 bean 的名称或 ID。

```java
//定义 dao 类型属性
//不需要添加 set 方法
//添加注入属性注解
@Autowired //根据类型进行注入
@Qualifier(value = "userDaoImpl1") //根据名称进行注入
private UserDao userDao;
```

###### @Resource：可以根据类型注入，可以根据名称注入

> `@Resource` 注解可以用于注入资源，如 JNDI 对象、数据源等，也可以用于注入其他 Spring 管理的 bean。当用于注入 bean 时，它可以指定要注入的 bean 的名称或 ID。

```java
//@Resource //根据类型进行注入
@Resource(name = "userDaoImpl1") //根据名称进行注入
private UserDao userDao;
```

###### @Value：注入普通属性

```java
@Value(value = "abc")
private String name;
```

##### 完全注解开发 

0. Spring 5中完全注解开发主要有以下几个步骤：

1. 在Spring配置类上添加@Configuration注解，标识该类为Spring配置类；
2. 使用@ComponentScan注解扫描需要被Spring管理的Bean，可以指定扫描路径；
3. 使用@Bean注解定义Bean，可以指定Bean名称；
4. 使用@Autowired注解注入Bean，可以在构造函数、Setter方法或字段上使用；
5. 使用@Value注解注入属性值，可以在属性上使用；
6. 使用@Qualifier注解指定注入的Bean名称，用于解决多个同类型Bean注入的问题；
7. 使用@Scope注解指定Bean的作用域，可以是singleton或prototype。

1. 创建配置类，替代 xml 配置文件

```java
@Configuration //作为配置类，替代 xml 配置文件
@ComponentScan(basePackages = {"com.atguigu"})
public class SpringConfig {
}
```

2. 编写测试类

```java
@Test
public void testService2() {
    //加载配置类
    ApplicationContext context
    = new AnnotationConfigApplicationContext(SpringConfig.class);
    UserService userService = context.getBean("userService", 
    UserService.class);
    System.out.println(userService);
    userService.add();
}

```



[返回文首](#IOC容器)

