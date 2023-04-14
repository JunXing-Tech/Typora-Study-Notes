# Maven（基础）

[toc]

#### Maven简介

##### Maven是什么

* Maven的本质是一个项目管理工具，将项目开发和管理过程抽象成一个项目对象模型（POM）
* POM(Project Object Model)：项目对象模型

<img src="C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230411150942574.png" alt="image-20230411150942574" style="zoom: 50%;" />

##### Maven的作用

* 项目构建：提供标准的、跨平台的自动化项目构建方式
* 依赖管理：方便快捷的管理项目依赖的资源（jar包），避免资源间的版本冲突问题
* 统一开发结构：提供标准的、统一的项目结构

#### Maven下载与安装

1.官网下载

2.安装

3.Maven环境变量配置

* 依赖Java，需要配置JAVA_HOME
* 设置MAVEN自身的运行环境，需要配置MAVEN_HOME

#### Maven基本概念

##### 仓库

* 仓库：用于存储资源，包含各种jar包
* 仓库分类
  * 本地仓库：自己电脑上存储资源的仓库，连接远程仓库获取资源
  * 远程仓库：非本机电脑上的仓库，为本地仓库提供资源
    * 中央仓库：Maven团队维护，存储所有资源仓库
    * 私服：部门/公司范围内存储资源的仓库，从中央仓库获取资源
* 私服的作用：
  * 保存具有版权的资源，包含购买或自主研发的jar
    * 中央仓库中的jar都是开源的，不能存储具有版权的资源
  * 一定范围共享资源，仅对内部开发，不对外共享

##### 坐标

###### 什么是坐标？

* Maven中的坐标用于描述仓库中资源的位置

###### Maven坐标主要组成

* groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：org.mybatis）
* artifactId：定义当前Maven项目名称（通常是模块名称，例如CRM、SMS）
* version：定义当前项目版本号
* packaging：定义该项目的打包方式

###### Maven坐标的作用

* 使用唯一标识，唯一性定位资源位置，通过该标识可以将资源的识别与下载工作交由机器完成

##### 仓库配置

###### 本地仓库配置

Maven启动后，会自动保存下载的资源到本地仓库

* 默认位置

```xml
<localRepository>${user.home}/.m2/repository</localRepository>
<!--当前目录位置为登录用户名所在目录的.m2文件夹中-->
```

* 自定义位置

```xml
<localRepository>D:\maven\repository</localRepository>
```

###### 远程仓库配置

* Maven默认连接的仓库位置

```xml
<repositories>
    <repository>
        <id>centeral</id>
        <name>Central Repository</name>
        <url>https://repo.maven.apache.org/maven2</url>
        <layout>default</layout>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

###### 镜像仓库配置

在setting文件中配置阿里云镜像仓库

```xml
<mirrors>
    <!--配置具体的仓库下的镜像-->
    <mirror>
        <!--此镜像的唯一标识符，用来区别不同的mirror元素-->
        <id>nexus-aliyun</id>
        <!--对哪种仓库进行镜像，简单说就是替代哪个仓库-->
        <mirrorOf>central</mirrorOf>
        <!--镜像名称-->
        <name>Nexus aliyun</name>
        <!--镜像url-->
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
</mirrors>
```

##### 全局setting与用户setting区别

1. 全局setting（settings.xml）是Maven的全局配置文件，通常位于Maven安装目录的conf文件夹下，用于配置所有Maven项目共享的配置信息，例如镜像库、代理、全局属性等等。这个文件的修改需要管理员权限。
2. 用户setting（settings.xml）是Maven的用户配置文件，通常位于用户的Home目录下的`.m2`文件夹中，用于配置用户特定的配置信息，例如仓库地址、认证信息等等。这个文件的修改只会影响当前用户的Maven项目。

总的来说，全局setting是适用于所有用户的默认设置，而用户setting是适用于个人的自定义设置。如果您需要更改所有用户的设置，例如设置公司内部私有的仓库地址，则需要修改全局setting；如果您只需要更改自己的设置，则需要修改用户setting。

#### 第一个Maven项目（手工制作）

##### Maven项目结构

1. 创建一个新的文件夹，用作项目的根目录。
2. 在根目录下创建以下文件夹结构：

```markdown
project
|-- pom.xml
`-- src
    |-- main
    |   |-- java
    |   |   `-- package
    |   |       `-- MyClass.java
    |   |-- resources
    |   |   `-- myconfig.properties
    |   `-- webapp
    |       |-- WEB-INF
    |       |   `-- web.xml
    |       `-- index.jsp
    `-- test
        |-- java
        |   `-- package
        |       `-- MyClassTest.java
        `-- resources
            `-- testconfig.properties

