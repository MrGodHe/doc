# redis 线上CPU 占满

1.redis连接数过高

2.数据持久化导致的阻塞

3.主从存在频繁全量同步

4.value值过大

5.redis慢查询（如：使用keys命令）