# 基本CRUD

[toc]

#### 1、BaseMapper

MyBatis-Plus中的基本CRUD在内置的BaseMapper中都已得到了实现，我们可以直接使用，接口如下：

```java
package com.baomidou.mybatisplus.core.mapper;
public interface BaseMapper<T> extends Mapper<T> {
    /**
     * 插入一条记录
     * @param entity 实体对象
     */
    int insert(T entity);
    /**
     * 根据 ID 删除
     * @param id 主键ID
     */
    int deleteById(Serializable id);
    /**
     * 根据实体(ID)删除
     * @param entity 实体对象
     * @since 3.4.4
     */
    int deleteById(T entity);
    /**
     * 根据 columnMap 条件，删除记录
     * @param columnMap 表字段 map 对象
     */
    int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
    /**
     * 根据 entity 条件，删除记录
     * @param queryWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where
    语句）
     */
    int delete(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    /**
     * 删除（根据ID 批量删除）
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends
            Serializable> idList);
    /**
     * 根据 ID 修改
     * @param entity 实体对象
     */
    int updateById(@Param(Constants.ENTITY) T entity);
    /**
     * 根据 whereEntity 条件，更新记录
     * @param entity 实体对象 (set 条件值,可以为 null)
     * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成
    where 语句）
     */
    int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER)
    Wrapper<T> updateWrapper);
    /**
     * 根据 ID 查询
     * @param id 主键ID
     */
    T selectById(Serializable id);
    /**
     * 查询（根据ID 批量查询）
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends
            Serializable> idList);
    /**
     * 查询（根据 columnMap 条件）
     * @param columnMap 表字段 map 对象
     */
    List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object>
                                columnMap);
    /**
     * 根据 entity 条件，查询一条记录
     * <p>查询一条记录，例如 qw.last("limit 1") 限制取一条记录, 注意：多条数据会报异常
     </p>
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    default T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper) {
        List<T> ts = this.selectList(queryWrapper);
        if (CollectionUtils.isNotEmpty(ts)) {
            if (ts.size() != 1) {
                throw ExceptionUtils.mpe("One record is expected, but the query
                        result is multiple records");
            }
            return ts.get(0);
        }
        return null;
    }
    /**
     * 根据 Wrapper 条件，查询总记录数
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    Long selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    /**
     * 根据 entity 条件，查询全部记录
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    /**
     * 根据 Wrapper 条件，查询全部记录
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T>
                                                 queryWrapper);
    /**
     * 根据 Wrapper 条件，查询全部记录
     * <p>注意： 只返回第一个字段的值</p>
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
    /**
     * 根据 entity 条件，查询全部记录（并翻页）
     * @param page 分页查询条件（可以为 RowBounds.DEFAULT）
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    <P extends IPage<T>> P selectPage(P page, @Param(Constants.WRAPPER)
    Wrapper<T> queryWrapper);
    /**
     * 根据 Wrapper 条件，查询全部记录（并翻页）
     * @param page 分页查询条件
     * @param queryWrapper 实体对象封装操作类
     */
    <P extends IPage<Map<String, Object>>> P selectMapsPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
}
```

#### 2、添加

```java
@SpringBootTest
public class MyBatisPlusTest{
    
    @Autowired
    private UserMapper userMapper;
    
    @Test
    public void testInsert(){
        User user = new User(null, "张三", 23, "zhangsan@atguigu.com");
        //INSERT INTO user ( id, name, age, email ) VALUES ( ?, ?, ?, ? )
        int result = userMapper.insert(user);
        System.out.println("受影响行数："+result);
        //1475754982694199298
        System.out.println("id自动获取："+user.getId());
	}
}
```

> 最终执行的结果，所获取的id为1475754982694199298 
>
> 这是因为MyBatis-Plus在实现插入数据时，会默认基于雪花算法的策略生成id

#### 3、删除

##### a>通过id删除记

```java
@Test
public void testDeleteById(){
    //通过id删除用户信息
    //DELETE FROM user WHERE id=?
    int result = userMapper.deleteById(1475754982694199298L);
    System.out.println("受影响行数："+result);
}
```

##### b>通过id批量删除记录

```java
@Test
public void testDeleteBatchIds(){
    //通过多个id批量删除
    //DELETE FROM user WHERE id IN ( ? , ? , ? )
    List<Long> idList = Arrays.asList(1L, 2L, 3L);
    int result = userMapper.deleteBatchIds(idList);
    System.out.println("受影响行数："+result);
}
```

##### c>通过map条件删除记录

