## nignx 相关操作
#### nginx 启动：
* nginx -c /usr/local/etc/nginx/nginx.conf（此处是nginx的配置问价位置）
#### nginx 重启：
* nginx -s reload
#### 设置nginx开机自启动：
1. 新建一个Nginx的开机自启动文件
* sudo vim /Library/LaunchDaemons/com.nginx.plist
2. 添加com.nginx.plist文件,文件内容请参照：
* [链接:](https://www.cnblogs.com/52php/p/5684348.html)
3. 注册这个plist文件到系统服务，执行命令如下：
* sudo launchctl load -w +文件名
4. 下载命令:
* sudo launchctl unload -w /Library/LaunchDaemons/com.nginx.plist
5. 更多信息请参照:
* [链接:](https://newsn.net/say/php-fpm-autorun.html)