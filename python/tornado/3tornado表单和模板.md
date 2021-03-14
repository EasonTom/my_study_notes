hello.py 代码



debug：可以重启服务并显示异常问题
template_path：配置模板路径变量
static_path：配置静态文件路径变量

关键字参数和路径参数

1路径参数和关键字参数

```python
class PAttrHandler(tornado.web.RequestHandler):
    def get(self, age,name):
        self.write(age)
        self.write(name)


(r"/p/(\d*)/(.*)", PAttrHandler),

class KeyHandler(tornado.web.RequestHandler):
    def get(self):
        p = self.get_argument('p')
        n = self.get_argument('n')
        print(p)
        print(n)
        
  (r"/key",KeyHandler ),
(r"/test",TestHandler ),


key.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a href="/key?p=123&n=画龙">key</a>
</body>
</html>





重定向 redirect()
class RedHandler(tornado.web.RequestHandler):
    def get(self):
        self.redirect('/')
        
    
 (r"/tem",RedHandler ),



```

```python
import os.path

import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options

define("port", default=8000, help="run on the given port", type=int)


class IndexHandler(tornado.web.RequestHandler):
    def get(self):
        self.render("templates/index1.html")


class UserHandler(tornado.web.RequestHandler):
    def post(self):
        user_name = self.get_argument("username")
        user_email = self.get_argument("email")
        user_website = self.get_argument("website")
        user_language = self.get_argument("language")
        self.render("templates/user.html", username=user_name, email=user_email, website=user_website, language=user_language)


handlers = [
    (r"/", IndexHandler),
    (r"/user", UserHandler)
]

template_path = os.path.join(os.path.dirname(__file__), "templates")
print(template_path)

if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = tornado.web.Application(handlers, template_path)
    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()
```

index1.html代码

```html
<!DOCTYPE html>
<html>
    <head>
        <title>sign in your name</title>
    </head>
    <body>
        <h2>Please sing in.</h2>
        <form method="post" action="/user">
            <p>Name:<br><input type="text" name="username"></p>
            <p>Email:<br><input type="text" name="email"></p>
            <p>Website:<br><input type="text" name="website"></p>
            <p>Language:<br><input type="text" name="language"></p>
            <input type="submit" value="ok,submit my information">
        </form>
    </body>
```

```html 
<!DOCTYPE html>
<html>
    <head>
        <title>sign in your name</title>
    </head>
    <body>
        <h2>Your Information</h2>
        <p>Your name is {{username}}</p>
        <p>Your email is {{email}}</p>
        <p>Your website is {{website}}, it is very good. This website is make by {{language}}</p>
        #这个网站是用语言制作的
    </body>
</html>
```

