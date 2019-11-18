针对两个账户同时在一台电脑上正常使用git功能，通过查找资料找到了如下解决办法：
1. 首先将全局设置的账号和邮箱去掉，因为提交项目时会首先使用全局的账号（如果没有设置，可跳过）：


    git config --global --unset user.name
    git config --global --unset user.email
    git config --global -l # 查看全局设置
    
也可以在用户主目录下的`.ssh/config`文件下修改全局配置。

2. 首先创建两对公私钥，分别名不同的名字，都放在~/.ssh文件夹下。


    ssh-keygen -t rsa -C "youremail@example.com"

点击回车后会出现

    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/tys/.ssh/id_rsa): /home/tys/.ssh/gitee_rsa # 在这里自定义名称

创建两对密钥，然后分别将各自的公钥添加到远程仓库中。

3. 在~/.ssh/config文件中添加不同的git账号信息，config文件不存在就自己创建。

```
Host gitee.com # 自定义名称，可设置别名
hostname gitee.com # 域名
User EasonTom # 用户名
IdentityFile /home/tys/.ssh/gitee_rsa # 私钥地址

Host code.thecover.cn
hostname code.thecover.cn
User eason
IdentityFile /home/tys/.ssh/id_rsa

```


4. 在对应的工程目录下设置该工程自己的 git config
下面是 gitee项目根目录下的设置
    
    
    git config user.name EasonTom
    git config user.emall 1121651051@qq.com

配置之后就可以在不同的工程目录使用不同的 git config ，可以愉快的玩耍了。

测试连接：

    ssh -T git@host
    
    tys@tys:~$ ssh -T git@gitee.com
    Hi EasonTom! You've successfully authenticated, but GITEE.COM does not provide shell access.

