# 一. 找到最耗cup的进程
    工具：top（类似windows任务管理器）
    方法：
        1.执行 top 或者 top -c，显示进程运行信息列表
        2.键入大写P，进程按cup使用率排序

# 二. 根据进程pid 找到最耗cup的线程pid
    方法：
        1.执行top -H -p [进程pid] 或者 top -Hp [进程pid]， 显示一个进程的线程运行列表信息
        2.键入大写P，线程按cup使用率排序

# 三. 将线程pid转换为16进制
    工具：printf
    方法：printf “%x” [线程pid]  

# 四. 查看堆栈，找到线程在干嘛
    工具：jstack
    方法：jstack [进程pid] | grep ‘[线程pid的16进制]’ -C5 --color
    注：如果报错“xxpid” : Operation not permitted, 需用应用运行的用户登录执行该命令
    扩展：jstack(查看线程)、jmap(查看内存)和jstat(性能分析)
    1. jstack能得到运行java程序的java stack和native stack的信息。可以轻松得知当前线程的运行情况
    2. jmap得到运行java程序的内存分配的详细情况， jmap -dump:format=b,file=盘符:/XXX.hprof <pid>
       导出jvm内存文件
    3. jstat这是一个比较实用的一个命令，可以观察到classloader，compiler，gc相关信息。可以时时监控资源和性能     
