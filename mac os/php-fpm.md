### 启动php-fpm的方法
#### 命令启动:
* sudo /usr/local/opt/php56/sbin/php-fpm --fpm-config /usr/local/etc/php/5.6/php-fpm.conf

#### 设置php-fpm的开机自启动：
1. 新建一个php-fpm的开机自启动文件
* sudo vim /Library/LaunchDaemons/com.php-fpm.plist
2. 添加com.php-fpm.plist文件,文件内容请参照：
* [链接](https://www.cnblogs.com/52php/p/5684348.html)
3. 注册这个plist文件到系统服务，执行命令如下：
* sudo launchctl load -w +文件名
4. 下载命令:
* sudo launchctl unload -w /Library/LaunchDaemons/com.php-fpm.plist
5. 更多信息请参照：
* [链接](https://newsn.net/say/php-fpm-autorun.html)
