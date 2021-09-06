# 一、什么是Git

Git是分布式版本控制系统

结构功能图：

![](https://github.com/MrGodHe/doc/blob/master/image/git/git.jpg)

- workspace	工作区
- Index / Stage  暂存区
- Repository 仓库区
- Remote 远程仓库

# 二、安装

官网下载地址：https://git-scm.com/downloads

1、windows 安装

​	下载exe文件，一般默认next 就ok

2、linux 安装

- yum安装 ： yum -y install git
- GitHub上下载最新的源码编译后安装， 优点：使用最新版的git，缺点：太麻烦了

# 三、使用

创建版本库/仓库

```
git init
```

加入暂存区

```
git add xxx   		单文件加入暂存区
git add xxx1 xxx2 	多文件加入暂存区，文件之间以空格隔开
git add .     		所有改变文件加入暂存区，也可用（git add --all）
git xxx/*			添加指定路径下所有文件
git xxx/*.java    	添加指定路径下java格式文件
```

提交到版本库/仓库

``` 
git commit -m  'xxx'  	把暂存区的内容提交到仓库，引号内为提交描述信息内容
git commit -a -m 'xxx'  把暂存区的内容提交到仓库，且包括未添加到暂存区的也提交
git commit --amend  	修改或取消上一次的提交内容
```

查看是否还有未提交内容

```
git status
```

查看某文件具体被修改那些内容

```
git diff xxx
```

