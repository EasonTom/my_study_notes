## 简介
linux系统一般自带python2，因为有的系统文件会用到python2，而现在不管是学习python还是跑代码都首选python3，但是python2也还是需要，此时就需要python2与python3，pip2与pip3共存。

## 1 安装python3

首先需要一些依赖包：

    yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
    
    yum install gcc -y
    
然后下载python安装包（或者去官网下载然后上传到服务器）：

    wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz

解压安装包，配置安装目录：

    mkdir /usr/local/python3  # 新建文件夹安装python
    tar -xvf Python-3.6.5.tgz
    cd /usr/local/Python-3.6.5/
    ./configure --prefix=/usr/local/python3  # 配置安装目录
    make & make install  # 编译源码并安装
    
安装完成后，创建软连接：

    ln -s /usr/local/python3/bin/python3  /usr/bin/python3
    
如果运行python3命令报错，缺少.so文件，我们需要进行如下操作： 

    cp -R /usr/local/python3.5/lib/* /usr/lib64/ 
    
    
## 2 安装pip与setuptools

安装python时如果系统没有自动安装pip，需要我们手动安装，可以如下操作：

由于pip的安装需要依赖于setuptools，所以首先需要安装setuptools：

    wget --no-check-certificate  https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26  # 下载安装包

    tar -zxvf setuptools-19.6.tar.gz  # 解压

    cd setuptools-19.6  # 进入解压后的目录

    python3 setup.py build  # 编译

    python3 setup.py install  # 安装
    
setuptools安装完成后就可以安装pip了

    wget --no-check-certificate  https://pypi.python.org/packages/source/p/pip/pip-8.0.2.tar.gz#md5=3a73c4188f8dbad6a1e6f6d44d117eeb

    tar -zxvf pip-8.0.2.tar.gz

    cd pip-8.0.2

    python3 setup.py build

    python3 setup.py install

python2安装pip执行同样的步骤，只是将编译和安装的工具换成python2。

安装完成后，可以直接在python的目录下找到pip，输入pip3 -V 或者 pip2 -V 查看pip信息，如果pip命令无法使用，可以建立软连接。