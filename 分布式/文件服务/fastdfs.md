参考地址   https://github.com/happyfish100

#### 什么是fastdfs

FastDFS是一款开源的分布式文件系统，功能主要包括：文件存储、文件同步、文件访问（文件上传、文件下载）等，解决了文件大容量存储和高性能访问的问题。FastDFS特别适合以文件为载体的在线服务，如图片、视频、文档等等服务

```
FastDFS特点：
    1）分组存储，简单灵活；
    2）对等结构，不存在单点；
    3）文件ID由FastDFS生成，作为文件访问凭证。FastDFS不需要传统的name server或meta server；
    4）大、中、小文件均可以很好支持，可以存储海量小文件；
    5）一台storage支持多块磁盘，支持单盘数据恢复；
    6）提供了nginx扩展模块，可以和nginx无缝衔接；
    7）支持多线程方式上传和下载文件，支持断点续传；
    8）存储服务器上可以保存文件附加属性。
```

#### fastdfs部署

##### 一、环境准备

1. **安装gcc**

   使用yum命令安装gcc和gcc-c++，编译时需要使用，Linux系统下使用gcc编译C语言的代码，使用g++编译C++的代码。

   ```
   yum install -y gcc gcc-c++
   ```

2. **安装libevent**

   使用yum安装libevent，运行时需要

   ```
   yum -y install libevent
   ```

##### 二、fastdfs安装

1. **安装libfastcommon**

   libfastcommon是由FastDFS官方提供的，在GitHub开源的一个C基础库，包含了FastDFS运行所需要的一些基础库。它提供了ini文件解析、logger、64位唯一整数生成器、字符串处理、socket封装、对象池、skiplist、定时任务调度器、时间轮等等

   ```
   # github address:  https://github.com/happyfish100/libfastcommon.git
   # gitee address:   https://gitee.com/fastdfs100/libfastcommon.git
   
   git clone https://github.com/happyfish100/libfastcommon.git
   cd libfastcommon
   git checkout V1.0.71
   ./make.sh clean && ./make.sh && ./make.sh install
   ```

2. **安装libserverframe**

   FastDFS V6.09 发布，主要改进：引入网络框架库 libserverframe，替换原有的 tracker nio 和 storage nio 两个模块

   ```
   # github address:  https://github.com/happyfish100/libserverframe.git
   # gitee address:   https://gitee.com/fastdfs100/libserverframe.git
   
   git clone https://github.com/happyfish100/libserverframe.git
   cd libserverframe
   git checkout V1.2.1
   ./make.sh clean && ./make.sh && ./make.sh install
   ```

3. **安装fastDFS**

   ```
   # github address:  https://github.com/happyfish100/fastdfs.git
   # gitee address:   https://gitee.com/fastdfs100/fastdfs.git
   
   git clone https://github.com/happyfish100/fastdfs.git
   cd fastdfs
   git checkout V6.11.0
   ./make.sh clean && ./make.sh && ./make.sh install
   ```

4. **设置配置文件**

   ```
   ./setup.sh /etc/fdfs
   
   
   ```

5. **启动应用服务**

   ```
   # start the tracker server:
   /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart 
   # 查询启动状态
   /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf status
   
   # start the storage server:
   /usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart
   
   # (optional) in Linux, you can start fdfs_trackerd and fdfs_storaged as a service:
   /sbin/service fdfs_trackerd restart
   /sbin/service fdfs_storaged restart
   
   ```

6. **启动监视服务**

   ```
   /usr/bin/fdfs_monitor /etc/fdfs/client.conf
   ```

7. **测试**

   ```
   # for example, upload a file for test:
   /usr/bin/fdfs_test /etc/fdfs/client.conf upload  xx文件
   ```

##### 三、fastdfs-nginx-module安装和配置

参考1： https://www.cnblogs.com/xiaoshiwang/p/11453625.html

参考2：https://zhuanlan.zhihu.com/p/615399811?utm_id=0

