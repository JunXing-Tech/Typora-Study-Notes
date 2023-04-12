## JdbcTemplate

---

[toc]

#### JdbcTemplate(概念和准备)

##### 1、什么是 JdbcTemplate

（1）Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作

##### 2、准备工作

###### （1）引入相关 jar 包

```markdown
druid-1.1.9.jar
mysql-connector-java-5.1.7-bin.jar
spring-jdbc-5.2.6.RELEASE.jar
spring-orm-5.2.6.RELEASE.jar
spring-tx.5.2.6.RELEASE.jar
```

###### （2）在 spring 配置文件配置数据库连接池

```xml
<!-- 数据库连接池 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="url" value="jdbc:mysql:///user_db" />
    <property name="username" value="root" />
    <property name="password" value="root" />
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
</bean>
```

###### （3）配置 JdbcTemplate 对象，注入 DataSource

```xml
<!-- JdbcTemplate 对象 -->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
<!--注入 dataSource-->
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

###### （4）创建 service 类，创建 dao 类，在 dao 注入 jdbcTemplate 对象

配置文件

```xml
<!-- 组件扫描 -->
<context:component-scan base-package="com.spring5"></context:component-scan>
```

Service

```java
@Service
public class BookService {
    //注入 dao
    @Autowired
    private BookDao bookDao;
}
```

Dao

```java
public interface BookDao{
    
}

@Repository
public class BookDaoImpl implements BookDao {
    //注入 JdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;
}
```

#### JdbcTemplate 操作数据库（添加）

##### 1、对应数据库创建实体类

```java
public class User{
    private String userId;
    private String username;
    private String ustatus;
    	
        public String getUserId(Strin userId){
        return userId;
    }
    public String getUsername(Strin username){
        return usernname;
    }
    public String getUstatus(Strin ustatus){
        return ustatus;
    }
    public void setUserId(Strin userId){
        this.userId = userId;
    }
    public void setUsername(Strin username){
        this.userId = userId;
    }
    public void setUstatus(Strin ustatus){
        this.userId = userId;
    }
}
```

##### 2、编写 service 和 dao

（1）在 dao 进行数据库添加操作 

（2）调用 JdbcTemplate 对象里面 update 方法实现添加操作

```java
update(String sql, Object... args)
// 有两个参数
//第一个参数：sql语句
//第二个语句：可变参数，设置sql语句值
```

```java
public interface BookDao{
    //添加的方法
    void add(Book book);
    
    //修改的方法
    void updateBook(Book book);
    
    //删除的方法
    void delete(String id);
    
    //查询记录数
	int selectCount();
    
    //查询返回对象
    Book findBookInfo(String id);
    
    //查询返回集合
    List<Book> findAllBook();
}
```

```java
public class BookService{
    
    //注入dao
    @Autowired
    private BookDao bookDao;
    
    public void addBook(Book book){
        bookDao.add(book);
    }
    public void updateBook(Book book){
        bookDao.updateBook(book);
    }
    public void deleteBook(String id){
        bookDao.deketeBook(id);
    }
    public int findCount(){
        return bookDao.selectCount();
    }
    public Book findOne(String id){
        return bookDao.findBookInfo(id);
    }
    public List<Book> findAll(){
        return bookDao.findAllBook();
    }
}
```

```java
@Repository
public class BookDaoImpl implements BookDao {
    //注入 JdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;
    //添加的方法
    @Override
    public void add(Book book) {
        //1 创建 sql 语句
        String sql = "insert into t_book values(?,?,?)";
        //2 调用方法实现
        Object[] args = {book.getUserId(), book.getUsername(), 
        book.getUstatus()};
        int update = jdbcTemplate.update(sql,args);
        System.out.println(update);
    }
}

