# 配置SSH KEY

一、设置git的用户名和邮箱地址

```
git config --global user.name "xxxx"
git config --global user.email "xxx@email.com"
```

二、生成key 

打开git bash 命令界面

```
ssh-keygen -t rsa -C "xxx@qq.com" 生产公钥
```

找到 .ssh 文件夹下的 id_rsa.pub文件

三、配置github key

登录github，头像-> settings -SSH and GPG keys-->New SSH Key， 复制公钥到里面即可