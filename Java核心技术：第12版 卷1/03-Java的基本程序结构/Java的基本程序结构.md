## 第 3 章	Java的基本程序结构

[toc]

> 本章主要介绍如何在Java中实现基本程序设计概念（如数据类型、分支、以及循环）

#### 3.1 一个简单的Java程序

这是原书的示例代码，是一个非常简单的Java程序，作用只是向控制台打印消息。

```java
public class FirstSample
{
   public static void main(String[] args)
   {
      System.out.println("We will not use 'Hello, World!'");
   }
}
```

关于这段Java程序，书中主要是提及了以下几点

* Java区分大小写。

如果出现了Java关键字大小写拼写错误，那么程序将会报错（例如，将main拼写成Main，将String拼写成string），无法运行。

* 关键字public称为访问修饰符（access modifier）

访问修饰符用于控制程序的其他部分对这段代码的访问级别。

* 关键字Class

表明Java程序中的全部内容都包含在类中。在第四章会更多的了解类的概念，现在你只需要将类看作是程序逻辑的一个容器，定义了应用程序的行为。

* 类名

类名应紧跟在关键字Class后面。在Java中类名的定义规则和宽松，类名必须以字母开头，长度没有限制，不能使用Java保留字。









