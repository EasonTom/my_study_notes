# Selenium笔记（2）Chrome启动选项

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

在`Selenium`中使用不同的`Webdriver`可能会有不一样的方法，有些相同的操作会得到不一样的结果，本文主要介绍的是`Chrome()`的使用方法。

其他`Webdriver`可以查阅官方文档。

***

## 1. Chrome Options

这是一个Chrome的参数对象，在此对象中使用`add_argument()`方法可以添加启动参数，添加完毕后可以在初始化Webdriver对象时将此Options对象传入，则可以实现以特定参数启动Chrome。

### 1.1. 例子

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

# 实例化一个启动参数对象
chrome_options = Options()
# 添加启动参数
chrome_options.add_argument('--window-size=1366,768')
# 将参数对象传入Chrome，则启动了一个设置了窗口大小的Chrome
browser = webdriver.Chrome(chrome_options=chrome_options)
```

***

### 1.2. 常用的启动参数

|        启动参数        |                 作用                 |
| :--------------------: | :----------------------------------: |
|    --user-agent=""     |        设置请求头的User-Agent        |
| --window-size=1366,768 |           设置浏览器分辨率           |
|       --headless       |              无界面运行              |
|   --start-maximized    |              最大化运行              |
|      --incognito       |               隐身模式               |
|  --disable-javascript  |            禁用javascript            |
|   --disable-infobars   | 禁用浏览器正在被自动化程序控制的提示 |

完整启动参数可以到此页面查看：

[https://peter.sh/experiments/chromium-command-line-switches/](https://peter.sh/experiments/chromium-command-line-switches/)

#### 1.2.1. 禁用图片加载

Chrome的禁用图片加载参数设置比较复杂，如下所示：

```python
prefs = {
    'profile.default_content_setting_values' : {
        'images' : 2
    }
}
options.add_experimental_option('prefs',prefs)
```

#### 1.2.2. 禁用浏览器弹窗

使用浏览器时常常会有弹窗弹出，以下选项可以禁止弹窗：

```python
prefs = {  
    'profile.default_content_setting_values' :  {  
        'notifications' : 2  
     }  
}  
options.add_experimental_option('prefs',prefs)
```

***

## 2. Chrome WebDriver对象

这个对象继承自`selenium.webdriver.remote.webdriver.WebDriver`，这个类会在下一章讲到，Chrome的WebDriver作为子类增添了几个方法。

### 2.1. 指定chromedriver.exe的位置

chromedriver.exe一般可以放在环境文件中，但是有时候为了方便部署项目，或者为了容易打包，我们可以将chromedriver.exe放到我们的项目目录中，然后在初始化Chrome Webdriver对象时，传入chromedriver.exe的路径。

如下所示：

```python
from selenium import webdriver
browser = webdriver.Chrome(executable_path='chromedriver.exe')
```
