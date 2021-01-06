从https://repo.continuum.io/archive/index.html上下载对应版本的Anaconda。比如我选择安装 Anaconda3-5.0.1-Linux-x86_64.sh，(对应python3.6，x64系统)可以采用下列命令。

    wget https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh

   

下载完成成后直接进行安装：

    bash Anaconda3-5.0.1-Linux-x86_64.sh


​    
安装过程中会需要不断回车来阅读并同意license。安装路径默认为用户目录(可以自己指定)，最后需要确认将路径加入用户的.bashrc中。


最后，立即使路径生效，需要在用户目录下执行：

    source .bashrc

   

此时，打开python就是最新的3.6版本了。为了保持更新，可以在终端中执行：

    conda upgrade --all
