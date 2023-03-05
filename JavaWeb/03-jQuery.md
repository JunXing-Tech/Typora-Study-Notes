## jQuery

---

[TOC]

#### jQuery介绍

> jQuery是JavaScript和查询(Query)，是辅助JavaScript开发的js类库。jQuery的核心思想是**write less, do more**,它实现了很多浏览器的兼容问题。查询可点击 ==[jQuery API 中文在线手册](https://jquery.cuishifeng.cn/)==和==[菜鸟教程](https://www.runoob.com/jquery/jquery-tutorial.html)==

#### jQuery的Hello程序示例

```html
<script type="text/javascript" src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
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

#### 如何区分jQuery对象和DOM对象

##### DOM对象

* 通过getElementById() 查询出来的对象是DOM对象。
* 通过getElementByName() 查询出来的对象是DOM对象。
* 通过getElementByTagName() 查询出来的对象是DOM对象。
* 通过createElement() 查询出来的对象是DOM对象。
* DOM对象alert()出来的效果是**[object HTMLxxxElement]**

##### jQuery对象

* 通过jQuery提供的API创建的对象是jQuery对象。
* 通过jQuery包装的DOM对象是jQuery对象。
* 通过jQuery提供的API查询到的对象是jQuery对象。
* jQuery对象alert()出来的效果是**[object Object]**

##### jQuery对象的本质

> jQuery对象是DOM对象的数组 + jQuery提供的一系列功能函数。

##### jQuery对象和DOM对象使用区别

> jQuery对象与DOM对象的属性和方法无法通用。

#### DOM对象和jQuery对象互转

##### 1.DOM对象转化为jQuery对象

1. 先有DOM对象
2. $(DOM对象)就可以转换为jQuery对象

```javascript
var $obj = $(DOM对象);
```

##### 2.jQuery对象转化为DOM对象

1. 先有jQuery对象
2. jQuery对象[下标]取出相应的DOM对象

```javascript
var DOM = $obj[下标]
```

#### jQuery选择器

获取jQuery相关资料请==[点击](https://www.runoob.com/jquery/jquery-tutorial.html)==！

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
  * 在给定的父元素下匹配所有的**子元素**
* prev + next 相邻兄弟选择器
  * 匹配所有**紧接**在prev元素后的next元素(单个)
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
//在遍历的function函数，有一个this对象，就是当前遍历到的DOM对象
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

==[jQuery API 中文在线手册](https://jquery.cuishifeng.cn/)==

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

* 可设置和获取起始标签和结束标签中的内容。跟DOM属性innerHTML一样。

text()

* 可设置和获取起始标签和结束标签中的文本。跟DOM属性innerText一样。

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

#### DOM的增、删、改

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
                //复用
                var deleteFun = function(){
                        //在事件响应的function函数中，有一个this对象。这个this对象是当前正在响应事件的DOM对象
                        var $trObj = $(this).parent().parent();
                        var name = $teObj.find("td:first").text();
                        
                        //confirm是JavaScript语言提供的一个确认提示框函数，你给他传什么，它就提示什么
                        //当用户点击了确定，就返回true。当用户点击了取消就返回false
                        if(confirm("你确定要删除[" + name +"]吗？")){
                            $trObj.remove();
                        }
                        //return false; 可以阻止元素的默认行为
                        return false;
                }
                
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
                    //给添加的行的a标签绑上单击事件
                    $trObj.find("a").click(deleteFun);
                    //给删除的a标签绑定单击事件
                    $("a").click(deleteFun);
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

##### 品牌展示

```javascript

```

##### 图片跟随

```html
<script type="text/javascript" src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script type="text/javascript">
    $(function(){
        $("#small").bind("mouseover mouseout", function(event){
            if(event.type == "mouseover"){
                $("#showBig").show();
            }else if(event.type == "mouseout"){
                $("#showBig").hide();
            }else if(event.type == "mousemove"){
                $("#showBig").offset({
                    //避免两个图片重叠而导致event.type的判定出现问题
                    left:event.pageX + 10;
                    top:event.pageY + 10;
                });
            }
        });
    });
</script>
```

#### CSS样式操作

```javascript
addClass(样式1 样式2 ...); //添加样式

removeClass(); //删除样式

toggleClass(); //有则删除，无则添加

offset(); //获取和设置元素的坐标还可添加
$divEle.offset({
    top:100,
    left:50
});
```

#### jQuery动画操作

##### 基本动画

```javascript
show() //显示
$("#btn1").click(function(){
    $("#div1").show(2000,function(){
        alert("show动画完成");
    });
});

hide() //隐藏

toggle() //显示隐藏切换
```

以上动画方法都可以添加参数

1. 第一个参数是动画执行的时长，以毫秒为单位
2. 第二个参数是动画的回调函数（动画完成后自动调用的函数）

##### 淡入淡出动画

```javascript
fadeln() //淡入（慢慢可见）

fadeOut() //淡出（慢慢消失）

fadeTo() //在指定时长内慢慢的将透明度修改到指定的值(0 ~ 1)

fadeToggle() //淡化切换
```

#### jQuery事件操作

##### 原生JavaScript和jQuery页面加载完成之后的区别

```javascript
$(function(){
    
});

window.onload = function(){
    
};
```

###### 他们分别是在什么时候触发？

1. jQuery的页面加载完成之后是浏览器的内核解析完页面的标签创建好DOM对象之后就会马上执行。
2. 原生JavaScript的页面加载完成之后，除了要等浏览器内核解析完标签创建好DOM对象，还要等标签显示时需要的内容加载完成。

###### 他们触发的顺序？

1. jQuery页面加载完成后**先执行**
2. 原生JavaScript页面加载完成**后执行**

###### 他们执行的次数？

1. 原生JavaScript的页面加载完成后，只会执行最后一次的赋值函数
2. jQuery的页面加载完成之后，是把所有的注册的function函数，按顺序依次执行。

##### jQuery中其他的事件处理办法

```javascript
click() //可以绑定单击事件以及触发单击事件
$(function(){
   $("h5").click(funcion(){//传function是绑定事件
         alert("");
   });
	$("button").click(function(){//不传function是触发事件
        $("h5").click();
    });
});

mouseover() //鼠标移入事件

mouseout() //鼠标移出事件

bind() //可以给元素一次性绑定一个或多个事件

one() //与bind()类似，但one()方法绑定的事件只会响应一次

live() //可以用来绑定选择器匹配的所有元素事件， 尽管这个元素是后面动态创建出来的也有效

unbind() //解除事件的绑定
```

##### 事件的冒泡

###### 什么是事件的冒泡？

是指，父子元素同时监听同一个事件。当触发子元素的事件的时候，同一个事件也被传递到了父元素的事件里去响应。

###### 如何阻止事件冒泡？

在事件函数体内，`return false; `可以阻止事件的冒泡传递。

##### JavaScript事件对象

事件对象，是封装有触发的事件信息的一个JavaScript对象。我们重点关系是怎么拿到这个JavaScript的事件对象，以及使用。

###### 如何获取JavaScript事件对象？

在给元素绑定事件的时候，在事件的function(event)参数列表中添加一个参数，这个参数名习惯取名为event。这个event就是JavaScript传递参事件处理函数的事件对象。

1. ###### 原生JavaScript获取事件对象

```javascript
window.onload = function(){
    document.getElementById("areaDiv").onclick = function(event){
        console.log(event);
    }
}
```

2. ###### jQuery获取事件对象

```javascript
$(function(){
   $("#areaDiv").click(function(event){
   		console.log(event);
   });
});
```

3. ###### 使用bin()同时对多个事件绑定同一个函数。怎么获取当前操作时什么事件

```javascript
$(function(){
   $("#areaDiv").bind("mouseover mouseout", function(event){
       if(event.type == "mouseover"){
           console.log("鼠标移入");
       }else if(event.type == "mouseout"){
           console.log("鼠标移出");
       }
   }); 
});
```



[返回文首](#jQuery)

