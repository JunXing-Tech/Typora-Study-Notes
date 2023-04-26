# 自定义映射resultMap

[toc]

#### 1、通过字段别名解决字段名和属性名的映射关系

##### 第一种解决字段名和属性名不一致的情况--为字段名起别名,保持和属性名的一致

```xml
/**
 * 查询所有的员工信息
 */
List<Emp> getAllEmp();
```

```xml
<select id="getAllEmp" resultType="Emp">
    select eid, emp_name empName, age, sex, email from t_emp;
</select>
```

```java
/**
 * 解决字段名和属性不一致的情况:
 * a > 为字段名起别名,保持和属性名的一致
 */
@Test
public void testGetAllEmp(){
    SqlSession sqlSession  = SqlSessionUtils.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    List<Emp> list = mapper.getAllEmp();
    for(Emp emp : list){
        System.out.println(emp);
    }
}
```

##### 第二种解决字段名和属性名不一致的情况--设置全局配置，将_自动映射为驼峰

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"></properties>

     <!--设置MyBatis的全局配置-->
     <settings>
         <!--将_自动映射为驼峰， emp_name : empName-->
         <setting name="mapUnderscoreToCamelCase" value="true"/>
     </settings>
```

```xml
<select id="getAllEmp" resultType="Emp">
    <!--select eid, emp_name empName, age, sex, email from t_emp;-->
    select * from t_emp
</select>
```

```java
/**
 * 解决字段名和属性不一致的情况:
 * a > 为字段名起别名,保持和属性名的一致
 * b > 设置全局配置，将_自动映射为驼峰
 * <setting name="mapUnderscoreToCamelCase" value="true"/>
 */
@Test
public void testGetAllEmp(){
    SqlSession sqlSession  = SqlSessionUtils.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    List<Emp> list = mapper.getAllEmp();
    for(Emp emp : list){
        System.out.println(emp);
    }
}
```

##### 第三种解决字段名和属性名不一致的情况--resultMap处理字段和属性的映射关系

若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射

```xml
<!--
    resultMap：设置自定义映射关系
    id：表示自定义映射的唯一标识
    type：查询的数据要映射的实体类的类型
    子标签：
    id：设置主键的映射关系
    result：设置普通字段的映射关系
    association：设置多对一的映射关系
    collection：设置一对多的映射关系
    属性：
    property：设置映射关系中实体类中的属性名，必须是type属性所设置的实体类类型中的属性名
    column：设置映射关系中表中的字段名，必须是sql语句查询出的字段名
-->
<resultMap id="empResultMap" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
</resultMap>

<select id="getAllEmp" resultMap="empResultMap">
    select * from t_user;
</select>
```

##### 总结

若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则**（使用_）**，实体类中的属性名符合Java的规则（使用驼峰）

此时也可通过以下三种方式处理字段名和实体类中的属性的映射关系

a>可以通过为字段起别名的方式，保证和实体类中的属性名保持一致 

b>可以在MyBatis的核心配置文件中设置一个全局配置信息mapUnderscoreToCamelCase，可以在查询表中数据时，自动将_类型的字段名转换为驼峰 

例如：字段名user_name，设置了mapUnderscoreToCamelCase，此时字段名就会转换为 userName

c>若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射

#### 2、多对一映射处理

> 查询员工信息以及员工所对应的部门信息

##### a>处理多对一映射关系方式一：级联属性赋值

```java
/**
 * 查询员工以及员工所对应的部门信息
 */
Emp getEmpAndDept(@Param("eid") Integer eid);
```

```xml
<resultMap id="empAndDeptResultMapOne" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <result property="dept.did" column="did"></result>
    <result property="dept.deptName" column="dept_name"></result>
</resultMap>
<select id="getEmpAndDept" resultMap="empAndDeptResultMapOne">
    select * from t_emp left join t_dept on t_emp.did = t_dept.did where t_emp.eid = #{eid};
</select>
```

```java
@Test
public void testGetEmpAndDept(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    Emp emp = mapper.getEmpAndDept(1);
    System.out.println(emp);
}
```

##### b>处理多对一映射关系方式二：association处理映射关系

```xml
<resultMap id="empAndDeptResultMapTwo" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <!--
        association：处理多对一的映射关系
        property：需要处理多对的映射关系的属性名
        javaType：该属性的类型
    -->
    <association property="dept" javaType="Dept">
        <id property="did" column="did"></id>
        <result property="deptName" column="dept_name"></result>
    </association>
</resultMap>
<select id="getEmpAndDept" resultMap="empAndDeptResultMapTwo">
    select * from t_emp left join t_dept on t_emp.did = t_dept.did where t_emp.eid = #{eid};
</select>
```

##### c>分步查询

###### 1）查询员工信息

```java
/**
 * 通过分布查询查询员工以及员工所对应的部门信息
 * 分布查询第一步：查询员工信息
 */
Emp getEmpAndDeptByStepOne(@Param("eid") Integer eid);
```

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <!--
        select：设置分布查询的sql的唯一标识（namespace.SQLId或mapper接口的全类名.方法名）
        column：设置分布查询的条件
    -->
    <association property="dept"
                 select="com.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                 column="did"></association>
</resultMap>
<select id="getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
    select * from t_emp where eid = #{eid};
</select>
```

