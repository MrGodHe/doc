# 一、什么是Git

Git是分布式版本控制系统

结构功能图：

![](E:\project\study-doc\doc\image\git\git.jpg)

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

配置

```
git config --list  			查询所有配置
git config -e [--global] 	编辑Git配置文件

设置提交代码用户信息
git config [--global] user.name "[name]" 			
git config [--global] user.email "[email address]"
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
git diff xxx   对比的是工作区与暂存区的改动
```

查看提交日志

```
git log 
git log -n 		"-n"代表日志输出数量
git log -p		"-p"控制输出每个commit具体修改的内容，输出的形式以diff的形式给出
git log --stat 	"--stat":是在git log 的基础上输出文件增删改的统计数据。

注：个人喜欢 -n 再结合其他控制参数来使用！
```

回退commit 版本

```
git reset --hard HEAD^    	当前的版本回退到上一个版本，^几个就是回退上几个
git reset  --hard HEAD~100 	当前的版本回到前100个版本

注：git rest 有三种模式 soft, mixed, hard
1.hard 模式会重置工作目录和暂存区，没有commit的修改会被完全擦掉
2.soft 
3.mixed
```

克隆仓库

```
git clone xxx 
```

分支管理

```
--- 查看分支 ---
git branch 	查看本地分支
git branch -r 查看远程分支
git branch -a 查看所有分支（包括远程和本地）

--- 远程仓库无分支 ---
git checkout -b xxx  新建一个分支
git push origin xxx  推送分支到远程

--- 从远程仓库中拉取新分支到本地 --- 
git checkout -b xxx origin/xxx  

--- 删除分支 ---
git branch -d xxx 			  	删除本地分支
git push origin --delete  xxx	删除远程分支

--- 分支合并 ---
git merge xxx 	表示把xxx分支合并到当前分支上
```

远程仓库同步

```
git fetch  						同步远程仓库所有变动
git remote -v 					显示所有远程仓库信息
git remote add [origin] [url]  	增加一个新的远程仓库，并命名
git push [origin] [branch]		上传本地指定分支到远程仓库
git pull [origin] [branch] 		取回远程仓库的变化，并与本地分支合并
```

bug分支

场景：当我们在开发一个项目，开发到一半且不能提交内容时，突然又接到一个bug修复。

```
git stash 将当前工作现场隐藏起来
git stash save "save message"  加备注
......干修复bug

--- 恢复工作区 ---
git stash list 查看隐藏的工作区列表
1、git stash apply stash@{$num}	恢复，恢复后，stash内容并不删除，你需要使用命令git stash drop来删除
2、git stash pop  stash@{$num}	恢复的同时把stash内容也删除了
注：stash@{$num} 指的第几个隐藏

--- 清除所有缓存 ---
git stash clear
```

# 四、异常问题

