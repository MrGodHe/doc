# curl post 命令

curl  http://ip:port/message/messageList -X POST -H "Content-Type:application/json" -H "XXX:XXXX" -H "" -d '{"XXXX":"XXX","pageNum":1,"pageSize":20}'

# curl GET 命令

curl  http://ip:port/message/messageList?a=1&b=2

注意：在linux系统中& 会使进程系统后台运行，必须对&进行下转义为（\&）
