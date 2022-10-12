hadoop的分布式文件系统HDFS

```
#创建文件目录
hadoop fs -mkdir -p /xx/xx
#上传文件到hdfs
hadoop fs -put /本地目录/xx.txt /分布式目录
#删除文件夹
hadoop fs -rm -r /xx
```

