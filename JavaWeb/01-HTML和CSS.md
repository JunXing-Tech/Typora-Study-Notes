# HTML和CSS

[TOC]

### HTML

#### HTML书写规范

```html
<!DOCTYPE html><!--约束，声明-->
<html lang="zh_CN">
<head>
	<meta charset="UTF-8"><!--表示当前字符集-->
	<title>	</title><!--表示标题-->
</head>
<body>
    <!--页面主体内容-->
</body>
<html>
```

#### HTML标签介绍

* 标签拥有自己的属性
  * 基本属性：bgcolor="red"     可以修改简单的样式效果	
  * 事件属性:   onclick="alert()"; 可以直接设置事件响应后的代码

#### HTML常用标签

* font标签

* 特殊字符
* 标题标签
* 超连接
* 列表标签
* img标签
* 表格标签
* 跨行跨列表格
* iframe框架标签
* 表单标签
* 表单格式化
* 表单提交细节
* div、span、p标签



### CSS

#### 语法规则

```css
选择器{
    属性:值;
}
```

#### CSS和HTML的结合方式

第一种：在标签的style属性上设置"key:value value;" 修改样式表

第二种：在head标签中，使用style标签来定义各种需要的css样式。格式如下：

```html
xxx {
    Key : value value;
}
```

第三种：把css样式写成一个单独的css文件，再通过link标签引入即可服用。

```html
<link rel = "stylesheet" type = "text/css" href = "./style.css"/>
```

#### CSS选择器

##### 标签名选择器

```css
标签名{
    属性: 值；
}
```

##### id选择器

```css
#id 属性值{
    属性: 值;
}
```

##### class选择器

```css
.class 属性值{
    属性: 值;
}
```

##### 组合选择器

```css
选择器1, 选择器2, ..., 选择器n{
	属性: 值;
}
```

##### 常用样式

需要自查



[返回文首](#HTML和CSS)















