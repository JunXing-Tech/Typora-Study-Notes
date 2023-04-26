# 动态SQL

[toc]

Mybatis框架的动态SQL技术是一种根据特定条件动态拼装SQL语句的功能，它存在的意义是为了解决 拼接SQL语句字符串时的痛点问题。

#### 1、if

if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；反之标签中 的内容不会执行

```java
/**
 * 多条件查询
 */
List<Emp> getEmpByCondition(Emp emp);
```

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
    select * from t_emp where 1 = 1
    <if test="empName != null and empName != ''">
        emp_name = #{empName}
    </if>
    <if test="age != null and age != ''">
        and age = #{age}
    </if>
    <if test="sex != null and sex != ''">
        and sex = #{sex}
    </if>
    <if test="email != null and email != ''">
        and email = #{email}
    </if>
</select>
```

```java
@Test
public void testGetEmpByCondition(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);
    List<Emp> empByCondition = mapper.getEmpByCondition(new Emp(null, "张三",23, "男", "123@qq.com"));
    System.out.println(empByCondition);
}
```

#### 2、where

```xml
<select id="getEmpByCondition" resultType="Emp">
    select * from t_emp
    <where>
        <if test="empName != null and empName != ''">
            emp_name = #{empName}
        </if>
        <if test="age != null and age != ''">
            and age = #{age}
        </if>
        <if test="sex != null and sex != ''">
            and sex = #{sex}
        </if>
        <if test="email != null and email != ''">
            and email = #{email}
        </if>
    </where>
</select>
```

> where和if一般结合使用： 
>
> 1.若where标签中的if条件都不满足，则where标签没有任何功能，即不会添加where关键字 
>
> 2.若where标签中的if条件满足，则where标签会自动添加where关键字，并将条件最前方多余的 and去掉 
>
> 注意：where标签不能去掉条件最后多余的and

#### 3、trim

```xml
<select id="getEmpListByMoreTJ" resultType="Emp">
select * from t_emp
    <trim prefix="where" suffixOverrides="and">
        <if test="ename != '' and ename != null">
            ename = #{ename} and
        </if>
        <if test="age != '' and age != null">
            age = #{age} and
        </if>
        <if test="sex != '' and sex != null">
            sex = #{sex}
        </if>
    </trim>
</select>
```

> trim用于去掉或添加标签中的内容 
>
> 常用属性： 
>
> prefix：在trim标签中字符串的开头添加指定的字符串
>
> prefixOverrides：在trim标签中从字符串的开头开始，移除指定的前缀
>
> suffix：在trim标签中字符串的末尾添加指定的字符串
>
> suffixOverrides：在trim标签中从字符串的末尾开始，移除指定的后缀
>
> suffixOverrides和prefixOverrides：如果同时指定了suffixOverrides和prefixOverrides，那么MyBatis会先进行suffixOverrides操作，再进行prefixOverrides操作

#### 4、choose、when、otherwise

```java
/**
 * 测试choose、when、otherwise
 */
List<Emp> getEmpByChoose(Emp emp);
```

```xml
<select id="getEmpByChoose" resultType="Emp">
    select * from t_emp
    <where>
        <choose>
            <when test = "empName != null and empName != ''">
                emp_name = #{empName}
            </when>
            <when test = "age != null and age != ''">
                age = #{age}
            </when>
            <when test = "sex != null and sex != ''">
                sex = #{sex}
            </when>
            <when test = "email != null and email != ''">
                email = #{email}
            </when>
            <otherwise>
                did = 1
            </otherwise>
        </choose>
    </where>
</select>
```

#### 5、foreach

##### 通过数组实现批量删除

```java
/**
 * 通过数组实现批量删除
 */
int deleteMoreByArray(@Param("eids") Integer[] eids);
```

```xml
<!--int deleteMoreByArray(int[] eids);-->
<delete id="deleteMoreByArray">
    delete from t_emp where
    <foreach collection="eids" item="eid" separator="or">
        eid = #{eid}
    </foreach>
</delete>

<!--int deleteMoreByArray(int[] eids);-->
<delete id="deleteMoreByArray">
    delete from t_emp where eid in
    <foreach collection="eids" item="eid" separator="," open="(" close=")">
        #{eid}
    </foreach>
</delete>
```

##### 通过list集合实现批量添加

```java
    /**
     * 通过list集合实现批量添加
     */
    int insertMoreByList(@Param("emps") List<Emp> emps);
```

```xml
<!--int insertMoreByList(List<Emp> emps);-->
<insert id="insertMoreByList">
    insert into t_emp values
    <foreach collection="emps" item="emp" separator=",">
        (null, #{emp.empName}, #{emp.age}, #{emp.sex}, #{emp.email}, null)
    </foreach>
</insert>
```

```java
@Test
public void testInsertMoreByList(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);
    Emp emp1 = new Emp(null, "a1", 23, "男", "123@qq.com");
    Emp emp2 = new Emp(null, "a2", 24, "男", "123@qq.com");
    Emp emp3 = new Emp(null, "a3", 25, "男", "123@qq.com");
    List<Emp> emps = Arrays.asList(emp1, emp2, emp3);
    int i = mapper.insertMoreByList(emps);
    System.out.println(i);
}
```

> 属性： 
>
> collection：设置要循环的数组或集合 
>
> item：表示集合或数组中的每一个数据 
>
> separator：设置循环体之间的分隔符 
>
> open：设置foreach标签中的内容的开始符 
>
> close：设置foreach标签中的内容的结束符

#### 6、SQL片段

sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```xml
<sql id="empColumns">
    eid,ename,age,sex,did
</sql>
    select <include refid="empColumns"></include> from t_emp
```

7、总结

```markdown
**动态SQL:**
1、if
	根据标签中test属性所对应的表达式决定标签中的内容是否需要拼接到SQL中
2、where：
    当where标签中有内容时，会自动生成where关键字，并且将内容前多余的and或or去掉
    当where标签中没有内容时，此时where标签没有任何效果
    注意：where标签不能将其中内容后面多余的and或or去掉
3、trim
    若标签中有内容时：
        prefix | suffix：将trim标签中内容前面或后面添加指定内容
        suffixOverrides | prefixOverrides：将trim标签中内容前面或后面去掉指定内容；若标签中没有内容·时，trim标签也没有任何效果
        suffixOverrides和prefixOverrides：如果同时指定了suffixOverrides和prefixOverrides，那么MyBatis会先进行suffixOverrides操作，再进行prefixOverrides操作。
4、choose、when、otherwise，相当于if...else if...else
    when至少要有一个，otherwise最多只能有一个
5、foreach
    collection：设置需要循环的数组或集合
    item：表示数组或集合中的每一个数据
    separator：循环体之间的分割符
    open：foreach标签所循环的所有内容的开始符
    close：foreach标签所循环的所有内容的结束符
6、sql标签
    设置SQL片段：<sql id="empColums"> eid, emp_name, age, sex, email</sql>
    引用SQL片段：<include refid = "empColumns"></include>
```