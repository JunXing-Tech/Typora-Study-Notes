## Spring概念

---

[toc]

#### Spring5框架概述

1、Spring 是轻量级的开源的 JavaEE 框架 

2、Spring 可以解决企业应用开发的复杂性 

3、Spring 有两个核心部分：IOC 和 Aop 

1. IOC：控制反转，把创建对象过程交给 Spring 进行管理 

2. Aop：面向切面，不修改源代码进行功能增强 

4、Spring 特点 

1. 方便解耦，简化开发 
2. Aop 编程支持 
3. 方便程序测试
4. 方便和其他框架进行整合
5. 方便进行事务操作
6. 降低 API 开发难度

5.bean.xml

> bean.xml是一个配置文件，用于定义Spring框架中的JavaBean（对象）及其依赖关系。它描述了应用程序中的组件之间如何协作，并提供了一种实现松散耦合的机制，以便更轻松地更改应用程序的行为。通过bean.xml文件中的配置，Spring容器可以管理，创建和注入这些组件，从而使整个应用程序能够运行。

6.bean.xml中的id值的作用

> 在 Spring 框架中，bean.xml 文件的 id 值用于标识每个 bean 对象的唯一性。可以使用该值在代码中获取特定的 bean 对象实例，也可以将其用作其他 bean 的依赖项。
>
> 例如，通过在 bean.xml 中为一个 bean 指定一个 id 值，在 Java 代码中可以使用 ApplicationContext.getBean("id") 方法获取该对象的实例。
>
> 同时，id 值也用于在 XML 配置文件中引用其他的 bean 对象，如使用 ref 属性指定另一个 bean 对象的 id 作为当前 bean 的依赖项

#### 入门开发案例

1.下载Spring5

2.创建Java工程

3.导入Spring5相关jar包

4.在Java工程中，创建普通类并在类中创建普通方法

5.在**src**下创建Spring配置文件，在配置文件中配置创建的对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 xsi:schemaLocation="http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans.xsd">
     <!--配置 User 对象创建--> 
     <bean id="User" class="com.Spring5.类名"></bean>
</beans>
```

6.编写测试代码

```java
	@Test
    public  void testAdd(){
        //1.加载Spring配置文件，Spring配置文件名
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
        //2.获取配置创建的对象，("xml中的id", 类名.class)
        User user = context.getBean("user", User.class);
        System.out.println(user);
        user.add();
    }
```

7.运行即可