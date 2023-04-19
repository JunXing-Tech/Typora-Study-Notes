# Vue快速入门

[toc]

#### 1、Vue基础

##### 1.Vue简介

JavaScript框架

简化Dom操作

响应式数据驱动

##### 2.第一个Vue程序

```html
<body>
    <div id="app">
        {{ message }}
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app"
            data:{
            	message:"Hello Vue!"
        	}
        })
    </script>
</body>
```

导入开发版本的Vue.js

创建Vue实例对象，设置el属性和data属性

使用模板语法把数据渲染到页面上

##### 3.el:挂载点

Vue实例的作用范围是什么？

Vue会管理el选项命中的元素及其内部的后代元素

是否可以使用其他的选择器？

可以使用其他的选择器，但是建议使用ID选择器

是否可以设置其他的dom元素？

可以使用其他的双标签，不能使用html和body标签

##### 4.data:数据对象

```html
<body>
    <div id="app">
        {{ message }}
        <h2>{{ User.name }} {{ User.id }}</h2>
        <ul>
            <li>{{ arr[0] }}</li>
            <li>{{ arr[1] }}</li>
        </ul>   
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app"
            data:{
            	message:"Hello Vue!"
            	User:{
            		name:"xiaoming",
            		id:"001"
        		},
                arr:["01", "02"]
        	}
        })
    </script>
</body>
```

Vue中用到的数据定义在data中

data中可以写复杂类型的数据

渲染复杂类型数据时，遵守js的语法即可

#### 2、本地用法

##### 1.介绍

Vue指令

1.内容绑定，事件绑定

`v-text`

`v-html`

`v-on`基础

2.显示切换，属性绑定

`v-show`

`v-if`

`v-bind`

3.列表循环，表单元素绑定

`v-for`

`v-on`补充

`v-model`

通过Vue实现常见的网页效果

学习Vue指令，以案例巩固知识点

##### 2.`v-text`

设置标签的文本值

```html
<body>
    <div id="app">
        <h2 v-text="message + '!'"></h2>
        <h2>
            Test{{ message + "!"}}
        </h2>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app"
            data:{
            	message:"Hello Vue!"
        	}
        })
    </script>
</body>
```

`v-text`指令的作用是：设置标签的内容

默认写法会替换全部内容，使用差值表达式{{}}可以替换指定内容

##### 3.`v-html`

设置标签的innerHTML

```html
<body>
    <div id="app">
        <p v-html="context"></p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app"
            data:{
            	context:"xiaoming"
            	context:"<a href='#'>xiaoming</a>"
        	}
        })
    </script>
</body>
```

`v-html`指令的作用是：设置元素的innerHTML

内容中有html结构会被解析为标签

`v-text`指令无论内容是什么，只会解析为文本

##### 4.`v-on`基础

为元素绑定事件

```html
<body>
    <div id="app">
        <input type="button" value="事件绑定" v-on:click="dolt">
        <input type="button" value="事件绑定" v-on:monseenter="dolt">
        <input type="button" value="事件绑定" v-on:dblclick="dolt">
        <input type="button" value="事件绑定" @dblclick="dolt">
        
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app"
            methods:{
            	dolt:function(){
            		//逻辑
        		}
        	}
        })
    </script>
</body>
```

`v-on`指令的作用是：为元素绑定事件

事件名不需要on

指令可以简写为@

绑定的方法定义在methods属性中

方法内部通过this关键字可以访问定义在data中数据

##### 5.计数器

1. data中定义数据：比如num

2. methods中添加两个方法：比如add（递增），sub（递减）
3. 使用v-text将num设置span标签
4. 使用v-on将add，sub分别绑定+，-按钮
5. 累加的逻辑：小于10累加，否则提示
6. 递减的逻辑：大于10递减，否则提示

```html
<body>
    <div id="app">
        <div class="input-num">
            <button @click="sub">-</button>
            <span>{{ num }}</span>
            <button @click="add">+</button>
        </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app"
            data:{
            	num:1;
        	},
            methods:{
            	add:function(){
            		if(this.num < 10){
            			this.num++;
            		}else{
                		alert("Max");
            		}
        		},              			 
        		sub:function(){
                    if(this.num > 0){
                        this.num--;
                    }else{
                        alert("Min");
                    }  
                }
            }
        });
    </script>
</body>
```

创建Vue示例时：el（挂载点），data（数据），methods（方法）

`v-on`指令的作用是绑定事件，简写为@

方法中通过this，关键字获取data中的数据

`v-test`指令的作用是：设置元素的文本值，简写为{{}}

##### 6.`v-show`

根据表达值的真假，切换元素的显示和隐藏





