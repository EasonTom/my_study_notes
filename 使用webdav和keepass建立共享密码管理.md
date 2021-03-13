## 1 webdav

WebDAV （Web-based Distributed Authoring and Versioning） 一种基于 HTTP 1.1协议的通信协议。它扩展了HTTP 1.1，在GET、POST、HEAD等几个HTTP标准方法以外添加了一些新的方法，使应用程序可对Web Server直接读写，并支持写文件锁定(Locking)及解锁(Unlock)，还可以支持文件的版本控制。

由于keePass不支持云同步，不方便将各个设备的密码文件整合，因此可以搭建一个WebDAV服务器，将秘钥文件放在其中，在各个终端都访问同一个文件，来达到同步的目的。

### 1.1 Ubuntu搭建共享服务器

```
# 安装Apache2服务器
sudo apt-get install -y apache2

# 开启Apache2中对WebDav协议的支持 (记住最好在用户目录下执行否则报错)
cd ~
sudo a2enmod dav
sudo a2enmod dav_fs

# 创建共享目录并修改权限
sudo mkdir -p /var/www/webdav
sudo chown -R www-data:www-data  /var/www/webdav

# 创建WebDav的访问用户数据库，顺便创建用户`pi`，之后使用该用户登陆
sudo htpasswd -c /etc/apache2/webdav.password pi
# 创建guest用户
#sudo htpasswd /etc/apache2/webdav.password guest

# 修改用户数据库访问权限
sudo chown root:www-data /etc/apache2/webdav.password
sudo chmod 640 /etc/apache2/webdav.password

# 打开默认配置文件
sudo vim /etc/apache2/sites-available/000-default.conf

# 全部替换为以下内容（记得先备份）：

Alias /webdav  /var/www/webdav

<Location /webdav>
 Options Indexes
 DAV On
 AuthType Basic
 AuthName "webdav"
 AuthUserFile /etc/apache2/webdav.password
 Require valid-user
 </Location>

# 重启Apache2服务器
sudo systemctl restart apache2
# 或
sudo /etc/init.d/apache2 reload
```

### 1.2 centos搭建共享服务器

如果服务器没有则先安装apache ，yum install httpd ，Apache2.4是默认开启WebDAV的, 可以查看 /etc/httpd/conf.modules.d目录下的dav.conf配置文件如果没有此文件，可以把以下代码加到自定义的配置文件中以保证开启WebDAV服务

    LoadModule dav_module modules/mod_dav.so
    LoadModule dav_fs_module modules/mod_dav_fs.so
    LoadModule dav_lock_module modules/mod_dav_lock.so

在默认配置文件为WebDAV服务打开一个监听端口，以6666为例，并在conf.d文件夹中为WebDAV服务单独创建一个配置文件。

```
<VirtualHost *:6666>
   ServerAdmin name
    ServerName name.cn
    # 存放文件的位置
    DocumentRoot /data/webdav
    # 日志文件的位置
    ErrorLog /var/log/httpd/error.log
    CustomLog /var/log/httpd/access.log combined
    # 访问路径
    Alias /dav /data/webdav
<Directory "/data/webdav">
    # 访问权限
    Options Indexes MultiViews
    AllowOverride None
    Order allow,deny
    allow from all
</Directory>
<Location />
    # 验证密码
    DAV On
    AuthType Basic
    AuthName "WebDAV-Server"
    
    # 这是登录验证的密码文件  
    AuthUserFile "/etc/httpd/users.pwd"
    
    Require valid-user
</Location>
</VirtualHost>
```

我们需要创建一个用来验证登录的密码文件，存放在配置文件所规定的目录中 /etc/httpd/users.pwd。 创建密码需要用到一个htpasswd工具，这个工具是Apache内置的，不需要单独安装。
htpasswd -c /etc/httpd/users.pwd username 之后需要自己设定一个密码输入两次，便会在该位置创建WebDAV密码文件。


### 1.3 访问共享路径
#### 1.3.1 浏览器访问
服务器搭建好后，就可以在其他地方访问了，首先在浏览器中访问，输入`http://服务器ip/路径`，然后输入刚刚建立的用户名和密码进入共享目录。

