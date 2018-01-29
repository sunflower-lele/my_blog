### redis服务
#### redis启动和关闭
1. 首先进入redis目录下
* cd /usr/local/etc/redis/
2. 启动redis服务
* redis-server
3. 重新打开一个终端，启动redis客户端
* redis-cli
4. 指定redis地址和端口号启动
* redis-cli -h 127.0.0.1 -p 6379（此处为redis 默认端口号）
5. 关闭redis服务
* 命令关闭
* redis-cli shutdown
* 强行关闭
* kill -9 [pid] （pid为要杀死的进程号）
