## SQL-DQL

---

[TOC]

#### DQL-语法

```sql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

* 基本查询
* 条件查询(WHERE)
* 聚合查询(count、max、min、avg、sum)
* 分组查询(GROUP BY)
* 排序查询(ORDER BY)
* 分页查询(LIMIT)



#### DQL-基本查询

##### 查询多个字段

```sql
SELECT 字段1,字段2,字段3... FROM 表名;
```

```sql
SELECT * FROM 表名;
```

##### 设置别名

```sql
SELECT 字段1[AS 别名1],字段2[AS别名2] ... FROM 表名;
```

###### 案例

```sql
SELECT workaddress [AS] '工作地址' FROM emp;
```



##### 去除重复记录

```sql
SELECT DISTINCT 字段列表 FROM 表名;
```



#### DQL-条件查询

##### 语法

```sql
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

##### 条件

| 比较运算符       | 功能                                       |
| ---------------- | ------------------------------------------ |
| >                | 大于                                       |
| >=               | 大于等于                                   |
| <=               | 小于                                       |
| =                | 小于等于                                   |
| <>  或  !=       | 不等于                                     |
| BETWEEN...AND... | 在某个范围之内（含最小、最大值）           |
| IN(...)          | 在IN之后的列表中的值，多选一               |
| LIKE             | 模糊匹配（_匹配单个字符，%匹配任意个字符） |
| IS NULL          | 是NULL                                     |

| 逻辑运算符 | 功能                         |
| ---------- | ---------------------------- |
| AND 或 &&  | 并且（多个条件同时成立）     |
| OR 或 \|\| | 或者（多个条件任意一个成立） |
| NOT 或 ！  | 非，不是                     |

##### 案例

查询有身份证的员工信息

```sql
SELECT * FROM emp WHERE idcard IS NOT NULL;
```

查询年龄在15岁到20岁（包含）之间的员工信息

```sql
SELECT * FROM emp WHERE age BETWEEN 15 AND 20;
```

查询年龄等于18或20或40的员工信息

```sql
SELECT * FROM emp WHERE age IN (18, 20, 40);
```

查询姓名为两个字的员工信息

```sql
SELECT * FROM emp WHERE name LIKE '--';
```

查询身份证号最后一位是X的员工信息

```sql
SELECT * FROM emp WHERE idcard LIKE '%X';
```



#### DQL-聚合函数

##### 介绍

将一列数据作为一个整体，进行纵向计算

##### 常见聚合函数

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 最大值   |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |

##### 语法

```sql
SELECT 聚合函数(字段列表) FROM 表名;
```



#### DQL-分组查询

##### 语法

```sql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
```

##### WHERE与HAVING的区别

* 执行时机不同：WHERE是分组之前进行过滤，不满足WHERE条件，不参与分组；而HAVING是分组之后对结果进行过滤。
* 判断条件不同：WHERE不能对聚合函数进行判断，而HAVING可以。

##### 案例

根据性别分组，统计女性员工数量。

```sql
SELECT gender, COUNT(*) FROM emp GROUP BY gender;
```

查询年龄小于45的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址。

```sql
SELECT workaddress, COUNT(*) AS address_count FROM emp WHERE age < 45 GROUP BY workaddress HAVING addredd_count >= 3;
```

##### 注意

* 执行顺序：WHERE > 聚合函数 > HAVING。
* 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。



#### DQL-排序查询

##### 语法

```sql
SELECT 字段列表 FROM 表名 ODER BY 字段1 排序方式1, 字段2 排序方式2;
```





[返回文首](#SQL-DQL)












