## JavaScript

---

[TOC]

#### JavaScript介绍

> JavaScript语言诞生主要是完成页面的数据验证。因此它运行在客户端，需要运行浏览器来解析执行JavaScript代码。

JavaScript是弱类型，Java是强类型。弱类型指类型可变。强类型指定义变量时，类型已确定，且不可变。

##### 特点

* 交互性（信息交互）
* 安全性（不允许直接访问本地硬盘）
* 跨平台性（只要是可以解释JavaScript的浏览器都可以执行，与平台无关）

#### JavaScript和HTML代码的结合方式

##### 第一种方式

在head标签中，或者在body标签中，使用script标签来书写JavaScript代码。

```html
<script type = "text/javascript">
    alert("Hello JavaScript!");
</script>
```

##### 第二种方式

使用script标签引入单独的JavaScript代码文件

```html
<script type = "text/javascript" src = "路径.js"></script>
```

#### JavaScript的变量和数据类型

##### JavaScript的变量类型

* 数值类型
* 字符串类型
* 对象类型
* 布尔类型
* 函数类型