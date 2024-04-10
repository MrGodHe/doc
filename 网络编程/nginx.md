# nginx 安装

官方下载地址：http://nginx.org/en/download.html

\##nginx -基础命令

http://blog.sina.com.cn/s/blog_69230fe101013g3n.html 

**linux 命令**

```
nginx -s stop 强制关闭 
nginx -s quit 安全关闭 
nginx -s reload 改变配置文件的时候，重启nginx工作进程，来时配置文件生效 
nginx -s reopen 打开日志文件 
```

**windows命令**

```
tasklist | findstr nginx  查找nginx进程
taskkill /F /IM nginx.exe 停止所有进程

nginx  启动
nginx -t 验证配置正确
nginx -s stop
nginx -s reload
```

