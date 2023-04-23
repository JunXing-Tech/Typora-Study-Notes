# MyBatis的各种查询功能

[toc]

#### 1、查询一个实体类对象

```java
/**
* 根据用户id查询用户信息
* @param id
* @return
*/
User getUserById(@Param("id") int id);
```

```xml
<!--User getUserById(@Param("id") int id);-->
<select id="getUserById" resultType="User">
    select * from t_user where id = #{id};
</select>
```

#### 2、查询一个list集合

```java
/**
* 查询所有用户信息
* @return
*/
List<User> getUserList();
```

```xml
<!--List<User> getUserList();-->
<select id="getUserList" resultType="User">
    select * from t_user;
</select>
```

```java
@Test
public void testGetAllUser(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
    for(User user : mapper.getUserList()){
        System.out.println(user);
    }
}
```

#### 3、查询单个数据

```java
/**
* 查询用户的总记录数
* @return
* 在MyBatis中，对于Java中常用的类型都设置了类型别名
* 例如：java.lang.Integer-->int|integer
* 例如：int-->_int|_integer
* 例如：Map-->map,List-->list
*/
Integer getCount();
```

```xml
<!--int getCount();-->
<select id="getCount" resultType="_integer">
    select count(id) from t_user;
</select>
```

#### 4、查询一条数据为map集合

```java
/**
 * 根据用户id查询用户信息为map集合
 * @param id
 * @return
 */
Map<String, Object> getUserByIdToMap(@Param("id") Integer id);
```

```xml
<select id="getUserByIdToMap" resultType="map">
    select * from t_user where id = #{id};
</select>
```

```java
@Test
public void testGetUserByIdMap(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
    System.out.println(mapper.getUserByIdToMap(3));
    }
```



#### 5、查询多条数据为map集合

##### 方式一

```java
/**
* 查询所有用户信息为map集合
* @return
* 将表中的数据以map集合的方式查询，一条数据对应一个map；若有多条数据，就会产生多个map集合，此
时可以将这些map放在一个list集合中获取
*/
List<Map<String, Object>> getAllUserToMap();
```

```xml
<!--Map<String, Object> getAllUserToMap();-->
<select id="getAllUserToMap" resultType="map">
    select * from t_user;
</select>
```

##### 方式二

```java
/**
* 查询所有用户信息为map集合
* @return
* 将表中的数据以map集合的方式查询，一条数据对应一个map；若有多条数据，就会产生多个map集合，并
且最终要以一个map的方式返回数据，此时需要通过@MapKey注解设置map集合的键，值是每条数据所对应的
map集合
*/
@MapKey("id")
Map<String, Object> getAllUserToMap();
```

```xml
<!--Map<String, Object> getAllUserToMap();-->
<select id="getAllUserToMap" resultType="map">
    select * from t_user;
</select>
<!--
{
	结果：
    1={password=123456, sex=男, id=1, age=23, username=admin},
    2={password=123456, sex=男, id=2, age=23, username=张三},
    3={password=123456, sex=男, id=3, age=23, username=张三}
}
-->
```

#### 6、总结

```java
/**
 * MyBatis的各种查询功能:
 *  1.若查询出的数据只有一条
 *  a > 可以通过实体类对象接收
 *  b > 可以通过List集合接收
 *  c > 可以太过map集合接收
 *  2.若查询出的数据有多条，
 *  a > 可以通过实体类类型的List集合接收
 *  b > 可以通过map类型的List集合接收
 *  C > 可以在map接口的方法上添加@Mapkey注解，
 *      此时就可以将每条数据转换的map集合作为值，以某个字段的值作为键，放在同一个map集合中
 *  注意：一定不能通过实体类对象接收，此时会抛异常TooManyResultsException
 *
 *  MyBatis中设置了默认的类型别名
 *  java.lang,Integer --> int, integer
 *  int --> _int, _integer
 *  Map --> map
 *  String --> string
 */
```

