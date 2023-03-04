## jQuery

---

[TOC]

#### jQuery介绍

> jQuery是JavaScript和查询(Query)，是辅助JavaScript开发的js类库。jQuery的核心思想是**write less, do more**,它实现了很多浏览器的兼容问题。 

#### jQuery的Hello程序示例

```javascript
<script type="text/javascript" src="jquery的文件路径"></script>
<script type="text/javascript">
    $(function(){//表示页面完成后加载之后，相当于window.onload=funtion(){}
    var $btnObj = $("#btnId");//表示按id查询标签对象
    $btnObj.click(function(){//绑定单击事件
        alert("jQuery的单击事件");
    })
})
</script>
```

##### 常见问题

* 使用jQuery需用jQuery库

* $是jQuery的函数

#### jQuery的核心函数

> $为jQuery的核心函数，可完成jQuery的很多功能。
>
> $()就是调用此函数

##### 四个作用

1.传入参数为函数时，表示页面加载完成之后，自动调用。

2.传入参数为HTML字符串时，根据这个字符串创建元素节点对象。

```javascript
$(function(){
    alert("页面加载完成之后，自动调用");
    $(
        "<div>" +
        	"<span>div-span1</span>"
        	"<span>div-span2</span>"
        "</div>"
    ).appendTo("body");
});
```

3.传入参数为选择器字符串时，根据输入的选择器类型查询标签对象。

* $("#id属性值");
* $("标签名");
* $(".class属性值");

4.传入参数为DOM对象时，会把DOM对象转化为jQuery对象。

#### 如何区分jQuery对象和Dom对象

##### Dom对象

* 通过getElementById() 查询出来的对象是Dom对象。
* 通过getElementByName() 查询出来的对象是Dom对象。
* 通过getElementByTagName() 查询出来的对象是Dom对象。
* 通过createElement() 查询出来的对象是Dom对象。
* Dom对象alert()出来的效果是==[object HTMLxxxElement]==

##### jQuery对象

* 通过jQuery提供的API创建的对象是jQuery对象。
* 通过jQuery包装的Dom对象是jQuery对象。
* 通过jQuery提供的API查询到的对象是jQuery对象。
* jQuery对象alert()出来的效果是==[object Object]==

#### jQuery对象的本质

jQuery对象是Dom对象的数组 + jQuery提供的一系列功能函数。

#### jQuery对象和Dom对象使用区别

jQuery对象与Dom对象的属性和方法无法通用。

#### Dom对象和jQuery对象互转

##### 1.Dom对象转化为jQuery对象

1. 先有Dom对象
2. $(Dom对象)就可以转换为jQuery对象

```javascript
var $obj = $(dom对象);
```

##### 2.jQuery对象转化为Dom对象

1. 先有jQuery对象
2. jQuery对象[下标]取出相应的Dom对象

```javascript
var dom = $obj[下标]
```

#### jQuery选择器

