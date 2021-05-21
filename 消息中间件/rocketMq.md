官网入门地址：http://rocketmq.apache.org/docs/quick-start/

官网下载地址：http://rocketmq.apache.org/release_notes/release-notes-4.7.0/

| 官网文档地址                                                 |
| ------------------------------------------------------------ |
| 英文：https : //github.com/apache/rocketmq/tree/master/docs/en |
| 中文：https : //github.com/apache/rocketmq/tree/master/docs/cn |

# RocketMq 安装

## 一、linux安装

1. 下载最新源版本

   ```
    > unzip rocketmq-all-4.7.0-source-release.zip             //解压
    > cd rocketmq-all-4.7.0/      
    > mvn -Prelease-all -DskipTests clean install -U          //mvn源码打包
    > cd distribution/target/rocketmq-4.7.0/rocketmq-4.7.0    //进入打包文件
   ```

2. 启动服务

   ```
   > nohup sh bin/mqnamesrv &                    //启动
   > tail -f ~/logs/rocketmqlogs/namesrv.log     //查看启动服务日志
    
    The Name Server boot success...             //成功日志示例     
   ```

3. 启动borker

   ```
   > nohup sh bin/mqbroker -n localhost:9876 &    //启动broker
   > tail -f ~/logs/rocketmqlogs/broker.log       //查看broker启动日志
    
    The broker[%s, 172.30.30.233:10911] boot success...   //成功日志示例
   ```

4. 发送和接收消息

   ```
    > export NAMESRV_ADDR=localhost:9876
    > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
          
    SendResult [sendStatus=SEND_OK, msgId= ...]
         
    > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
    
    ConsumeMessageThread_%d Receive New Messages: [MessageExt...]
   ```

5. 关闭服务器

   ```
    > sh bin/mqshutdown broker
    
    The mqbroker(36695) is running...
    Send shutdown request to mqbroker(36695) OK
    
    > sh bin/mqshutdown namesrv
    
    The mqnamesrv(36664) is running...
    Send shutdown request to mqnamesrv(36664) OK 
   ```

   **注意**：roketMq 默认的jvm内存配置比较大，如仅测试机器可以适当调整， 需要修改找到runserver.sh 和 runbroker.sh
        （-server -Xms512m -Xmx512m -Xmn256m）



## 一、 windows安装

1. 下载最新的二进制版本。并将zip文件解压缩到本地磁盘中。如：D:\rocketmq

2. 配置系统环境变量：

   ```
   ROCKETMQ_HOME="D:\rocketmq"
   
   NAMESRV_ADDR="localhost:9876"
   ```

3. 启动服务        

   ```
   .\bin\mqnamesrv.cmd 
   ```

4. 启动broker    

   ```
   .\bin\mqbroker.cmd -n localhost:9876 autoCreateTopicEnable=true
   ```

5. 发送信息 

   ```
    .\bin\tool.cmd  org.apache.rocketmq.example.quickstart.Producer
   ```

6. 接收信息   

   ```
    .\bin\tool.cmd  org.apache.rocketmq.example.quickstart.Consumer
   ```

7. 关闭服务和broker， 关闭windows窗口就行
             