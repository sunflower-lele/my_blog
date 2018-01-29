## git ssh以及代码补全
### git ssh 添加
git 生成ssh

    ssh-keygen -t rsa -C "11@1.com"（此处为邮箱,格式标准即可，本机只会保留最后一次生成的）

添加ssh到id_rsa.pub,并保存
    
    vi ~/.ssh/id_rsa.pub
    
查看id_rsa.pub文件，并把该值添加到gitlab
    
    cat ~/.ssh/id_rsa.pub
    

安装 bash-completion 工具 （已安装就忽略这一步）

 	brew install bash-completion       //brew包管理工具
查看 bash-completion 工具的配置信息

 	brew info bash-completion 

 	=======复制信息中的下面这句话====（以系统输出为准）======

 	if [ -f $(brew --prefix)/etc/bash_completion ]; then
 		. $(brew --prefix)/etc/bash_completion
 	fi
把第二步的复制出来的代码添加入 .bash_profile 文件中并保存

 	vi ~/.bash_profile 
克隆git源码

 	git clone https://github.com/git/git.git	
找到 git-completion.bash 脚本并复制到家目录

 	cp git/contrib/completion/git-completion.bash ~/.git-completion.bash
修改 .bashrc文件 （没有则创建该文件）

 	vi  ~/.bashrc
 	
 	++++++++添加下面的代码+++++++++++
 	
 	source ~/.git-completion.bash
重启终端

#### 试试你的新git吧~