浏览器上：随便什么设备，只要是个浏览器就能支持。可以在线播放常用视频，直接打开图片浏览。但是不能上传。

注：如果页面提示网页不存在，可能是端口没开，需要将服务器端口映射出来，如果没指定默认是80端口。


#### 1.3.2 磁盘映射

网页里只能像FTP一样显示文件目录和下载文件。
如果要正常使用，我们需要把它映射为本地目录才行：

Mac上：在Finder中用CMD+K打开连接服务器选项，输入http://IP地址/webdav，输入Webdav创建过的用户名密码来完成映射。
iPhone上：安装网盘访问最强的Readdle Documents，添加WebDav服务，输入信息后就可以访问。直接看文档、看视频、听歌都行。

Windows上：比较麻烦的是，Win7以上默认只支持HTTPS的网络驱动器，做为HTTP的WebDav是不能连的。所以要修改Windows注册表，让它支持HTTP。方法入下：

开始菜单 -> 运行 -> 输入regedit 并按回车，就打开了注册表
注册表中找到HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WebClient\Parameters\BasicAuthLevel这个项目，把值改为2(支持http和https)。
开始菜单 -> 运行 -> 输入cmd 并按回车，打开命令行
输入net stop webclient并按回车，停止网络客户端
输入net start webclient并按回车，开启网络客户端
然后在文件夹菜单中找到映射网络驱动器，输入网址http://IP地址/webdav或\\IP地址\webdav，然后输入用户名密码，就能映射成功了。

注：http协议默认是80端口，如果是自己指定的端口需要在ip后面加上端口ip:端口。


linux将webdav共享文件夹挂在到磁盘（linux系统下安装keepassx，该软件不能直接打开服务器上的数据库，需要将共享文件夹映射到本地磁盘然后使用软件打开数据库）：

    apt-get install davfs2
    mkdir /mnt/webdav
    mount -t davfs https://webdav.yandex.ru /mnt/webdav
    Please enter the username to authenticate with server
    https://webdav.yandex.ru or hit enter for none.
      Username: test
    Please enter the password to authenticate user test with server
    https://webdav.yandex.ru or hit enter for none.
      Password:

### 1.4 常见问题

用命令sudo /etc/init.d/apache2 reload重启服务器没有反应，用命令sudo /etc/init.d/apache2 reload重新加载Apache2时也报错，一般来讲，很有可能是80端口被占用了，有可能是Nginx。
所以要找到占用端口的服务，并关闭它。或者可以换一个端口。

```
方法如下：
# 找到所有nginx相关进程
$  ps -ef |grep nginx

# 按照显示出的nginx进程号逐一关闭
$ sudo kill -TERM 进程号
# 或
$ pkill -9 nginx

# 重新加载Apache2服务器
$ sudo /etc/init.d/apache2 reload

# 重启Apache2服务器
$ sudo systemctl restart apache2

作者：Solomon_Xie
链接：https://www.jianshu.com/p/17da6608dc74
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
```


## 2 keepass

keePass介绍 ：KeePass 是一款管理密码的开源的免费软件，KeePass 将密码存储为一个数据库，而这个数据库由一个主密码或密码文件锁住，也就是说我们只需要记住一个主密码，或使用一个密码文件，就可以解开这个数据库，就可以获得其他的密码内容。不用担心安全，这个数据库采用当今非常安全的密码算法AES 和 Twofish。

打开软件，file-open-open url：
url直接输入webdav文件夹的keepass数据库文件：`http://ip:port/webdav/keyword.kdbx`，username和password输入访问webdav的用户名和密码，打开即可加载进入密码管理，实现云同步。

它的linux版本是keepassx，直接使用apt安装：
    
    sudo apt-get install keepassx

然后终端输入keepassx即可运行。

keepassx不支持直接打开url，所以需要将服务期上的共享文件夹映射到本地，然后打开。



## 3 使用keepass和坚果云完成共享密码管理

https://www.zhihu.com/question/36932954/answer/1487504294