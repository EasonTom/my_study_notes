# Git版本控制工具

Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。

[官网](https://git-scm.com/)

最早Git是在Linux上开发的，很长一段时间内，Git也只能在Linux和Unix系统上跑。不过，慢慢地有人把它移植到了Windows上。现在，Git可以在Linux、Unix、Mac和Windows这几大平台上正常运行了。

## 1 在Linux安装
首先，你可以试着输入git，看看系统有没有安装Git

如果你安装了Git，输入git可以查看一些命令。

如果你碰巧用Debian或Ubuntu Linux，通过`apt-get`就可以直接完成Git的安装，非常简单。

    sudo apt-get install git
    
如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入这几个命令安装就好了。

    ./config
    make
    sudo make install
    
## 2 在Windows上安装

在Windows上使用Git，可以从Git官网直接下载[安装程序](https://git-scm.com/downloads)，（网速慢的同学请移步[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)），然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

## 3 配置

安装完成后，还需要最后一步设置，在命令行输入：

    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@example.com"
    
因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

