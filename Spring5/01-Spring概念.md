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