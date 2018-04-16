###linux日常使用技巧
####linux是一项基本的能力，在日常开发中会频繁的使用，但是有时候我们可能也会遗忘，所以在此简单记录一下常用的linux命令。

####文件搜索
查找指定文件名的文件(不区分大小写)
```
find -iname "my project"
```
查找home下的所有空文件
```
find ~ -empty
```
####文本操作
tail 命令默认显示文件最后的10行文本
```
tail test.txt
```
查看日志
```
tail -f /xxx/xxx/xx.log
```
日志过滤
```
tail -f /xxx/xxx/xx.log | grep /接口名 --col
```
取一个请求的日志
```
grep logid /xxx/xxx/xx.log --col
```
####ssh
登录到远程主机
```
$ ssh -l test.example.com
```
调试ssh客户端
```
$ ssh -v -l jsmith remotehost.example.com
```
显示ssh客户端版本
```
$ ssh -V
```
