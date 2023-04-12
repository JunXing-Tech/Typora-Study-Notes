## AOP

---

[toc]

#### AOP（概念）

##### 什么是 AOP

（1）面向切面编程（方面），利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得 业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。 

（2）通俗描述：不通过修改源代码方式，在主干功能里面添加新功能 

（3）使用登录例子说明 AOP

<img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230407082730119.png" alt="image-20230407082730119" style="zoom:50%;" />

#### AOP（底层原理）

##### AOP 底层使用动态代理 

有两种情况动态代理 ：

1. 有接口情况，使用 JDK 动态代理 

创建接口实现类代理对象，增强类的方法

<img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230407092330946.png" alt="image-20230407092330946" style="zoom: 67%;" />

2. 没有接口情况，使用 CGLIB 动态代理 

创建子类的代理对象，增强类的方法

<img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230407092400209.png" alt="image-20230407092400209" style="zoom:67%;" />

##### AOP(JDK动态代理)

1、使用 JDK 动态代理，使用 Proxy 类里面的方法创建代理对象

```java\
java.lang.reflect
Class Proxy

java.lang.Object
	java.lang.reflect.Proxy
```

（1）调用 newProxyInstance 方法

```markdown
static Object		newProxyInstance(ClassLoader loader, 类<?>[] interface, InvocationHandler h)
					返回指定接口的代理类的实例，该接口将方法调用分派给指定的调用处理程序	
```

方法有三个参数： 

第一参数，类加载器 

第二参数，增强方法所在的类，这个类实现的接口，支持多个接口 

第三参数，实现这个接口 InvocationHandler，创建代理对象，写增强的方法

2、编写 JDK 动态代理代码

（1）创建接口，定义方法

```java
public interface UserDao {
    public int add(int a,int b);
    public String update(String id);
}
```

（2）创建接口实现类，实现方法

```java
public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        return a+b;
    }
    @Override
    public String update(String id) {
        return id;
    }
}

```

（3）使用 Proxy 类创建接口代理对象

```java
public class JDKProxy {
    public static void main(String[] args) {
        //创建接口实现类代理对象
        Class[] interfaces = {UserDao.class};
        //匿名内部类
        // Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new InvocationHandler() {
        // @Override
        // public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // return null;
        // }
        // });
        UserDaoImpl userDao = new UserDaoImpl();
        UserDao dao = (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
        int result = dao.add(1, 2);
        System.out.println("result:"+result);
    }
}

//创建代理对象代码
class UserDaoProxy implements InvocationHandler {
    //1 把创建的是谁的代理对象，把谁传递过来
    //有参数构造传递
    private Object obj;
    public UserDaoProxy(Object obj) {
        this.obj = obj;
    }
    //增强的逻辑
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //方法之前
        System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ Arrays.toString(args));
        //被增强的方法执行
        Object res = method.invoke(obj, args);
        //方法之后
        System.out.println("方法之后执行...."+obj);
        return res;
    }
}
```

#### AOP（操作术语）

1.连接点

类里面哪些方法可以被增强，这些方法称为连接点

2.切入点

实际被真正增强的方法，称为切入点

3.通知（增强）

（1）实际增强的逻辑部分称为通知（增强）

（2）通知有多种类型

* 前置通知
* 后置通知
* 环绕通知
* 异常通知
* 最终通知 类似 try / catch / finally

4.切面

是动作

（1）把通知应用到切入点过程

#### AOP 操作（准备工作）

1、Spring 框架一般都是基于 AspectJ 实现 AOP 操作 

（1）什么是AspectJ

AspectJ 不是 Spring 组成部分，独立 AOP 框架，一般把 AspectJ 和 Spirng 框架一起使 用，进行 AOP 操作

2、基于 AspectJ 实现 AOP 操作

（1）基于 xml 配置文件实现 

（2）基于注解方式实现（使用）

3、在项目工程里面引入 AOP 相关依赖

```jar
com.springsource.net.sf.cglib-2.2.0.jar
com.springsource.org.aopalliance-1.0.0.jar
com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
//以下两个jar包根据版本不同，有所不同
spring-aop-5.2.6.jar
spring-aspects-5.2.6.jar
```

4、切入点表达式

（1）切入点表达式作用：知道对哪个类里面的哪个方法进行增强 

（2）语法结构： `execution([权限修饰符] [返回类型] [类全路径] [方法名称]([参数列表]) )`

举例 1：对 com.atguigu.dao.BookDao 类 里面的 add 进行增强`execution(* com.atguigu.dao.BookDao.add(..))`

举例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强 `execution(* com.atguigu.dao.BookDao.* (..))`

举例 3：对 com.atguigu.dao 包里面所有类，类里面所有方法进行增强 `execution(* com.atguigu.dao.*.* (..))`

#### AOP 操作（AspectJ 注解）

1、创建类，在类里面定义方法

