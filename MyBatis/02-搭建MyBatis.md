# 搭建MyBatis

[toc]

#### 1、开发环境

IDE、构建工具：maven、MySQL、MyBatis

#### 2、创建Maven工程

##### 1.打包方式

* jar

##### 2.引入依赖

```xml
<dependencies>
    <!-- Mybatis核心 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <!-- junit测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!-- MySQL驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.3</version>
    </dependency>
</dependencies>
```

#### 3、创建MyBatis的核心配置文件 

习惯上命名为**mybatis-config.xml**，这个文件名仅仅只是建议，并非强制要求。将来整合Spring 之后，这个配置文件可以省略，所以大家操作时可以直接复制、粘贴。

核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息

核心配置文件存放的位置是src/main/resources目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--设置连接数据库的环境-->
    <environments default="development">
        <environment id="development">
        <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url"
                value="jdbc:mysql://localhost:3306/MyBatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <mapper resource="mappers/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### 4、创建mapper接口

MyBatis中的mapper接口相当于以前的dao。但是区别在于，mapper仅仅是接口，我们不需要提供实现类。

```mysql
create table t_user
(
    id       int auto_increment
        primary key,
    username varchar(20) null,
    password varchar(20) null,
    age      int         null,
    sex      char        null,
    email    varchar(20) null
);
```

```java
//在此之前，需要相关的表（t_user），实体类(User)
    /**
     * Mybatis面向接口编程的两个一致：
     * 1、映射文件的namespace要和mapper接口的全类名保持一致
     * 2、映射文件中的SQL语句id要和mapper接口中的方法名一致
     */
public interface UserMapper {
/**
* 添加用户信息
*/
int insertUser();
}
```

#### 5、创建MyBatis的映射文件

相关概念：==ORM（Object Relationship Mapping）对象关系映射==。

* 对象：Java的实体类对象 

* 关系：关系型数据库 

* 映射：二者之间的对应关系

| Java概念 | 数据库概念 |
| -------- | ---------- |
| 类       | 表         |
| 属性     | 字段/列    |
| 对象     | 记录/行    |

##### 1.映射文件的命名规则

表所对应的实体类的类名+Mapper.xml 

例如：表t_user，映射的实体类为User，所对应的映射文件为UserMapper.xml 

因此一个映射文件对应一个实体类，对应一张表的操作 

MyBatis映射文件用于编写SQL，访问以及操作表中的数据 

MyBatis映射文件存放的位置是src/main/resources/mappers目录下

##### 2、MyBatis中可以面向接口操作数据，要保证两个一致

1.mapper接口的全类名和映射文件的命名空间（namespace）保持一致 

2.mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.mapper.UserMapper">
    <!--int insertUser();-->
    <insert id="insertUser">
        insert into t_user values(null, 'admin', '123456', 23, '男', '123456@qq.com');
    </insert>
</mapper>
```

#### 6、通过junit测试功能

```java
//读取MyBatis的核心配置文件
InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
//创建SqlSessionFactoryBuilder对象
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new
SqlSessionFactoryBuilder();
//通过核心配置文件所对应的字节输入流创建工厂类SqlSessionFactory，生产SqlSession对象
SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);

//SqlSession默认不自动提交事务
//创建SqlSession对象，此时通过SqlSession对象所操作的sql都必须手动提交或回滚事务
//SqlSession sqlSession = sqlSessionFactory.openSession();

//创建SqlSession对象，此时通过SqlSession对象所操作的sql都会自动提交
SqlSession sqlSession = sqlSessionFactory.openSession(true);
//通过代理模式创建UserMapper接口的代理实现类对象
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
//调用UserMapper接口中的方法，就可以根据UserMapper的全类名匹配元素文件，通过调用的方法名匹配映射文件中的SQL标签，并执行标签中的SQL语句
int result = userMapper.insertUser();
//sqlSession.commit();
System.out.println("结果："+result);
```

* SqlSession：代表Java程序和数据库之间的会话。（HttpSession是Java程序和浏览器之间的会话）。
* SqlSessionFactory：是“生产”SqlSession的“工厂”。
* 工厂模式：如果创建某一个对象，使用的过程基本固定，那么我们就可以把创建这个对象的 相关代码封装到一个“工厂类”中，以后都使用这个工厂类来“生产”我们需要的对象。

#### 7、加入log4j日志功能

##### 1.加入依赖

```xml
<!-- log4j日志 -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

##### 2.加入log4j的配置文件

> log4j的配置文件名为log4j.xml，存放的位置是src/main/resources目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<!--
    原官方URL地址进行了更换，所以只需将配置地址进行相应修改即可（只修改URL也是会报错的，需要修改成下方所示）
    <!DOCTYPE log4j:configuration SYSTEM "http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/xml/doc-files/log4j.dtd">
    <log4j:configuration debug="true">
-->
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m (%F:%L) \n" />
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT" />
    </root>
</log4j:configuration>
```

> 日志的级别 
>
> FATAL(致命)>ERROR(错误)>WARN(警告)>INFO(信息)>DEBUG(调试) 
>
> 从左到右打印的内容越来越详细