```java
@Test
public void testDeleteByMap(){
    //根据map集合中所设置的条件删除记录
    //DELETE FROM user WHERE name = ? AND age = ?
    Map<String, Object> map = new HashMap<>();
    //同时满足map中的所有条件
    map.put("age", 23);
    map.put("name", "张三");
    int result = userMapper.deleteByMap(map);
    System.out.println("受影响行数："+result);
}
```

#### 4、修改

```java
@Test
public void testUpdateById(){
    User user = new User(4L, "admin", 22, null);
    //UPDATE user SET name=?, age=? WHERE id=?
    int result = userMapper.updateById(user);
    System.out.println("受影响行数："+result);
}
```

#### 5、查询

##### a>根据id查询用户信息

```java
@Test
public void testSelectById(){
    //根据id查询用户信息
    //SELECT id,name,age,email FROM user WHERE id=?
    User user = userMapper.selectById(4L);
    System.out.println(user);
}
```

##### b>根据多个id查询多个用户信息

```java
@Test
public void testSelectBatchIds(){
    //根据多个id查询多个用户信息
    //SELECT id,name,age,email FROM user WHERE id IN ( ? , ? )
    List<Long> idList = Arrays.asList(4L, 5L);
    List<User> list = userMapper.selectBatchIds(idList);
    list.forEach(System.out::println);
}
```

##### c>通过map条件查询用户信息

```java
@Test
public void testSelectByMap(){
    //通过map条件查询用户信息
    //SELECT id,name,age,email FROM user WHERE name = ? AND age = ?
    Map<String, Object> map = new HashMap<>();
    map.put("age", 22);
    map.put("name", "admin");
    List<User> list = userMapper.selectByMap(map);
    list.forEach(System.out::println);
}
```

##### d>查询所有数据

```java
@Test
public void testSelectList(){
    //查询所有用户信息
    //SELECT id,name,age,email FROM user
    List<User> list = userMapper.selectList(null);
    list.forEach(System.out::println);
}
```

> 通过观察BaseMapper中的方法，大多方法中都有Wrapper类型的形参，此为条件构造器，可针对于SQL语句设置不同的条件，若没有条件，则可以为该形参赋值null，即查询（删除/修改）所有数据

#### 6、测试自定义功能

```java
/**
 * 根据id查询用户信息为map集合
 * @param id
 * @return
 */
Map<String, Object> selectMapById(Long id);
```

```xml
<select id="selectMapById" resultType="map">
    select id, name, age, email from user where id = #{id};
</select>
```

```java
@Test
public void testSelectByMap(){
    Map<String, Object> map = userMapper.selectMapById(1L);
    System.out.println(map);
}
```

#### 7、通用Service

> 说明: 
>
> 通用 Service CRUD 封装IService接口，进一步封装 CRUD 采用 <u>**get 查询单行**、**remove 删除 **、**list查询集合**、**page 分页**</u>，前缀命名方式区分 Mapper 层避免混淆
>
> 泛型 T 为任意实体对象 
>
> 建议如果存在自定义通用 Service 方法的可能，请创建自己的 IBaseService 继承 Mybatis-Plus 提供的基类 
>
> 官网地址：https://baomidou.com/pages/49cc81/#service-crud-%E6%8E%A5%E5%8F%A3

##### a>IService

MyBatis-Plus中有一个接口 IService和其实现类 ServiceImpl，封装了常见的业务层逻辑 

详情查看源码IService和ServiceImpl

##### b>创建Service接口和实现类

```java
/**
* UserService继承IService模板提供的基础功能
*/
public interface UserService extends IService<User> {
}
```

```java
/**
* ServiceImpl实现了IService，提供了IService中基础功能的实现
* 若ServiceImpl无法满足业务需求，则可以使用自定的UserService定义方法，并在实现类中实现
*/
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements
UserService {
}

```

##### c>测试查询记录数

```java
@SpringBootTest
public class MyBatisPlusServiceTest{
	
    @Autowired
    private UserService userService;

    @Test
    public void testGetCount(){
        //查询总记录数
        //SELECT COUNT(*) FROM user
        long count = userService.count();
        System.out.println("总记录数：" + count);
    }
}
```

##### d>测试批量插入

```java
@Test
public void testSaveBatch(){
    // SQL长度有限制，海量数据插入单条SQL无法实行，
    // 因此MP将批量插入放在了通用Service中实现，而不是通用Mapper
    ArrayList<User> users = new ArrayList<>();
    for (int i = 0; i < 5; i++) {
        User user = new User();
        user.setName("ybc" + i);
        user.setAge(20 + i);
        users.add(user);
    }
    //SQL:INSERT INTO t_user ( username, age ) VALUES ( ?, ? )
    userService.saveBatch(users);
}
```









