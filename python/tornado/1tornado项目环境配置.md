

1框架配置和基本模板



hello.py

```python
import tornado.ioloop
import tornado.web


class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")


application = tornado.web.Application([
    (r"/", MainHandler),
])

if __name__ == "__main__":
    application.listen(8080)
    tornado.ioloop.IOLoop.current().start()
```



1创建app.py运行文件

```python
import tornado.ioloop
import tornado.web
import tornado.options
from tornado.options import define,options
from handlers import main
define('port', default='8000',help='Listening port',type=int)


class Application(tornado.web.Application):

    def __init__(self):
        handlers = [
            ('/', main.IndexHandler),
            ('/explore', main.ExploreHandler),
            ('/post/(?P<post_id>[0-9]+)', main.PostHandler),

        ]
        settings = dict(
            debug=True,
            template_path='templates',
            static_path='static'
        )
        super().__init__(handlers, **settings)


application = Application()

if __name__ == "__main__":
    tornado.options.parse_command_line()
    application.listen(options.port)
    print('Server start on port {}'.format(options.port))
    tornado.ioloop.IOLoop.current().start()
```



2创建handlers包文件并在里面创建main.py 文件用与编写逻辑

```Python
import tornado.web
class IndexHandler(tornado.web.RequestHandler):
    def get(self,*args,**kwargs):
        self.render('index.html')


class ExploreHandler(tornado.web.RequestHandler):
    def get(self, *args, **kwargs):
        self.render('explore.html')


class PostHandler(tornado.web.RequestHandler):
    def get(self, post_id):
        self.render('post.html',post_id=post_id)
```

3创建模板文件并在模板文件中创建模板文件

3.1创建base.html基本模板用于继承

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Tornado Title{% end %}</title>
</head>
<body>
{% block content %}Default body of base{% end %}
</body>
</html>
```

3.2创建index.html做首页

```html
{% extends 'base.html' %}
{% block title %}index page{% end %}
{% block content %}
I am index
{% end %}
```

3.3创建explore.html做发现页

```html
{% extends 'base.html' %}
{% block title %}explore page{% end %}
{% block content %}
I am explore
{% end %}
```

3.4创建post.html做详情页

```html
{% extends 'base.html' %}
{% block title %}post page{% end %}
{% block content %}
I am post｛｛ post_id ｝｝

{% end %}
```

4创建static文件用于存放js和css