```java
public class User {
    public void add() {
        System.out.println("add...");
    }
}
```

2、创建增强类（编写增强逻辑）

```java
（1）在增强类里面，创建方法，让不同方法代表不同通知类型
//增强的类
public class UserProxy {
    public void before() {//前置通知
        System.out.println("before...");
    }
}
```

3、进行通知的配置

（1）在 spring 配置文件中，开启注解扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xmlns:context="http://www.springframework.org/schema/context" 
xmlns:aop="http://www.springframework.org/schema/aop" 
xsi:schemaLocation="http://www.springframework.org/schema/beans 
                    http://www.springframework.org/schema/beans/spring-beans.xsd 
                    http://www.springframework.org/schema/context 
                    http://www.springframework.org/schema/context/spring-context.xsd 
                    http://www.springframework.org/schema/aop 
                    http://www.springframework.org/schema/aop/spring-aop.xsd">
 <!-- 开启注解扫描 -->
<context:component-scan base-package="com.spring5.aopanno"></context:component-scan>
</beans>
```

（2）使用注解创建 User 和 UserProxy 对象

```java
//被增强的类
@Component
public class User
    
//增强的类
public class UserProxy
```

（3）在增强类上面添加注解 @Aspect

```java
//增强的类
@Component
@Aspect	//生成代理对象
public class UserProxy
```

（4）在 spring 配置文件中开启生成代理对象

```xml
<!-- 开启 Aspect 生成代理对象-->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

4、配置不同类型的通知

（1）在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置

```java
//增强的类
@Component
@Aspect //生成代理对象
public class UserProxy {
    //前置通知
    //@Before 注解表示作为前置通知
    @Before(value = "execution(* com.spring5.aopanno.User.add(..))")
    public void before() {
        System.out.println("before.........");
    }
    //后置通知（返回通知） --- 在返回值之后执行 + 有异常不执行
    @AfterReturning(value = "execution(* com.spring5.aopanno.User.add(..))")
    public void afterReturning() {
        System.out.println("afterReturning.........");
    }
    //最终通知 --- 在方法之后执行 + 无论是否有异常都执行
    @After(value = "execution(* com.spring5.aopanno.User.add(..))")
    public void after() {
        System.out.println("after.........");
    }
    //异常通知
    @AfterThrowing(value = "execution(* com.spring5.aopanno.User.add(..))")
    public void afterThrowing() {
        System.out.println("afterThrowing.........");
    }
    //环绕通知
    @Around(value = "execution(* com.spring5.aopanno.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("环绕之前.........");
    //被增强的方法执行
    proceedingJoinPoint.proceed();
    System.out.println("环绕之后.........");
    }
}
```

```java
public class TestAop{
    @Test
    public void testAopAnno(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
        User user = context.getBean("user", User.class);
        user.add();
    }
}
```



5、相同的切入点抽取

```java
//相同切入点抽取
@Pointcut(value = "execution(* com.spring5.aopanno.User.add(..))")
public void pointdemo() {
}
//前置通知
//@Before 注解表示作为前置通知
@Before(value = "pointdemo()")
public void before() {
    System.out.println("before...");
}
```

6、有多个增强类多同一个方法进行增强，设置增强类优先级

（1）在增强类上面添加注解 @Order(数字类型值)，数字类型值越小优先级越高

```java
@Component
@Aspect
@Order(1)
public class PersonProxy
```

7、完全使用注解开发 

（1）创建配置类，不需要创建 xml 配置文件

```java
@Configuration 
//开启组件扫描
@ComponentScan(basePackages = {"com.atguigu"}) 
//开启Aspect生成代理对象
@EnableAspectJAutoProxy(proxyTargetClass = true) 
public class ConfigAop { 
}
```

#### AOP 操作（AspectJ 配置文件）

了解即可

1、创建两个类，增强类和被增强类，创建方法

```java
//被增强类
public class Book{
    public void buy(){
        System.out.println("buy...");
    }
}
//增强类
public class BookProxy{
    public void before(){
        System.out.println("before...");
    }
}
```

2、在 spring 配置文件中创建两个类对象

3、在 spring 配置文件中配置切入点

```xml
<!--创建对象-->
<bean id="book" class="com.spring5.aopxml.Book"></bean>
<bean id="bookProxy" class="com.spring5.aopxml.BookProxy"></bean>

<!--配置 aop 增强-->
<aop:config>
    <!--切入点-->
    <aop:pointcut id="p" expression="execution(* com.spring5.aopxml.Book.buy(..))"/>
    <!--配置切面-->
    <!--ref="增强类"-->
    <aop:aspect ref="bookProxy">
    <!--增强作用在具体的方法上-->
        <!--method="增强类中的具体方法名"-->
        <aop:before method="before" pointcut-ref="p"/>
    </aop:aspect>
</aop:config>
```

[返回文首](#AOP)
