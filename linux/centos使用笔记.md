环境，centos7，买的百度云的服务器，２核４ｇ内容，40g硬盘

# 安装mysql

<https://www.cnblogs.com/funbin/p/11154784.html>

# 安装mongo

<https://blog.csdn.net/qq_39478237/article/details/90167761>

# mkvirtualenv创建虚拟环境时提示无此命令以及bash: virtualenv: 未找到命令

安装完virtualenv和virtualenvwrapper包后，输入mkvirtualenv提示没有找到该命令，原因是系统没有将命令加入到环境变量（ubuntu和deepin好像是自动加的），需要手动加入，参考：<https://blog.csdn.net/liu_xzhen/article/details/79293373>

<https://www.cnblogs.com/st-st/p/10251449.html>

<https://www.cnblogs.com/hszstudypy/p/11511228.html>

# 安装redis

<https://blog.csdn.net/hjq_ku/article/details/89320930>

使用yum安装完成后，在开启redis服务的时候遇到启动失败的问题

```
# service redis start
Redirecting to /bin/systemctl start redis.service
Job for redis.service failed because the control process exited with error code. See "systemctl status redis.service" and "journalctl -xe" for details.
```

根据提示查看最终找到问题：

```
$ sudo journalctl -xe
-- 
12月 10 21:20:21 instance-ps44hyta systemd[1]: Starting Redis persistent key-value database...
-- Subject: Unit redis.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit redis.service has begun starting up.
12月 10 21:20:21 instance-ps44hyta redis-server[10294]: *** FATAL CONFIG FILE ERROR ***
12月 10 21:20:21 instance-ps44hyta redis-server[10294]: Reading the configuration file, at line 163
12月 10 21:20:21 instance-ps44hyta redis-server[10294]: >>> 'logfile /var/log/redis/redis.log'
12月 10 21:20:21 instance-ps44hyta redis-server[10294]: Can't open the log file: Permission denied
```

最后发现是文件`/var/log/redis/redis.log`没有写入权限，

然后给该文件增加权限，然后启动redis服务

```
chmod 777 /var/log/redis/redis.log
service redis start
```

# centos7中python3.6报错ModuleNotFoundError: No module named '_ssl'

参考：<https://blog.csdn.net/qq_23889009/article/details/100887640>

<https://blog.csdn.net/qq_26870933/article/details/84336109>

# centos7使用virtualenv报错：ModuleNotFoundError: No module named 'zlib'

网上找了很多例子，大部分都是说需要安装依赖库，但是我的情况不同，我安装了依赖库并且重新编译了python结果还是一样，而且创建python2的虚拟环境可以但是python3不行。

最后经过尝试发现是创建虚拟环境的姿势错了，他需要使用原始的python解释器而不是创建的软链接（这与ubuntu和deepin上不同，不知道为什么）。

```
$ mkvirtualenv --python=/usr/bin/python3 test
Running virtualenv with interpreter /usr/bin/python3
Traceback (most recent call last):
  File "/usr/lib/python2.7/site-packages/virtualenv.py", line 39, in <module>
    import zlib
ModuleNotFoundError: No module named 'zlib'


$ mkvirtualenv -p /usr/local/python3/bin/python3 test
Running virtualenv with interpreter /usr/local/python3/bin/python3
Already using interpreter /usr/local/python3/bin/python3
Using base prefix '/usr/local/python3'
New python executable in /home/eason/.virtualenvs/test/bin/python3
Also creating executable in /home/eason/.virtualenvs/test/bin/python
Installing setuptools, pip, wheel...
done.

```



<https://blog.csdn.net/sirria1/article/details/83115582>

<https://www.jianshu.com/p/60c46e982f18>

<https://blog.csdn.net/djshichaoren/article/details/75534240>



# centos7安装kafka

{'url': 'https://alpha-api.fm-dev.thecover.cn/', 'token': 'FM_TOKEN_DATA', 'screate_key': '12345678', 'method': 'POST', 'requests_args': "{'timeout': 10}", 'requests_headers': "{'tenantId':8}"}

```
{
    "spider_name" : "pengpai",
    "type" : 0,
    "time_update" : "2019-12-26 17:36:55",
    "item_id" : "pengpai",
    "url" : "http://www.thepaper.cn/newsDetail_forward_5349045",
    "data" : {
        "channel" : null,
        "sub_channel" : null,
        "url_source" : "http://www.thepaper.cn/channel_25950",
        "url_page" : "http://www.thepaper.cn/newsDetail_forward_5349045",
        "img_list" : null,
        "title" : "微信公众号平台疑似出现故障，内容无法浏览",
        "editor" : "",
        "author" : "",
        "rpfrom" : "澎湃新闻",
        "keywords" : "",
        "tags" : "",
        "brief" : "",
        "content" : "<div class=\"news_txt\" data-size=\"standard\">澎湃新闻（www.thepaper.cn）注意到，12月26日17:20左右，微信平台疑似暂时出现故障，多个微信公众号内容无法正常浏览，点击后出现带有“系统出错”字样的页面。多名网友在网上反映出现同样问题。<br><div class=\"contheight\"></div>截至发稿前，腾讯方面暂未对此置评。<div class=\"hide_word\">(本文来自澎湃新闻，更多原创资讯请下载“澎湃新闻”APP)</div></div>",
        "is_original" : true,
        "score" : null,
        "grade" : null,
        "is_auto_publish" : null,
        "error" : null,
        "duplicate" : null,
        "names" : null,
        "item_id" : null,
        "update" : null,
        "condition" : null,
        "other" : null,
        "pub_time" : "2019-12-26 17:36:00",
        "comment_count" : null,
        "from" : "澎湃新闻"
    }
}
```