上面的项目结构有以下几个部分：
pom.xml：Maven 项目的核心文件，它包含了项目的元数据和构建配置信息，例如依赖、插件、构建目标等。
src/main/java：主要 Java 代码的目录，通常包含一个或多个 Java 包，每个包下面可能包含一个或多个 Java 类。
src/main/resources：主要资源文件的目录，例如配置文件、属性文件、XML 文件等。
src/main/webapp：Web 应用程序的根目录，包含 Web 应用程序的所有资源，例如 JSP、HTML、CSS、JavaScript 等。
src/test/java：测试 Java 代码的目录，通常包含一个或多个测试包，每个测试包下面可能包含一个或多个测试类。
src/test/resources：测试资源文件的目录，例如测试配置文件、测试数据文件等。
```

3. 在根目录下创建一个 pom.xml 文件。这个文件是 Maven 项目的核心，定义了项目的依赖、插件、版本等信息。以下是一个示例 pom.xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>1.0.0</version>
    
    <dependencies>
        <!-- 添加所需的依赖 -->
    </dependencies>
    
    <build>
        <plugins>
            <!-- 添加所需的插件 -->
        </plugins>
    </build>
    
</project>
```

在这个 pom.xml 文件中，您需要指定项目的 groupId、artifactId 和 version，以及任何其他的依赖和插件。

4. 在 src/main/java 目录下创建您的 Java 代码。例如，您可以创建一个名为 `com.example.App` 的类，并在其中添加一个 `main` 方法。

```java
package com.example;

public class App {
    public static void main(String[] args) {
        System.out.println("Hello, Maven!");
    }
}
```

##### Maven项目构建

在 Maven 中，项目构建是通过执行 Maven 命令来完成的。以下是构建一个 Maven 项目的常用命令：

1. `mvn clean`：清除之前构建的结果，包括编译的类、打包的 jar 等。
2. `mvn compile`：编译项目的 Java 代码。
3. `mvn package`：打包项目，生成可执行的 jar、war 或 ear 文件。
4. `mvn install`：安装项目到本地 Maven 仓库，以便其他项目可以依赖它。
5. `mvn test`：运行项目的测试。
6. `mvn verify`：验证项目的正确性。
7. `mvn deploy`：将打包好的项目部署到远程 Maven 仓库，以便其他项目可以依赖它。

这些命令是 Maven 中最常用的命令，通过它们可以完成大部分项目构建的任务。在执行这些命令之前，需要先进入项目的根目录，然后在命令行中输入相应的命令即可。

##### 插件插件Maven工程

1. 打开命令行终端，并进入您想要创建Maven项目的目录。
2. 运行以下命令来创建一个新的Maven项目：

```bash
mvn archetype:generate 
	-DgroupId={project-packaging}
	-DartifactId={project-name}
	-DarchetypeArtifactId=maven-archetype-quickstart 
	-DinteractiveMode=false
```

#### 第一个Maven项目（IDEA生成）

Maven环境配置

Maven项目创建

Maven命令执行

原型创建Maven项目

原型创建Web项目

Tomcat7插件安装

运行web项目

#### 依赖管理

##### 依赖配置

* 依赖指当前项目运行所需jar，一个项目可以设置多个依赖

```xml
<!--设置当前项目所依赖的所有jar-->
<dependencies>
    <!--设置具体的依赖-->
    <dependency>
        <!--依赖所属群组id-->
        <groupId>groupId</groupId>
        <!--依赖所属项目id-->
        <artifactId>artifactId</artifactId>
        <!--依赖版本号-->
        <version>version</version>
    </dependency>
</dependencies>
```

##### 依赖传递

###### 依赖具有传递性

* 直接依赖：在当前项目中通过依赖配置建立的依赖关系
* 间接依赖：被资源的资源如果依赖其他资源，当前项目间接依赖其他资源

###### 依赖传递冲突问题

* 路径优先：当依赖中出现相同的资源时，层级越深，优先级越低，层次越浅，优先级越高
* 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
* 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的

##### 可选依赖（不透明）

