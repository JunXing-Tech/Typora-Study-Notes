## SQL-DCL

---



[TOC]

#### DCL-介绍

> DCL英文全称是Data Control Language（数据控制语言），用来管理数据库用户、控制数据库的访问权限。

#### DCL-管理用户

##### 查询用户

```mysql
USE mysql;
SELECT * FROM user;
```

##### 创建用户

```mysql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

##### 修改用户密码

```mysql
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```

##### 删除用户

```mysql
DROP USER '用户名'@'主机名';
```

##### 案例

创建用户itcast，只能够在当前主机localhost访问，密码123456

```mysql
CREATE USER 'itcast'@'localhost' IDENTIFIED BY '123456';
```

创建用户heima，可以在任意主机访问该数据库，密码123456

```mysql
CREATE USER 'heima'@'%' '123456';
```

修改用户heima的访问密码1234

```mysql
ALTER USER 'heima'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
```

删除itcast@localhost用户

```mysql
DROP USER 'itcast'@'localhost';
```

##### 注意

* 主机名可以使用%通配



#### DCL-权限控制

| 权限                | 说明               |
| ------------------- | ------------------ |
| ALL、ALL PRIVILEGES | 所有权限           |
| SELECT              | 查询数据           |
| INSERT              | 插入数据           |
| UPDATE              | 修改数据           |
| DELETE              | 删除数据           |
| ALTER               | 修改表             |
| DROP                | 删除数据库/表/视图 |
| CREATE              | 创建数据库/表      |

##### 查询权限

```mysql
SHOW GRANTS FOR '用户名'@'主机名';
```

##### 授予权限

```mysql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```

##### 撤销权限

```mysql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

##### 案例

授予权限

```mysql
GRANT ALL ON itcast.* TO 'heima'@'%';
```

撤销权限

```mysql
REVOKE ALL ON itcast.* FROM 'heima'@'%';
```



[返回文首](#SQL-DCL)