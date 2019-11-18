# pyspider使用手册

## 1 pyspider的安装

首先更新一下源：

```shell
apt-get update
```

然后使用`pip`安装：

```shell
pip install pyspider
```

如果出现`pycurl`有问题，输入以下命令：

```shell
sudo apt-get install libssl-dev libcurl4-openssl-dev python-dev
```

如果出现网络问题，安装的时候更换以下源

```
pip install pyspider -i http://pypi.douban.com/simple --trusted-host <pypi.douban.com>
```

如果出现某个依赖库使用pip安装不了，进入以下网址下载对应版本的依赖库进行安装

<https://www.lfd.uci.edu/~gohlke/pythonlibs/#genshi>

---

## 2 运行

在终端或命令行中输入`pyspider`运行框架，成功后会提示：

```
webui running on 0.0.0.0:5000
```

也就是在本地的`5000`端口开启了服务。

`pyspider`框架会给用户提供一个web页面，开启服务后使用浏览器直接连接服务器的`5000`端口进入`pyspider`的界面。

**如果是本地服务直接输入网址<http://127.0.0.1:5000>，如果是外网服务器先确认服务器开了`5000`端口，然后再通过服务器的ip地址访问。**

## 3 创建项目

点击界面右上方`create`,输入项目名称和第一次请求的`URL`地址，然后创建。

---

## 4 代码结构

进入新项目会出现下列代码：

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-
# Created on 2019-01-12 00:07:55
# Project: baidu

from pyspider.libs.base_handler import *


class Handler(BaseHandler):
    crawl_config = {
    }

    @every(minutes=24 * 60)
    def on_start(self):
        self.crawl('https://www.baidu.com', callback=self.index_page)

    @config(age=10 * 24 * 60 * 60)
    def index_page(self, response):
        for each in response.doc('a[href^="http"]').items():
            self.crawl(each.attr.href, callback=self.detail_page)

    @config(priority=2)
    def detail_page(self, response):
        return {
            "url": response.url,
            "title": response.doc('title').text(),
        }
```

代码运行流程：

1. 首先运行`on_start`方法，所以该方法名称不能修改。
2. `on_start`方法调用了`self.crawl`方法
3. `self.crawl`方法生成一个爬取任务，将其加入待爬取队列，该方法传入`url`用来生成爬取任务，`callback`参数传入回调函数，当爬取任务完成后用来执行。使用参数`validate_cert=False`关闭`SSL`证书验证。
4. `self.crawl`完成爬取任务后执行回调函数。`self.index_page`函数传入获取到的响应，首先解析网页数据，可以自定义解析方法。然后继续访问其它网页，回调`self.detail_page`。
5. `self.detail_page`返回需要的数据，可以自定义。
6. 代码遇到return时，pyspider底层会自动调用on_result方法，该方法传入的result参数就是return返回的值。on_result方法是底层自己调用的，用以返回数据的后续处理，可以重写。

其它代码的作用：

1. `crawl_config`  存放请求头和代理等。
2. 装饰器`@every(minutes=24 * 60)`    该方法执行间隔时间，每天一次。
3. `@config(age=10 * 24 * 60 * 60)`  定义requests请求过期时间，10天之内遇到同样的请求会被忽略。

## 5 pyspider控制台

1. 第一列是group分组

   删除项目：

   将项目的`group`改成`delete`，将`status`换成`stop`，24小时后项目就会被删除。

2. 第二列为项目名

3. 第三列为状态

   | 状态     | 作用             |
   | -------- | ---------------- |
   | TODO     | 新建项目默认     |
   | STOP     | 停止项目         |
   | CHECKING | 在运行时修改代码 |
   | DEBUG    | 调试             |
   | RUNNING  | 运行             |


## 6 文件目录

数据会存放在用户目录下的data文件夹下，该文件夹下存放着以下几个文件

| 名称          | 作用     |
| ------------- | -------- |
| project.db    | 项目     |
| result.db     | 结果信息 |
| scheduler.1d  |          |
| scheduler.1h  |          |
| scheduler.all |          |
| task.db       | 任务队列 |

可以直接删除这些数据来控制项目。

## 7 数据存储

新建一个方法存储数据，将代码改写为如下形式：

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-
# Created on 2019-01-12 00:07:55
# Project: baidu

import pymysql
import pymongo
from pyspider.libs.base_handler import *


class Handler(BaseHandler):
    crawl_config = {
    }
	
	def __init__(self):
		client = pymongo.MongoClient(host='127.0.0.1', port='27017')
		db = client['baidu']
		self.items = db['items']
	
    @every(minutes=24 * 60)
    def on_start(self):
        self.crawl('https://www.baidu.com', callback=self.index_page)

    @config(age=10 * 24 * 60 * 60)
    def index_page(self, response):
        for each in response.doc('a[href^="http"]').items():
            self.crawl(each.attr.href, callback=self.detail_page)

    @config(priority=2)
    def detail_page(self, response):
        return {
            "url": response.url,
            "title": response.doc('title').text(),
        }
        
    def save_mongo(self, result):
    	slef.items.insert(dict(result))
```

## 8 phantomjs

pyspider使用的是requests请求，不能解析js代码，此时需要用到phantomjs。

### 8.1 安装

phantomjs是一款无界面浏览器，需要下载安装。

windows安装：

去[官网](http://phantomjs.org/download.html)下载windows版本，解压后，将bin文件夹下的phantomjs.exe放入python解释器根目录下，这样可以避免配置环境变量。

linux安装：

```
sudo apt-get install phantomjs
```

## 8.2 启动

首先重启pyspider，然后在抓取函数中传入参数`fetch_type='js'`用来启动phantomjs

```python
self.crawl('https://www.baidu.com', callback=self.index_page, fetch_type='js')
```

## 9 问题

### 9.1 [Ubuntu16.04部署phantomjs的一个问题](https://www.cnblogs.com/dhcn/p/7743714.html)



​      首先phantomjs是作为pyspider的一个外部依赖组件部署的。

​      apt安装完出现问题：

```
QXcbConnection: Could not connect to display 
PhantomJS has crashed. Please read the bug reporting guide at
<http://phantomjs.org/bug-reporting.html> and file a bug report.
Aborted
```

​      在/usr/bin/phantomjs的合适位置加上以下设置代码：

```
export QT_QPA_PLATFORM=offscreen
export QT_QPA_FONTDIR=/usr/share/fonts
```

​     phantomjs即可正常运行。