获取jQuery相关资料请[点击](https://www.runoob.com/jquery/jquery-tutorial.html)！

##### 基础选择器

```javascript
$(function(){//
	$("#btn1").click(funvtion(){
        //css()方法可以设置和获取样式
		$("#one").css("样式");
	});
	$("#btn2").click(function(){
        $(".mini").css("样式");
    });
	$("#btn3").click(function(){
        $("div").css("样式");
    })
	$("#btn4").click(function(){
        $("*").css("样式");
    })
	$("btn5").click(function(){
        $("span,#two").css("样式");
    })
})
```

##### 层级选择器

* ancestor descendant 后代选择器
  * 在给定的祖先元素下匹配所有的后代元素

* parent > child 子元素选择器
  * 在给定的父元素下匹配所有的==子元素==
* prev + next 相邻兄弟选择器
  * 匹配所有==紧接==在prev元素后的next元素(单个)
* perv ~ siblings 之后的兄弟元素选择器
  * 匹配perv元素之后的所有siblings元素


```javascript
$("from input");
$("form > input")
$("label + input");
$("form ~ input");
```

##### 过滤选择器

###### 基本过滤器

```javascript
:first //匹配第一个
$("li:first");
:last //匹配第二个
$("li:last");
:not(selector)
$("input:not(:checked)");
:even //匹配所有索引值为偶数的元素，从0开始计数
$("tr:even");
:odd //匹配所有索引值为奇数的元素，从0开始计数
$("tr:odd");
:eq(index) //匹配一个给定索引值的元素，从0开始计数
$("tr:eq(1)");
:gt(index) //匹配所有大于给定索引值的元素
$("tr:gt(0)");
:lt(index) //匹配所有小于给定索引值的元素
$("tr:lt(2)");
:header //匹配标题元素
$(":header").css("样式");
:animated //匹配所有正在执行动画效果的元素

```

###### 内容过滤选择器

```javascript
:contains(text) //匹配包含给定文本的元素
$("div:contains("John")");
:empty //匹配所有不包含子元素或者文本的空元素
$("td:empty");
:has(selector) //匹配含有子元素或者文本的元素的元素
$("div:has(p).addClass("test")");
:parent //匹配含有选择器所匹配的元素的元素（非空）
$("td:parent");
```

###### 属性过滤选择器

```javascript
[attribute] //匹配包含给定属性的元素
$("div[id]");
[attribute=value] //匹配包含给定属性是某个特定值的元素
$("input[name='newsletter']").attr("checked",true);
[attribute!=value] //匹配所有属性不等于特定值的元素，或者不含有指定的属性
$("input[name!='newsletter']").att("checked",true);
[attribute^=value] //匹配给定的属性是以某些值开始的元素
$("input[name^='news']");
[attribute$=value] //匹配给定的属性是以某些值结尾的元素
$("input[name$='letter']");
[attribute*=value] //匹配给定的属性是以包含某些值的元素
$("input[name*='man ']");
[attrSet1][attrSet2][attrSet3] //复合属性选择器，需要同时满足多个条件时使用
$("input[id][name$='man']");
```

###### 表单过滤选择器

```javascript
:input //匹配所有 input,textarea,select和button元素
$(":input");
:text //匹配所有的单行文本框
$(":text");
:password //匹配所有的密码框
$(":password");
:radio //匹配所有的单选按钮
$(":radio");
:checkbox //匹配所有的复选框
$(":checkbox");
:submit //匹配所有的提交按钮
$(":submit");
:image //匹配所有的图像
$("image");
:reset //匹配所有的重置按钮
$("reset");
:button //匹配所有的所有按钮
$("button");
:file //匹配所有的文件域
$(":file");
:hidden //匹配所有不可见元素，或者type为hidden的元素
$(":hidden");
```

###### 表单对象属性过滤选择器

```javascript
:enabled //匹配所有可用元素
$("input:enabled");
:disabled //匹配所有不可用元素
$("input:disabled");
:checked //匹配所有选中的被选中元素（复选框、单选框，不包括select中的option）
$("input:checked");
:selected //匹配所有选中的option元素
$("select option:selected");
```

###### 补充

```javascript
//val()可以操作表单项的value属性值
$(":text:enabled").val();

//each方法是jQuery对象提供用来遍历元素的方法
//在遍历的function函数，有一个this对象，就是当前遍历到的dom对象
$checkboies.each(function(){
    alert(this.values);
});

$("#btn5").click(function(){
    var $options = $("select option:selected");
    $options.each(function(){
        alert(this.innerHTML);
    });
});
```

#### jQuery元素的筛选

[参考资料](https://www.w3cschool.cn/jquery/jquery-ref-selectors.html)

```javascript
eq(index)
first()
last()
filter(expr)
is(exp)
has(expr)
not(exp)
chiledren(exp)
find(exp)
next()
nextAll()
nextUntil()
parent()
prev(exp)
prevAll()
prevUnit(exp)
siblings(exp)
add()
```

#### jQuery的属性操作

##### html()、text()、val方法

html()

* 可设置和获取起始标签和结束标签中的内容。跟dom属性innerHTML一样。

text()

* 可设置和获取起始标签和结束标签中的文本。跟dom属性innerText一样。

val()

* 可设置和获取表单项的value属性值。

##### val方法同时设置多个表单项的值

```javascript
$(function(){
    //不传参数，是获取，传递参数是设置
    alert($("div").html());
    $("div").html("可往div中设置内容");
    
    //不传参数，是获取，传递参数是设置
    alert($("div").text());
    $("div").text("可设置div中的文本");
    
    //不传参数，是获取，传递参数是设置
    $("button").click(function(){
        alert("#username").val();
        $("#username").val("");
    })
    
    //批量操作单选
    $(function(){
        //批量操作单选
        $(":radio").val(["radio01"]);
        //批量操作筛选框的选中状态
        $(":checkbox").val(["checkbox01","checkbox02"]);
        //批量操作多选的下拉框选中状态
        $("#multiple").val(["mul1","mul2"]);
        //批量操作单选的下拉框选中状态
        $("#single").val(["sin1"]);
        //批量操作
        $(":radio, :checkbox, ...").val(["radio01", ...]);
    })
})
```

##### att()和prop()方法

attr()

* 可以设置和获取属性的值

* 不推荐操作checked、readOnly、selected、disabled等
* 还可以操作自定义属性

prop()

* 可以设置和获取属性的值，只推荐操作checked、readOnly、selected、disabled等

```javascript
$(function(){
    $(":checkbox:first").attr("name");//获取
    $(":checked:first").attr("name","zjx");//设置
})
```

#### Dom的增、删、改

##### 内部插入

append()

```javascript
a.appendTo(b) //把a插入到b子元素末尾，成为最后一个子元素
$("<h1>标题</h1>").appendTo("div");
```

prependTo()

```javascript
 a.prependTo(b) //把a插入到b所有子元素前面，成为第一个子元素
```

##### 外部插入

insertAfter()

```javascript
a.insertAfter(b) //得到ba，平级插入
```

insertBefore()

```javascript
a.insertBefore(b) //ab
```

##### 替换

replaceWith()

```javascript
a.replaceWith(b) //用b替换a
```

replaceAll()

```javascript
a.replaceAll(b) //用a替换所有的b
```

##### 删除

remove()

```javascript
a.remove() //删除a标签
```

empty()

```javascript
a.empty() //清空a标签的内容
```

#### jQuery练习

##### 全选、全不选、反选

```javascript
```



##### 两列表的内容的相互移动

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset = "UTF-8">
    <title></title>
    <script type = "text/javascript" src = "https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <script type = "text/javascript">
        //页面加载完成
        $(function(){
            //第一个按钮 / 选中添加到右边
            $("button:eq(0)").click(function(){
                $("select:eq(0) option:selected").appendTo($("select:eq(1)"));
            });

            $("button:eq(1)").click(function(){
                $("select:eq(0) option").appendTo($("select:eq(1)"));
            });

            $("button:eq(2)").click(function(){
                $("select:eq(1) option:selected").appendTo($("select:eq(0)"));
            });

            $("button:eq(3)").click(function(){
                $("select:eq(1) option").appendTo($("select:eq(0)"));
            });
        });
    </script>

</head>
<body>
<div id = "left">
    <select multiple = "multiple" name = "sel01">
        <option value = "opt01">选项1</option>
        <option value = "opt02">选项2</option>
        <option value = "opt03">选项3</option>
        <option value = "opt04">选项4</option>
        <option value = "opt05">选项5</option>
        <option value = "opt06">选项6</option>
        <option value = "opt07">选项7</option>
        <option value = "opt08">选项8</option>
    </select>

    <button>选中添加到右边</button>
    <button>全部添加到右边</button>
</div>
<div id = "right">
    <select multiple = "multiple" name = "sel02"></select>
    <button>选中删除到左边</button>
    <button>全部删除到左边</button>
</div>
</body>
</html>
```

##### 动态添加、删除表格记录

```html
<html>
    <head>
        <meta charset = "UTF-8">
        <title></title>
        <script type = "text/javascript" src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
        <script type = "text/javascript">
            $(function(){
                //给submit按钮绑定单击事件
                $("#addEmpButton").click(function(){
                    //获取输入框的姓名、邮箱、工资的内容
                    var name = $("#empName").val();
                    var email = $("#email").val();
                    var salary = $("#salary").val();
                    //创建一个行标签对象，添加到显示器数据的表格中
                   	var $trObj = ("<tr> +
                	"<td>" + name +"</td>" +
                	"<td>" +  email +"</td>" +
                	"<td>" + salary + "</td>" +
                	"<td><a href = "deleteEmp?id = 001">Delete</a></td>" +
            		"</tr>");
                    //添加到显示数据的表格中
            		$trObj.appendTo("#employeeTable");
                    
                    //给删除的a标签绑定单击事件
                    $("a").click(function(){
                        
                        //return false; 可以阻止元素的默认行为
                        return false;
                    })
                });
            });
        </script>
    </head>
    <body>
        <table id = "">
            <tr>
                <th>name</th>
                <th>Email</th>
                <th>Salary</th>
                <th> </th>
            </tr>
            <tr>
                <td>Tom</td>
                <td>tom@tom.com</td>
                <td>5000</td>
                <td><a href = "deleteEmp?id = 001">Delete</a></td>
            </tr>
            <tr>
                <td>Jerry</td>
                <td>jerry@sohu.com</td>
                <td>8000</td>
                <td><a href = "deleteEmp?id = 002">Delete</a></td>
            </tr>
            <tr>
                <td>Bob</td>
                <td>bob@tom.com</td>
                <td>10000</td>
                <td><a href = "deleteEmp?id = 003">Delete</a></td>
            </tr>
        </table>
        <div id = "">
            <tr>
                <td class = "">name</td>
                <td class = "">
                    <input type = "text" name = "empName" id = "empName" />
                </td>
            </tr>
            <tr>
                <td class = "">email</td>
                <td class = "">
                    <input type = "text" name = "email" id = "email" />
                </td>
            </tr>
            <tr>
                <td class = "">salary</td>
                <td class = "">
                    <input type = "text" name = "salary" id = "salary" />
                </td>
            </tr>
            <tr>
                <td colspan = "2" align = "center">
                    <button id = "addEmpButton" value = "abc">
                        Submit
                    </button>
                </td>
            </tr>
        </div>
    </body>
</html>
```



[返回文首](#jQuery)

