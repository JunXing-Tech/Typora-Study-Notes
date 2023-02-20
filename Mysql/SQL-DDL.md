



## SQL-DDL

---

[TOC]



### DDL - 数据库操作

#### 查询

##### 查询所有数据库

```sql
SHOW DATABASES;
```



##### 查询当前数据库

```sql
SELECT DATABASE();
```



#### 创建

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
```



#### 删除

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```



#### 使用

```sql
USE 数据库名;
```



### DDL-表操作

#### DDL-表操作-查询

##### 查询当前数据库所有表

```sql
SHOW TABLES
```



##### 查询表结构

```sql
DESC 表名;
```



##### 查询指定表的建表语句

```sql
SHOW CREATE TABLE 表名;
```



#### DDL-表操作-创建

```sql
CREATE TABLE 表名(
	字段1 字段1类型[COMMENT 字段1注释],
	字段2 字段2类型[COMMENT 字段2注释],
	字段3 字段3类型[COMMENT 字段3注释],
	......
	字段n 字段n类型[COMMENT 字段n注释]
)[COMMIT 表注释]；
```



#### DDL-表操作-数据类型

##### 数值类型

| 类型        | 大小   | 有符号范围(SIGNED)                 | 无符号范围(UNSIGNED)                  | 描述                 |
| ----------- | ------ | ---------------------------------- | ------------------------------------- | -------------------- |
| TINYINT     | 1 byte | (-128, 127)                        | (0, 255)                              | 小整数值             |
| SMALLINT    | 2byte  | (-32768, 32767)                    | (0, 65535)                            | 大整数值             |
| MEDIUMINT   | 3byte  | (-8388608, 8388607)                | (0,  )                                | 大整数值             |
| INT/INTEGER | 4 byte | (-2147483648, 2147483647)          | (0, 4294967295)                       | 大整数值             |
| BIGINT      | 8 byte | (-2^63^, 2^63-1^)                  | (0, 2^64-1^)                          | 极大整数值           |
| FLOAT       | 4 byte | $\approx -(3.4e^{38},3.4^{38})$    | 0和$\approx(1.1e^{-38}, 3.4e^{38} $)  | 单精度浮点数值       |
| DOUBLE      | 8 byte | $\approx (-1.7e^{308},1.7e^{308})$ | 0和$\approx(2.2e^{-308}, 1.7e^{308}$) | 双精度浮点数值       |
| DECIMAL     |        | 依赖于M（精度）和D（标度）的值     | 依赖于M（精度）和D（标度）的值        | 小数值（精确定点数） |

`score double(4,1)`



##### 字符串类型

| 类型       | 大小         | 描述                         | 特点                |
| ---------- | ------------ | ---------------------------- | ------------------- |
| CHAR       | 0-255        | 定长字符串                   | char(10)性能较好    |
| VARCHAR    | 0-65535      | 变长字符串                   | varchar(10)性能较差 |
| TINYBLOB   | 0-255        | 不超过255个字符的二进制数据  |                     |
| TINYTEXT   | 0-255        | 短文本字符串                 |                     |
| BLOB       | 0-65535      | 二进制形式的长文本数据       |                     |
| TEXT       | 0-65535      | 长文本数据                   |                     |
| MEDIUMBLOB | 0-16777215   | 二进制形式的中等长度文本数据 |                     |
| MEDIUMTEXT | 0-16777215   | 中等长度文本数据             |                     |
| LONGBLOB   | 0-4294967295 | 二进制形式的极大文本数据     |                     |



##### 日期时间类型

| 类型      | 大小(bytes) | 范围                                    | 格式                | 描述                     |
| --------- | ----------- | --------------------------------------- | ------------------- | ------------------------ |
| DATE      | 3           | 1000-01-01~9999-12-12                   | YYYY-MM-DD          | 日期值                   |
| TIME      | 3           | -838:59:59~838:59:59                    | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1           | 1901~2155                               | YYYY                | 年份值                   |
| DATETIME  | 8           | 1000-01-01 00:00:00~9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4           | 1970-01-01 00:00:00~2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |

案例：

```sql
CREATE TABLE emp{
	id INT COMMENT '编号',
	job_Number VARCHAR(10) COMMENT '员工工号',
	name VARCHAR(10) COMMENT '员工姓名',
	gender CHAR(1) COMMENT '性别',
	age TINYINT UNSIGNED COMMENT '年龄',
	identity_card CAHR(18) COMMENT '身份证号',
	onboarding_time DATE COMMENT '入职时间'
} COMMENT '员工信息表';
```



#### DDL-表操作-修改

##### 添加字段

```sql
ALTER TABLE 表名 ADD 字段名 类型（长度）[COMMENT注释][约束];
```

* 案例：为emp表增加一个新的字段“昵称”为nickname，类型为varchar(20)

```sql
ALTER TABLE emp nickname varchar(20) COMMENT '昵称';
```



##### 修改数据类型

```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）;
```



##### 修改字段名和字段类型

```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释][约束];
```

* 案例：将emp表的nickname字段修改为username，类型为varchar(30)

```sql
ALTER TABLE emp CHANGE nickname username varchar(30);
```



##### 删除字段

```sql
ALTER TABLE 表名 DROP 字段名;
```

案例：将emp表的字段username删除

```sql
ALTER TABLE emp DROP username;
```



##### 修改表名

```sql
ALTER TABLE 表名 RENAME TO 新表名;
```

案例：将emp表的表名修改为employee

```sql
ALTER TABLE emp RENAME TO employee;
```



#### DDL-表操作-删除

##### 删除表

```sql
DROP TABLE [IF EXISTS] 表名;
```



##### 删除指定表，并重新创建该表

```sql
TRUNCATE TABLE 表名;
```



#### 总结

##### 1.DDL-数据库操作

```SQL
SHOW DATABASE;
CREATE DATABASE 数据库名;
USE 数据库名;
SELECT DATABASE();
DROP DATABASE 数据库名;
```



##### 2.DDL-表操作

```sql
SHOW TABLE;
CREATE TABLE 表名（字段 字段类型，字段 字段类型）;
DESC 表名;
SHOW CREATE TABLE 表名;
ALTER TABLE 表名 ADD/MODIFY/CHANGE/DROP/RENAME TO...;
DROP TABLE 表名;
```



[返回文首](#SQL-DDL)

