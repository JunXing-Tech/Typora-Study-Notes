## Nullable注解和函数式注册对象

---

[toc]

（1）@Nullable 注解可以使用在方法上面，属性上面，参数上面，表示方法返回可以为空，属性值可以 为空，参数值可以为空 

（2）注解用在方法上面，方法返回值可以为空

```java
@Nullable
String getId();
```

（3）注解使用在方法参数里面，方法参数可以为空

```java
public <T> void registerBean(@Nullable String beanName, ){
    this.reader.registerBean(beanClass, beanName, suppli)
}
```

（4）注解使用在属性上面，属性值可以为空

```java
@Nullable
private String bookName;
```

4、Spring5 核心容器支持函数式风格 GenericApplicationContext

```java
//函数式风格创建对象，交给 spring 进行管理
@Test
public void testGenericApplicationContext() {
    //1 创建 GenericApplicationContext 对象
    GenericApplicationContext context = new GenericApplicationContext();
    //2 调用 context 的方法对象注册
    context.refresh();
    context.registerBean("user1",User.class,() -> new User());
    //3 获取在 spring 注册的对象
    // User user = (User)context.getBean("com.spring5.test.User");
    User user = (User)context.getBean("user1");
    System.out.println(user);
}

```

