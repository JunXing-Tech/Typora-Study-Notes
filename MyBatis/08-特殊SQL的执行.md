# 特殊SQL的执行

[toc]

#### 1、模糊查询

```java
/**
 * 根据用户名模糊查询用户信息
 */
List<User> getUserByLike(@Param("username") String username);
```

```xml
<select id="getUserByLike" resultType="User">
    <!--select * from t_user where username like '%${username}%';-->
    <!--select * from t_user where username like 'concat('%',#{username},'%')';-->
    select * from t_user where username like "%"#{username}"%";
</select>
```

```java
@Test
public void testGetUserByLike(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    List<User> userByLike = mapper.getUserByLike("a");
    System.out.println(userByLike);
}
```

#### 2、批量删除

```java
/**
* 批量删除
* @param ids
* @return
*/
int deleteMore(@Param("ids") String ids);
```

```java
<!--int deleteMore(@Param("ids") String ids);-->
<delete id="deleteMore">
    <!--只能使用${}-->
    delete from t_user where id in (${ids});
</delete>
```

```java
@Test
public void testDeleteMore(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    int result = mapper.deleteMore("1, 2, 3");
    System.out.println(result);
}
```

#### 3、动态设置表名 

```java
/**
* 动态设置表名，查询所有的用户信息
* @param tableName
* @return
*/
List<User> getUserByTableName(@Param("tableName") String tableName);
```

```xml
<select id="getUserByTableName" resultType="User">
    <!--表名不能加单引号，所以不能用#{}, 而是应该使用${}-->
    select * from ${tableName};
</select>
```

```java
@Test
public void testGetUserByTableName(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    List<User> list = mapper.getUserByTableName("t_user");
    for(User user : list){
        System.out.println(user);
    }
}
```

#### 4、添加功能获取自增的主键

t_clazz(clazz_id,clazz_name)

t_student(student_id,student_name,clazz_id)

1、添加班级信息 

2、获取新添加的班级的id

3、为班级分配学生，即将某学的班级id修改为新添加的班级的id

```java
/**
* 添加用户信息
* @param user
* @return
* useGeneratedKeys：设置当前标签中的sql使用了自增的主键
* keyProperty：因为增删改有统一的返回值是受影响的行数，因此只能将获取的自增的主键放在传输的参数user对象的某个属性中（将自增的主键的值赋给传输到映射文件中参数的某个属性）
*/
void insertUser(User user);
```

```xml
<!--int insertUser(User user);-->
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
    insert into t_user values(null,#{username},#{password},#{age},#{sex}, #{email});
</insert>
```

```java
@Test
public void testInsertUser(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    User user = new User(null, "王五", "123", 23, "男", "12345@qq.com");
    mapper.insertUser(user);
    System.out.println(user.getId());
    System.out.println(user);
}
```

