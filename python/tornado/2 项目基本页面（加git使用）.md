# 项目简介

## 为什么做一个模仿 Instagram 的应用

- 偏后端和后台的开发
- 充分利用 tornado 的特点
- 积累项目经验，巩固知识点

## Instagram 主要组成

- 发现或最近上传的图片页面
- 所关注的用户图片流
- 单个图片详情页面
- 数据库 Database
- 用户档案 User Profile



# 怎么做

- 从最简单开始，迭代增加功能
- 用户，登陆，关注等
- 数据库保存
- UI 和 Web 界面美化
- 外部连接
- 部署和运行



# 开始功能

## 运行基本程序

- app.py
- handlers 配置
- templates 和 static 目录设置

## 三个基本页面

- 发现或最近上传的图片页面 /explore  ExploreHandler

- 所关注的用户图片流  /   IndexHandler

- 单个图片详情页面  /post/id   PostHandler


## Tornado 文档

http://www.tornadoweb.org/en/stable/  官方英文

https://tornado-zh.readthedocs.io/zh/latest/index.html  中文 4.3

1将文件中的imgs文件夹复制到static下 然后在每个HTML页面中插入图片进行显示



```HTML
<!--<img src="/static/imgs/1.jpg" />-->


<!--<img src="/static/imgs/1.jpg"width="200px" />-->

<!--<img src="/static/imgs/{{ post_id }}.jpg" width="560px" />-->


```



static_url_prefix 的使用和目的  

如果老板需要效果静态文件夹的名称就可以使用他

比如：

```Python
ettings = dict(
    debug=True,
    template_path='templates',
    static_path='static',
    static_url_prefix='/tupian/'   # 一定要加斜杠
)
这样访问的时候就实现从tupian这个路径开始访问图片
前端代码修改
<!--<img src="/static/imgs/1.jpg"width="200px" />-->
<!--<img src="{{ static_url('imgs/1.jpg') }}" width="200px" />-->

使用for循环输出图片
<!--{% for i in range(1,5) %}-->
<!--<img src="{{ static_url('imgs/{}.jpg'.format(i)) }}"/><br>-->
<!--{% end %}-->

使用后端循环输出：
在main文件中
    def get(self,*args,**kwargs):
        imgs = []
         for i in range(1,5):
             imgs.append('imgs/{}.jpg'.format(i))
        self.render('index.html',imgs=imgs) 
        
 前端修改：
<!--{% for img_url in imgs %}-->
<!--<img src="{{ static_url(img_url) }}"/><br>-->
<!--{% end %}-->

使用，对图片进行封装封装，创建utils包，在包项目创建photo文件


def get_images():
    imgs_paths = []
    for i in range(1,5):
        imgs_paths.append('imgs/{}.jpg'.format(i))
    return imgs_paths
在main.py中
imgs = photo.get_images()
为了减少前端循环可以，
class PostHandler(tornado.web.RequestHandler):
    def get(self, post_id):
        # imgs = photo.get_images()
        img_path = "imgs/{}.jpg".format(post_id)
        # self.render('post.html',post_id=post_id)
        self.render('post.html',img_path=img_path)
        
<img src="{{ static_url(img_path) }}"  width="560px" />

```



# Git 使用

使用参考文档

- [git 使用简易指南](http://www.bootcss.com/p/git-guide/) 
- [在Pycharm中使用GitHub - 刘江liujiangblog.com - 博客园](https://www.cnblogs.com/feixuelove1009/p/5955332.html) 
- [Git - 起步](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5)   官方文档，详细。
- 忽略文件 .gitignore



初次运行配置用户信息（在 cmd 里运行）

`git config --global user.name "John Doe"`

`git config --global user.email johndoe@example.com` 

Linux 安装 

`sudo apt-get install git`



下载地址

- git  https://git-scm.com/download/win 
-  GitHub Desktop https://central.github.com/deployments/desktop/desktop/latest/win32  







# 作业

## 基本的三个页面

## 额外：Git 使用 

# 