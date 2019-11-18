首先确保手机和电脑在同一个网络！！

1、安装fiddler，并且进行配置：

​    Tools >> options >> connections >> 勾选 allow remote computers to connect

2、查看本机ip地址：    

  在cmd窗口中，输入 ipconfig  ，查看  以太网 ，可以看到

IPv4 地址...............：192.168.0.104

这个192.168.*.***（192.168.0.104） 就是你的本机IP

3、确保手机连接了wifi，并且和电脑是在同一个局域网，

  在手机中，打开浏览器，访问

http://192.168.0.104:8888

IP：是第二步查看到的ip地址，替换成你自己的IP

port：8888是你在fiddler中配置的

注意：有些浏览器会显示打不开，更换其他浏览器就可以了

4、访问成功的话，会显示：



​    Fiddler Echo Service

​    ......

​    ......

​    This page returned a HTTP/200 response

​    .To configure fiddler as a reverse proxy instead of seeing this

​     page, see Reverse Proxy Setup

​    .You can download the FiddlerRoot certificate

5、点击 
FiddlerRoot
certificate ， 下载
证书

6、安装 证书

​    6.1 部分手机可以直接点击 安装

​    6.2 部分手机需要 设置 >> wifi(或WLAN) >> 高级设置 >> 安装证书 >>

​            选中刚刚下载的 证书文件 FiddlerRoot.cer >> 确定

​    6.3 设置(Settings) >> 更多设置 >> 系统安全 >> 从存储设备安装



​    6.4 为证书命名 , 输入自己喜欢的名字，譬如 fiddler  ，确定 ，  显示 证书安装完成



​    6.5 安装完成后，在 设置(Settings) >> 更多设置 >> 系统安全 >> 信任的凭证 >>

​        系统和用户2个tab页 >> 用户 >> 可以查看到 DO_NOT_RUST_FiddlerRoot



PS: 不安装证书，抓取http的数据是没问题的，但是抓取不了https的数据

7、手机设置代理

​    手机设置 >> wifi(或WLAN) >>  选中连接的网络 >> 代理 >> 手动

​    主机名：192.168.0.104    这个是刚刚在 cmd 中查看到的电脑的 IP

​    端口  ：8888

​    不使用网址： 这个不用理会

​    修改完成后，确认



8、打开 fiddler 的抓包，然后在手机端运行要抓包的app，会查看到fiddler中已经可以抓到app的数据了



注意：

1、大部分app都可以直接抓包

2、少部分app没办法直接获取，需要 wireshark、反编译、脱壳 等方式去查找加密算法

3、app抓包一般都是抓取到服务器返回的json数据包