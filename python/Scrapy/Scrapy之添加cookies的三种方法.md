1.settings
settings文件中给Cookies_enabled=False解注释
settings的headers配置的cookie就可以用了
这种方法最简单，同时cookie可以直接粘贴浏览器的。
后两种方法添加的cookie是字典格式的，需要用json反序列化一下,
而且需要设置settings中的Cookies_enabled=True

2.DownloadMiddleware
settings中给downloadmiddleware解注释
去中间件文件中找downloadmiddleware这个类，修改process_request，添加`request.cookies={}`即可，或者直接添加到meta参数中

```
request.meta{'cookiejar'}=response.meta['cookiejar']
```

3.爬虫主文件中重写start_request

```python
def start_requests(self):
	yield scrapy.Request(url,dont_filter=True,cookies={自己的cookie})
```

同样，headers和代理也可以上述方法设置。

