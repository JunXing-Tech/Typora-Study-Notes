# Git

---

[toc]

### 1. Git概述

>Git is a [free and open source](https://git-scm.com/about/free-and-open-source) distributed version control system designed to handle everything from small to very large projects with speed and efficiency.



### 2. Git安装

官网安装：<http://git-scm.com>



### 3. Git常用命令

* 查看git系统版本   ==git --version==

* 设置用户签名 ==git config --global user.name 用户名==

* 设置用户签名 ==git config --global user.email 邮箱==
* 初始化 ==git init==
* 查看本地库状态 ==git status==
* 添加到暂存区 ==git add 文件名==
* > 只需在`git add` 后面加一个`.`，这种办法适用于将项目中所有文件或文件夹上传。
* 删除暂存区文件 ==git rm --cached 文件名==
* 提交到本地库 ==git commit -m "日志信息" 文件名==
* 查看详细日志 ==git log==
* 查看历史记录 ==git reflog==
* 版本穿梭 ==git reset --hard 版本号==

* 修改文件 ==vim指令==

* git add . 失效

* 删除缓存 git rm -f --cached / git add .



### 4. Git分支操作 

* 创建分支 ==git branch==
* 查看分支 ==git branch -v==
* 切换分支 ==git checkout 分支名== 

* 正常合并：把指定的分支合并到当前分支上 ==git merge 分支名==

* 冲突合并：合并分支时，两个分支在**同一个文件在同一个位置**有两套完全不同的修改。Git无法替使用者决定使用哪一个，则必须**人为决定**新代码内容。

  4.5.1.解决冲突

  ​	1）编辑有冲突的文件（Vim），删除特殊符号，决定要	      使用的内容。

  ​	2）添加到暂存区

  ​	3）执行提交**（注意：此时使用git commit 命令时不能带文件名）**



### 5. Git团队协作机制

#### 	5.1团队内协作

![Git 和 GitHub 快速入门_Nice2cu_Code的博客-CSDN博客](https://img-blog.csdnimg.cn/20201201205422229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70)

#### 	5.2跨团队协作

![Git 和 GitHub 快速入门_Nice2cu_Code的博客-CSDN博客](https://img-blog.csdnimg.cn/20201201205427710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70)



### 6. GitHub操作

#### 6.1远程仓库操作

* 查看当前所有远程地址别名 ==git remote -v==

* 起别名 ==git remote add 别名 远程地址==

* 推送本地分支上的内容到远程仓库 ==git push 别名 分支==

* 拉取远程仓库的内容到本地分支==git pull 别名 分支==

* 将远程仓库的内容克隆到本地 ==git clone 远程地址==

  6.4.1 clone 会做如下操作 1.拉取代码 2.初始化本地仓库 3.创建别名

* 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并 ==git pull 远程库地址别名 远程分支名==



### 7. IDEA集成Git

#### 7.1配置Git忽略文件

> 文件存放位置建议放在用户家目录下

创建忽略规则文件**git.ignore**（建议/前缀名）

git.ignore文件模板内容

```git.ignore
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

#Package Files
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

.classpath
.project
.setting
target
.idea
*.iml
```

.gitconfig文件内追加配置

```.gitconfig
[core]git
	excludesfile = C:/Users/UserName/git.ignore
```



创建分支

切换分支

合并分支

解决冲突

### 7. IDEA集成GitHub

* 操作简单

### 8. Gitee

* 操作简单

### 9. GitLab

* 需要再学

[点击返回文首](#Git)

![image-20230320213833334](C:\Users\JunXing\AppData\Roaming\Typora\typora-user-images\image-20230320213833334.png)