```

##### 3、测试类

```java
@Test
public void testJdbcTemplate() {
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    BookService bookService = context.getBean("bookService", BookService.class);
    Book book = new Book();
    book.setUserId("1");
    book.setUsername("java");
    book.setUstatus("a");
    bookService.addBook(book);
}
```

#### JdbcTemplate 操作数据库（修改和删除）

##### 1、修改

```java
@Override
public void updateBook(Book book) {
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    Object[] args = {book.getUsername(), book.getUstatus(),book.getUserId()};
    int update = jdbcTemplate.update(sql, args);
    System.out.println(update);
}
```

##### 2、删除

```java
@Override
public void delete(String id) {
    String sql = "delete from t_book where user_id=?";
    int update = jdbcTemplate.update(sql, id);
    System.out.println(update);
}
```

#### JdbcTemplate 操作数据库（查询返回某个值）

1、查询表里面有多少条记录，返回是某个值 

2、使用 JdbcTemplate 实现查询返回某个值代码

```java
queryForObject(String sql, Class<T> requiredType)
//有两个参数
//第一个参数：sql语句
//第二个参数：返回类型Class
```

```java
@Override
public int selectCount(){
    String sql = "selcet count(*) from t_book";
    Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
    return count;
}
```

```java
//TestBook.java
int count  = bookService.findCount();
System.out.println(count);
```

#### JdbcTemplate 操作数据库（查询返回对象）

1、场景：查询图书详情 

2、JdbcTemplate 实现查询返回对象

```java
queryForObject(String sql, RowMapper<T> rowMApper, Object... args)
//有三个参数
//第一个参数：sql语句
//第二个参数：RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装
//第三个参数：Sql语句值
```

```java
//查询返回对象
@Override
public Book findBookInfo(String id) {
    String sql = "select * from t_book where user_id=?";
    //调用方法
    Book book = jdbcTemplate.queryForObject(sql, new 
    BeanPropertyRowMapper<Book>(Book.class), id);
    return book;
}
```

```java
Book book = bookService.findOne("1");
System.out.println(book);
```

#### JdbcTemplate 操作数据库（查询返回集合）

1、场景：查询图书列表分页… 

2、调用 JdbcTemplate 方法实现查询返回集合

```java
query(String sql, RowMapper<T> rowMapper, Object.. args)
//有三个参数
//第一个参数：sql语句
//第二个参数：RowMapper是接口，针对返回不同数据类型，使用这个接口里面实现类完成数据封装
//第三个参数：sql语句值
```

```java
//查询返回集合
@Override
public List<Book> findAllBook() {
    String sql = "select * from t_book";
    //调用方法
    List<Book> bookList = jdbcTemplate.query(sql, new 
    BeanPropertyRowMapper<Book>(Book.class));
    return bookList;
}
```

```java
List<Book> all = bookService.findAll();
System.out.println(all);
```

#### JdbcTemplate 操作数据库（批量操作）

##### 1.JdbcTemplate 实现批量添加操作

1、批量操作：操作表里面多条记录 

2、JdbcTemplate 实现批量添加操作

```java
batchUpdate(String sql, List<Object[]> batchArgs)
//有两个参数
//第一个参数：sql语句
//第二个参数：List集合，添加多条记录数据
```

```java
//Service
public void batchAdd(List<Object[]> batchArgs){
    bookDao.batchAddBook(batchArgs);
}

//Dao
void batchAddBook(List<Object[] batchArgs>);

//批量添加
@Override
public void batchAddBook(List<Object[]> batchArgs) {
    String sql = "insert into t_book values(?,?,?)";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}

//批量添加测试
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"3","java","a"};
Object[] o2 = {"4","c++","b"};
Object[] o3 = {"5","MySQL","c"};
batchArgs.add(o1);
batchArgs.add(o2);
batchArgs.add(o3);
//调用批量添加
bookService.batchAdd(batchArgs);
```

##### 2、JdbcTemplate 实现批量修改操作

```java
//批量修改
@Override
public void batchUpdateBook(List<Object[]> batchArgs) {
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
//批量修改
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"java0909","a3","3"};
Object[] o2 = {"c++1010","b4","4"};
Object[] o3 = {"MySQL1111","c5","5"};
batchArgs.add(o1);
batchArgs.add(o2);
batchArgs.add(o3);
//调用方法实现批量修改
bookService.batchUpdate(batchArgs);
```

##### 3、JdbcTemplate 实现批量删除操作

```java
//批量删除
@Override
public void batchDeleteBook(List<Object[]> batchArgs) {
    String sql = "delete from t_book where user_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
//批量删除
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"3"};
Object[] o2 = {"4"};
batchArgs.add(o1);
batchArgs.add(o2);
//调用方法实现批量删除
bookService.batchDelete(batchArgs);
```



[返回文首](#JdbcTemplate)