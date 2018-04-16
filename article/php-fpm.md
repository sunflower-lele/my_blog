### php-fpm开机自启动
命令启动:
    
    sudo /usr/local/opt/php56/sbin/php-fpm --fpm-config /usr/local/etc/php/5.6/php-fpm.conf

### 设置php-fpm的开机自启动
新建一个php-fpm的开机自启动文件
    
    sudo vim /Library/LaunchDaemons/com.php-fpm.plist
添加com.php-fpm.plist文件,文件内容请参照下文或者链接：[链接](https://www.cnblogs.com/52php/p/5684348.html)
    
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
            <key>Label</key>
            <string>com.php-fpm.plist</string>
            <key>ProgramArguments</key>
            <array>
                    <string>/usr/local/opt/php56/sbin/php-fpm</string>
            </array>
            <key>RunAtLoad</key>
            <true/>
            <key>KeepAlive</key>
            <true/>
    </dict>
    </plist>

注册这个plist文件到系统服务，执行命令如下：
    
    sudo launchctl load -w +文件名
卸载命令:
    
    sudo launchctl unload -w /Library/LaunchDaemons/com.php-fpm.plist
更多信息请参照：
[链接](https://newsn.net/say/php-fpm-autorun.html)
