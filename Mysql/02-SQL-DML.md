## SQL-DML

---

[TOC]

#### DML-添加数据

##### 给指定字段添加数据

```sql
INSERT INTO 表名(字段名1,字段名2, ...) VALUES (值1,值2, ...);
```



##### 给全部字段添加数据

```sql
INSERT INTO 表名 VALUES (值1,值2, ...);
```



##### 批量添加数据

```sql
INSERT INTO 表名(字段名1,字段名2, ...) VALUES(值1,值2, ...),(值1,值2, ...),(值1,值2, ...);
```

```sql
INSERT INTO 表名 VALUES(值1,值2, ...),(值1,值2, ...),(值1,值2, ...);
```



#### DML-修改数据

```sql
UPDATE 表名 SET 字段名1 = 值1,字段名2 = 值2, ...[WHERE 条件];
```

* 例子

```sql
UPDATE employee SET name = '小明', gender = 'man' WHERE id = '1';
```



#### DML-删除数据

```sql
DELETE FROM 表名 [WHERE 条件]; 
```

* 注意
  * DELETE语句不能删除某一个字段的值（可以使用UPDATE设置为NULL）

[返回文首](#SQL-DML)
