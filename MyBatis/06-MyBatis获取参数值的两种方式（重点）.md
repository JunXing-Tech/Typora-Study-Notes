# MyBatis获取参数值的两种方式（重点）

[toc]

MyBatis获取参数值的两种方式：`${}`和`#{} `

`${}`的本质就是字符串拼接，`#{}`的本质就是占位符赋值

` ${}`使用字符串拼接的方式拼接sql，若为字符串类型或日期类型的字段进行赋值时，需要手动加单引 号；

但是`#{}`使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，可以自 动添加单引号

==补充==：在获取参数为单个字面量类型或多个字面量类型是，可用@Param注解命名参数（1、2、5）

#### 1、单个字面量类型的参数

若mapper接口中的方法参数为单个的字面量类型 

此时可以使用`${}`和`#{}`以任意的名称获取参数的值，注意`${}`需要手动加单引号

```java
/**
 * 根据用户名查询用户信息
 */
User getUserByUsername(String username);
```

```xml
<select id="getUserByUsername" resultType="User">
    <!--select * from t_user where username = #{username};-->
    select * from t_user where username = '${username}';
</select>
```

```java
@Test
public void testGetUserByUsername(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    User user = mapper.getUserByUsername("admin");
    sqlSession.commit();
    System.out.println(user);
}
```

#### 2、多个字面量类型的参数

若mapper接口中的方法参数为多个时

此时MyBatis会自动将这些参数放在一个map集合中，以两种方式进行存储

1. 以arg0,arg1...为键，以参数为值
2. 以 param1,param2...为键，以参数为值

因此只需要通过`${}`和`#{}`访问map集合的键就可以获取相对应的 值，注意`${}`需要手动加单引号

```java
/**
 * 验证登录
 */
User checkLogin(String username, String password);
```

```xml
<select id="checkLogin" resultType="User">
    select * from t_user where username = #{param1} and password = #{param2};
    <!--select * from t_user where username = ${arg0} and password = ${arg1};-->
</select>
```

```java
@Test
public void testCheckLogin(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    User user = mapper.checkLogin("admin", "123456");
    sqlSession.commit();
    System.out.println(user);
}
```

#### 3、map集合类型的参数

若mapper接口中的方法需要的参数为多个时，此时可以手动创建map集合，将这些数据放在map中 只需要通过`${}`和`#{}`访问map集合的键就可以获取相对应的值，注意`${}`需要手动加单引号

```java
/**
 * 验证登录（参数为Map）
 */
User checkLoginByMap(Map<String, Object> map);
```

```java
@Test
public void testCheckLoginByMap(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    Map<String, Object> map = new HashMap<>();
    map.put("username", "admin");
    map.put("password", "123456");
    User user = mapper.checkLoginByMap(map);
    System.out.println(user);
}
```

```xml
<select id="checkLoginByMap" resultType="User">
    select * from t_user where username = #{username} and password = #{password};
</select>
```

#### 4、实体类类型的参数

若mapper接口中的方法参数为实体类对象时 此时可以使用`${}`和`#{}`，通过访问实体类对象中的属性名获取属性值，注意`${}`需要手动加单引号

```java
    /**
     * 添加用户信息
     */
    int insertUser(User user);
```

```xml
    <insert id="insertUser">
        insert into t_user values (null, #{username}, #{password}, #{age}, #{sex}, #{email});
    </insert>
```

```java
    @Test
    public void testInsertUser(){
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
        int result = mapper.insertUser(new User(null, "李四", "123", 23, "男", "123@qq.com"));
        System.out.println(result);
    }
```

#### 5、使用@Param标识参数

可以通过@Param注解标识mapper接口中的方法参数 

此时，会将这些参数放在map集合中，

1. 以@Param注解的value属性值为键，以参数为值；

2. 以 param1,param2...为键，以参数为值；

只需要通过`${}`和`#{}`访问map集合的键就可以获取相对应的值， 注意`${}`需要手动加单引号

```Java
/**
 * 验证登录（使用@Param）
 */
User checkLoginByParam(@Param("username") String username, @Param("password") String password);
```

```xml
    <select id="checkLoginByParam" resultType="User">
        select * from t_user where username = #{username} and password = #{password};
    </select>
```

```java
    @Test
    public void testCheckLoginByParam(){
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
        User user = mapper.checkLoginByParam("admin", "123456");
        System.out.println(user);
    }
```
