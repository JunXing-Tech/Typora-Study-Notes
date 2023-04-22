# MyBatis的增删改查

[toc]

#### 1、添加

```xml
<!--int insertUser();-->
<insert id="insertUser">
    insert into t_user values(null,'admin','123456',23,'男')
</insert>

```

#### 2、删除 

```xml
<!--int deleteUser();-->
<delete id="deleteUser">
    delete from t_user where id = 7
</delete>
```

#### 3、修改 

```java
/**
 * 修改用户名
 */
void updateUser();
```

```java
@Test
public void testUpdate() throws IOException{
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    mapper.updateUser();
}
```

```xml
<!--int updateUser();-->
<update id="updateUser">
    update t_user set username='张三' where id = 1
</update>
```

#### 4、查询一个实体类对象

```java
User getUserById();
```

```xml
<!--User getUserById();-->
<!--
    查询功能标签必须设置resultType或resultMap
    resultType: 设置默认的映射关系
    resultMap:  设置自定义的映射关系
-->
<select id="getUserById" resultType="com.mybatis.bean.User">
    select * from t_user where id = 2
</select>
```

```java
@Test
public void testCRUD() throws IOException{
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    //mapper.updateUser();
    User user = mapper.getUserById();
    System.out.println(user);
}
```

#### 5、查询集合

```java
List<User> getAllUser();
```

```xml
<!--List<User> getUserList();-->
<select id="getUserList" resultType="com.mybatis.pojo.User">
    select * from t_user
</select>

```

```java
@Test
public void testCRUD() throws IOException{
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    //mapper.updateUser();
    /*User user = mapper.getUserById();*/
    List<User> allUser = mapper.getAllUser();
    //System.out.println(allUser);

    //allUser.forEach(user -> System.out.println(user));

    for(User users : allUser){
        System.out.println(users);
    }
}
```

> 注意： 
>
> 1、查询的标签select必须设置属性resultType或resultMap，用于设置实体类和数据库表的映射关系 
>
> resultType：自动映射，用于属性名和表中字段名一致的情况 
>
> resultMap：自定义映射，用于一对多或多对一或字段名和属性名不一致的情况
>
> 2、当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常 
>
> TooManyResultsException；但是若查询的数据只有一条，可以使用实体类或集合作为返回值