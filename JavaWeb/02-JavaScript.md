## JavaScript

---

[TOC]

[^文末]:点击到达文末。

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

* 数值类型 number
* 字符串类型 string
* 对象类型 object
* 布尔类型 boolean
* 函数类型 function

##### JavaScript里的特殊值

* undefined：未定义，所有JavaScript变量为赋予初始值时，默认值为undefined
* null
* NAN：Not a Number - 非数值

```javascript
var a = 12
var b = "abc";
alert(a * b); //NaN 非数值 非数字
```

##### JavaScript定义变量格式

```javascript
var 变量名 = 值;
```

#### JavaScript的运算

##### JavaScript的关系（比较）运算

* 等于： ==	等于是简单的做字面的比较

* 全等于： ===	除了做字面值的比较之外，还会比较两个变量的数据类型

```javascript
var a = "12"
var b = 12;
alert(a == b); //true
alert(a === b); //false
```

##### JavaScript的逻辑运算

在JavaScript中，所有变量都可以作为一个Boolean类型的变量去使用，且==0、null、undefined、""（空串）被认为是false==。

###### && 与运算

* 第一，当表达式全为真时，返回最后一个表达式的值。

* 第二，当表达式有一个为假时，返回第一个为假的表达式的值。

###### || 或运算

* 第一，当表达式全为假时，返回最后一个表达式的值
* 第二，只要有一个表达式为真。就会把第一个为真的表达式的值返回

#### JavaScript的数组 

##### 数组定义方式

```javascript
var 数组名 = [];
var 数组名 = [1, 'abc', true];
```

==注意==：JavaScript中的数组，只要我们通过数组下标赋值，那么最大的下标值，就会自动的给数组做扩容操作。

```javascript
for(var i = 0; i < arr.length; i++){
    alert(arr[i]);
}
```

#### 函数

##### 函数的两种定义方式

###### 第一种

可以使用function关键字定义函数

```javascript
function 函数名(形参列表){
    函数体
}
```

```javascript
function fun(a, b){
    alert(a + b);
}
fun("Hello", "JavaScript");
```

==注意==：JavaScript中定义带有返回值的函数，直接在函数体内加上==return==语句即可。

###### 第二种

```javascript
var 函数名 = function(形参列表){函数体}
```

##### 函数不允许重载

Javascript 中的函数重载 Javascript 中没有真正意义上的函数重载，因为 javascript 中同一作用域下的同名函数，前者会被后者覆盖，但是可通过其他方法间接实现重载同样的效果，Javascript中的函数没有签名，它的参数是由包含零的多个数组来表示的。

##### 函数的arguments隐形参数

* 只在function函数内

* 定义：在function函数中不需要定义，但却可以直接用来获取所有参数的变量。称为隐形参数。
* 隐形参数特别像Java中的可变长参数
* public void function(Object... args)
* JavaScript中隐形参数与Java的可变长参数一样，操作类似数组。

```javascript
function fun(){
    alert(arguments.length);
    
    for(var i = 0; i < arguments.length; i++){
        alert(arguments[i]);
    }
}
```

###### 需求

要求编写一个函数，用于计算所有参数相加的和并返回、

```javascript
function sum(num1, num2){
    var result = 0;
    for(var i = 0; i < arguments.length; i++){
        //避免字符串或者其他非number类型造成除了参数相加的其他操作
        if(typeof(arguments[i]) == "number"){
        	result += arguments[i];
    	}
    }
    return result;
}
alert(sum(num1, num2, ...));
```

#### JavaScript中的自定义对象

##### Object形式的自定义对象

```javascript
var 变量名 = new Object();//对象实例（空对象）
变量名.属性名 = 值;//定义一个属性
变量名.函数名 = function(){}//定义一个函数
```

##### {}花括号形式的自定义对象

```javascript
var 变量名 = {
    属性名:值,
    属性名:值,
    函数名:function(){}//最后一个不需要加逗号
};
```

#### JavaScript的事件

> 事件是电脑输入设备与页面进行交互的响应

##### 常用的事件

* onload 加载完成事件：页面加载完成之后，常用于做页面JavaScript代码初始化操作。
* onclick 单击事件：常用于按钮的点击响应操作。
* onblur 失去焦点事件：常用于输入框失去焦点后验证其输入内容是否合法。
* onchange 内容发送改变事件：常用于下拉列表和输入框发送改变后操作。
* onsubmit 表单提交事件：常用于表单提交前，验证所有表单项是否合法。

##### 事件的注册

> 告诉浏览器，当事件响应后要执行哪些操作代码，称为事件注册或事件绑定。

##### 静态注册事件

> 通过HTML标签的事件属性直接赋予事件响应后的代码，这种方式被称为静态注册。

##### 动态注册事件

> 是指先通过JavaScript代码得到标签的dom对象.事件名=function(){}，这种形式赋予事件响应后的代码，被称为动态注册。

###### 动态注册基本步骤：

1. 获取标签对象
2. 标签对象.事件名 = function(){}

##### onload事件

###### 静态注册onload事件

> onload事件是浏览器解析完页面就会自动触发的事件

onload事件的静态注册是在<body>标签中

```javascript
<body onload = "alert('静态注册onload事件')">
```

###### 动态注册onload事件

```javascript
<script type = "text/javascript">
    //onload事件动态注册，固定写法
    window.onload = function(){
    	alert("动态注册的onload事件");
	}
</script>
```

##### onclick事件

###### 静态注册onclick事件

```javascript
<button onclick = "onclickFun();"></button>
```

动态注册onclick事件

```javascript
<script type = "text/javascript">
    //动态注册onclick事件
    //1.获取标签对象
    //document对象是文档的根节点
    var btnObj = document.getElementById("btn01");
	//2.通过标签对象.事件名 = finction(){}
	btnObj.onclick = function(){
        alert("动态注册onclick事件");
    }
</script>
</head>
<body>
    <button id = "btn01">按钮</button>
```

##### 失去焦点事件





文末[^文末]

[返回文首](#JavaScript)

