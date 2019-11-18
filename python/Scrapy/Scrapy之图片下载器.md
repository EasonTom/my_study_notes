# 图片下载器

`Scrapy`中内置了一个图片下载器的功能（当然也可以自己定义），它依靠` ImagesPipeline`这个类。

## 1 实现功能

- 将下载图片转换成通用的JPG和RGB格式
- 避免重复下载
- 缩略图生成
- 图片大小过滤



## 2 工作流程

- 爬取一个Item，将图片的URLs放入image_urls字段
- 从Spider返回的Item，传递到Item Pipeline
- 当Item传递到ImagePipeline，将调用Scrapy 调度器和下载器完成
- image_urls中的url的调度和下载。
- 图片下载成功结束后，图片下载路径、url和校验和等信息会被填充到images字段中。



## 3 实现方式：

1、自定义pipeline，优势在于可以重写ImagePipeline类中的实现方法；

2、直接使用ImagePipeline类，简单但不够灵活；

具体使用哪种方式，视情况而定。

### 3.1 自定义

item.py文件：定义item字段

```
import scrapy

class PicPhotoPeopleItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    # 存放url的下载地址
    image_urls = scrapy.Field()
    # 图片下载路径、url和校验码等信息（图片全部下载完成后将信息保存在images中）
    images = scrapy.Field()
    # 图片的本地保存地址
    image_paths = scrapy.Field()
```

spider.py文件：编写爬虫文件，解析源码，得到图片的url下载路径

```

# -*- coding: utf-8 -*-
import scrapy
from ..items import PicPhotoPeopleItem
 
 
class PeopleSpider(scrapy.Spider):
    name = 'people'
    allowed_domains = ['699pic.com']
    start_urls = ['http://699pic.com/people.html']
    base_url = 'http://699pic.com/'
 
    def parse(self, response):
        # 获取全部的图片地址
        list_imgs = response.xpath('//div[@class="swipeboxEx"]/div[@class="list"]//img/@data-original').extract()
        if list_imgs:
            item = PicPhotoPeopleItem()
            item['image_urls'] = list_imgs  # 保存到item的image_urls里
            yield item  # 返回item给pipeline文件处理
 
        # 获取下一页url
        next_url = response.xpath('//a[@class="downPage"]/@href').extract()[0] \
            if len(response.xpath('//a[@class="downPage"]/@href')) else None
        if next_url:
            yield scrapy.Request(url=self.base_url + next_url,callback=self.parse)
```

pipelines.py文件：对item里的数据进行处理，这里是调用调度器和下载器下载图片。重写一些ImagesPileline类的方法。

需要重写的方法有：get_media_requests(item, info)和item_completed(results, items, info)

```
from scrapy.pipelines.images import ImagesPipeline
from scrapy.exceptions import DropItem
from scrapy.http import Request
 
 
class PicPhotoPeoplePipeline(ImagesPipeline):
    def file_path(self, request, response=None, info=None):
        """
        重写ImagesPipeline类的file_path方法
        实现：下载下来的图片命名是以校验码来命名的，该方法实现保持原有图片命名
        :return: 图片路径
        """
        image_guid = request.url.split('/')[-1]   # 取原url的图片命名
        return 'full/%s' % (image_guid)
 
    def get_media_requests(self, item, info):
        """
        遍历image_urls里的每一个url，调用调度器和下载器，下载图片
        :return: Request对象
        图片下载完毕后，处理结果会以二元组的方式返回给item_completed()函数
        """
        for image_url in item['image_urls']:
            yield Request(image_url)
 
    def item_completed(self, results, item, info):
        """
        将图片的本地路径赋值给item['image_paths']
        :param results:下载结果，二元组定义如下：(success, image_info_or_failure)。
        第一个元素表示图片是否下载成功；第二个元素是一个字典。
        如果success=true，image_info_or_error词典包含以下键值对。失败则包含一些出错信息。
         字典内包含* url：原始URL * path：本地存储路径 * checksum：校验码
        :param item:
        :param info:
        :return:
        """
        image_paths = [x['path'] for ok, x in results if ok]
        if not image_paths:
            raise DropItem("Item contains no images")   # 如果没有路径则抛出异常
        item['image_paths'] = image_paths
        return item

```

settings.py文件：配置setting文件。

```
import os
# 配置数据保存路径，为当前工程目录下的 images 目录中
project_dir = os.path.abspath(os.path.dirname(__file__))
IMAGES_STORE = os.path.join(project_dir, 'images')
# 过期天数
IMAGES_EXPIRES = 90  # 90天内抓取的都不会被重抓
 
# IMAGES_MIN_HEIGHT = 100  # 图片的最小高度
# IMAGES_MIN_WIDTH = 100   # 图片的最小宽度

```

最后激活pipeline。

### 3.2 直接使用

item.py和spider.py同上；pipeline.py此处不需要自己编写，直接引用现成的ImagePileline；

修改settings.py文件即可，代码如下：

```

import os
IMAGES_URLS_FIELD = 'image_urls' # 要保存的字段，即在 Item 类中的字段名为 image_urls
 
# 配置数据保存路径，为当前工程目录下的 images 目录中
project_dir = os.path.abspath(os.path.dirname(__file__))
IMAGES_STORE = os.path.join(project_dir, 'images')
# 过期天数
IMAGES_EXPIRES = 90  # 90天内抓取的都不会被重抓
 
# IMAGES_MIN_HEIGHT = 100  # 图片的最小高度
# IMAGES_MIN_WIDTH = 100   # 图片的最小宽度
 
ITEM_PIPELINES = {
   'scrapy.pipelines.images.ImagesPipeline': 1,
}

```




    # 
   