* 可选依赖指对外隐藏当前所依赖的资源--不透明

```xml
<dependency>
        <groupId>groupId</groupId>
        <artifactId>artifactId</artifactId>
        <version>version</version>
    	<!--optional - true - 透明 / 默认 - false-->
    	<optional>true</optional>
</dependency>
```

##### 排除依赖（不需要）

* 排除依赖指主动断开依赖的资源，被排除的资源无需指定版本--不需要

```xml
<dependency>
    <groupId>groupId</groupId>
    <artifactId>artifactId</artifactId>
    <version>version</version>
    <exclusions>
        <exclusion>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

##### 依赖范围

* 依赖的jar默认情况可以在任何地方使用，可以通过scope标签设定其作用范围
* 作用范围
  * 主程序范围有效（main文件夹范围内）
  * 测试程序范围有效（test文件夹范围内）
  * 是否参与打包（package指令范围内）

| scope           | 主打包 | 测试代码 | 打包 | 范例        |
| --------------- | ------ | -------- | ---- | ----------- |
| compile（默认） | Y      | Y        | Y    | log4j       |
| test            |        | Y        |      | junit       |
| provided        | Y      | Y        |      | servlet-api |
| runtime         |        |          | Y    | hdbc        |

###### 依赖范围传递性

* 带有依赖范围的资源在进行传递时，作用范围将受到影响

|          | compile | test | provided | runtime | 直接依赖 |
| -------- | ------- | ---- | -------- | ------- | -------- |
| compile  | compile | test | provided | runtime |          |
| test     |         |      |          |         |          |
| provided |         |      |          |         |          |
| runtime  | runtime | test | provided | runtime |          |
| 间接依赖 |         |      |          |         |          |

##### 生命周期与插件

##### 构建生命周期

项目构建生命周期

* Maven构建生命周期描述的是一次构建过程经历的多少事件

```markdown
compile -> test-compile -> test -> package -> install
```

* Maven对项目构建的生命周期划分为3套
  * clean：清理工作
  * default：核心工作，例如编译，测试，打包，部署等
  * site：产生报告，发布站点等
* clean生命周期
  * pre-clean：执行清理前的准备工作。
  * clean：清理项目，删除生成的目录和文件。
  * post-clean：清理后的工作，例如生成一些报告或者日志。
* default构建生命周期
  * validate：验证项目是否正确并且所有必要的信息可用。
  * initialize：初始化构建环境，例如设置构建版本号等。
  * generate-sources：生成源代码，例如使用JavaCC等。
  * process-sources：处理源代码，例如使用JavaDoc或者其他代码生成工具。
  * generate-resources：生成资源文件，例如使用Maven Resource Plugin。
  * process-resources：处理资源文件，例如复制资源文件到输出目录。
  * compile：编译源代码。
  * process-classes：处理编译后的代码，例如进行字节码增强等。
  * generate-test-sources：生成测试代码。
  * process-test-sources：处理测试代码，例如使用JavaDoc或者其他代码生成工具。
  * generate-test-resources：生成测试资源文件。
  * process-test-resources：处理测试资源文件，例如复制资源文件到输出目录。
  * test-compile：编译测试代码。
  * process-test-classes：处理编译后的测试代码，例如进行字节码增强等。
  * test：执行测试。
  * prepare-package：准备打包阶段，例如生成一些额外的文件或者目录。
  * package：打包，例如生成JAR或者WAR文件。
  * pre-integration-test：执行集成测试前的准备工作。
  * integration-test：执行集成测试。
  * post-integration-test：执行集成测试后的工作，例如生成一些报告或者日志。
  * verify：验证构建的结果是否正确。
  * install：安装构建结果到本地Maven仓库。
  * deploy：部署构建结果到远程Maven仓库。

* site构建生命周期
  * pre-site：执行站点生成前的准备工作。
  * site：生成站点文档。
  * post-site：执行站点生成后的工作，例如打开浏览器查看生成的站点。
  * site-deploy：将站点文档部署到远程服务器。

##### 插件

* 插件与生命周期内的阶段绑定，在执行到对应生命周期时执行对应的插件功能
* 默认Maven在各个生命周期上绑定有预设的功能
* 通过插件可以自定义其他功能

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>2.2.1</version>
            <executions>
                <execution>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                    <phase>generate-test-resources</phase>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```



[返回文首](#Maven（基础）)