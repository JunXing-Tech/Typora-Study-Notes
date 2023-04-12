## JUnit5单元测试框架

---

[toc]

5、Spring5 支持整合 JUnit5

（1）整合 JUnit4

第一步 引入 Spring 相关针对测试依赖

```jar
hamcrest-core-13.jar
junit-4.12.jar
```

第二步 创建测试类，使用注解方式完成

```java
@RunWith(SpringJUnit4ClassRunner.class) //单元测试框架
@ContextConfiguration("classpath:bean1.xml") //加载配置文件
public class JTest4 {
    @Autowired
    private UserService userService;
    @Test
    public void test1() {
        userService.accountMoney();
    }
}

```

（2）Spring5 整合 JUnit5

第一步 引入 JUnit5 的 jar 包

```markdown
Add 'JUnit5.3' to classPath
```

第二步 创建测试类，使用注解完成

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration("classpath:bean1.xml")
public class JTest5 {
    @Autowired
    private UserService userService;
    @Test
    public void test1() {
        userService.accountMoney();
    }
}

```

（3）使用一个复合注解替代上面两个注解完成整合

```java
SpringJUnitConfig(locations = "classpath:bean1.xml")
public class JTest5 {
    @Autowired
    private UserService userService;
    @Test
    public void test1() {
        userService.accountMoney();
    }
}
```