###### 2）根据员工所对应的部门id查询部门信息

```java
/**
 * 通过分布查询查询员工以及员工所对应的部门信息
 * 分布查询第二步：通过did查询员工所对应的部门
 */
Dept getEmpAndDeptByStepTwo(@Param("did") Integer did);
```

```xml
<select id="getEmpAndDeptByStepTwo" resultType="Dept">
    select * from t_dept where did = #{did};
</select>
```

###### 延迟加载

分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：

lazyLoadingEnabled（默认false）：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载 

```xml
<!--设置MyBatis的全局配置-->
 <settings>
     <!--将_自动映射为驼峰， emp_name : empName-->
     <setting name="mapUnderscoreToCamelCase" value="true"/>
     <!--开启延迟加载-->
     <setting name="lazyLoadingEnabled" value="true"/>
 </settings>
```

```xml
<!--
    select：设置分布查询的sql的唯一标识（namespace.SQLId或mapper接口的全类名.方法名）
    column：设置分布查询的条件
    fetchType：当开启了全局的延迟加载之后，可通过此属性手动控制延迟加载的效果
    fetchType="lazy | eager" : lazy表示延迟加载 eager表示立即加载
-->
<association property="dept"
             select="com.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
             column="did"
             fetchType="eager">
</association>
```

aggressiveLazyLoading（默认false）：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个 属性会按需加载 

###### 在多对一映射中分步查询时，延迟加载和立即加载的区别

1. 延迟加载：在查询主实体时，只查询主实体的信息，而附属实体信息暂时不查询，只有在需要访问附属实体信息时才会再次查询。这样可以减少查询次数，提高查询效率。但是必须要注意保证会话（Session）的打开状态，否则在访问附属实体信息时，如果会话已经关闭，将无法再次查询。
2. 立即加载：在查询主实体时，同时查询附属实体信息，将两个实体的信息都查询出来，这样就不需要再次查询。这样可以保证在访问附属实体信息时不会出现查询失败的情况。但是如果附属实体信息过多，会导致查询效率较低。

因此，应该根据具体情况选择适合的加载方式，以达到最优的查询效果。

#### 3、一对多映射处理

```java
//Dept.java
private List<Emp> emps;

public List<Emp> getEmps() {
    return emps;
}

public void setEmps(List<Emp> emps) {
    this.emps = emps;
}
//toString
```

##### a>collection

```java
/**
 * 获取部门以及部门中所有的员工信息
 */
Dept getDeptAndEmp(@Param("did") Integer did);
```

```xml
<resultMap id="deptAndEmpResultMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="deptName" column="dept_name"></result>
    <!--
        collection：处理一对多的映射关系
        ofType：表示该属性所对应的集合中存储数据的类型
    -->
    <collection property="emps" ofType="Emp">
        <id property="eid" column="eid"></id>
        <result property="empName" column="emp_name"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>
        <result property="email" column="email"></result>
    </collection>
</resultMap>
<select id="getDeptAndEmp" resultMap="deptAndEmpResultMap">
    select * from t_dept left join t_emp on t_dept.did = t_emp.did where t_dept.did = #{did};
</select>
```

```java
@Test
public void testGetDeptAndEmp(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    DeptMapper mapper = sqlSession.getMapper(DeptMapper.class);
    Dept dept = mapper.getDeptAndEmp(1);
    System.out.println(dept);
}
```

##### b>分步查询

1）查询部门信息

```java
/**
 * 通过分布查询查询部门以及部门中所有的员工信息
 * 分布查询第一步：查询部门信息
 */
Dept getDeptAndEmpByStepOne(@Param("did") Integer did);
```

```xml
<resultMap id="deptAndEmpByStepResultMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="deptName" column="dept_name"></result>
    <collection property="emps"
                select="com.mybatis.mapper.EmpMapper.getDeptAndEmpByStepTwo"
                column="did"
                fetchType="eager"></collection>
</resultMap>
<select id="getDeptAndEmpByStepOne" resultMap="deptAndEmpByStepResultMap">
    select * from t_dept where did = #{did};
</select>
```

2）根据部门id查询部门中的所有员工

```java
/**
 * 通过分布查询查询部门以及部门中所有的员工信息
 * 分布查询第二步：根据did查询员工信息
 */
List<Emp> getDeptAndEmpByStepTwo(@Param("did") Integer did);
```

```xml
<select id="getDeptAndEmpByStepTwo" resultType="Emp">
    select * from t_emp where did = #{did};
</select>
```

```java
@Test
public void testGetDeptAndEmpByStep(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    DeptMapper mapper = sqlSession.getMapper(DeptMapper.class);
    Dept dept = mapper.getDeptAndEmpByStepOne(1);
    System.out.println(dept);
}
```



> 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：
>
> lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载 
>
> aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个 属性会按需加载 
>
> 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和 collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加 载)|eager(立即加载)"
