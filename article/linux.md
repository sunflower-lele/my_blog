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
####ls

列出文件或者目录的信息，目录的信息就是其中包含的文件。

```
# ls [-aAdfFhilnrRSt] file|dir
-a ：列出全部的文件
-d ：仅列出目录本身
-l ：以长数据串行列出，包含文件的属性与权限等等数据
```

####cp

复制。如果源文件有两个以上，目的文件一定要是目录才行

```
-a : 相当于-dr --preserve=all的意思
-d ：若来源文件为链接文件，则复制链接文件属性而非文件本身
-i ：若目标文件已经存在时，覆盖前会先询问
-p ：连同文件属性一同复制
-r ：递归持续复制
```

####rm

删除操作

```
-r ：递归删除
-f ：强制删除文件或者目录
-i ：删除前先询问
```

####mv

对文件或目录重命名或者从一个目录移动到另外一个目录，

```
-f ：强制移动
mv ex3 new1 ：改名为new1
mv /usr/local/* . : 将目录/usr/local中的所有问津移动到当前目录（用.表示）中
```

