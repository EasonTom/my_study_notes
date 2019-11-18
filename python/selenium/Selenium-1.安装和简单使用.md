# Selenium笔记（1）安装和简单使用

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

***

## 1. 简介

Selenium是一个用于Web应用程序测试的工具。

Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。支持的浏览器包括IE（7, 8, 9, 10, 11），Firefox，Safari，Chrome，Opera等。

这个工具的主要功能包括：测试与浏览器的兼容性——测试你的应用程序看是否能够很好得工作在不同浏览器和操作系统之上。测试系统功能——创建回归测试检验软件功能和用户需求。

而用在爬虫上则是模拟正常用户访问网页并获取数据。

***

## 2. 安装

### 2.1. ChromeDriver

使用selenium驱动chrome浏览器需要下载chromedriver，而且chromedriver版本需要与chrome的版本对应，版本错误的话则会运行报错。

Chromedriver下载地址：[http://npm.taobao.org/mirrors/chromedriver/](http://npm.taobao.org/mirrors/chromedriver/)

目前最新的Chromedriver版本开始与Chrome同名，所以尽量选择版本相近的即可。

Chromedriver与Chrome版本映射表：

| chromedriver版本 | 支持的Chrome版本 |
| ---------------- | ---------------- |
| v2.39            | v66-68           |
| v2.38            | v65-67           |
| v2.37            | v64-66           |
| v2.36            | v63-65           |
| v2.35            | v62-64           |
| v2.34            | v61-63           |
| v2.33            | v60-62           |

#### 2.1.1. Mac/Linux

下载完成解压后，将文件移动至`/usr/local/bin`目录中，则可以正常使用。

#### 2.1.2. Windows

下载完成解压后，将文件移动到一个配置了环境变量的文件夹中，例如你的Python安装文件夹。

### 2.2. Selenium安装

`Selenium`的安装非常简单，直接pip就可以搞定。

```python
pip install selenium
```

***

## 3. 简单使用

### 3.1. Chrome无界面运行

这是chrome浏览器2017年发布的新特性，需要unix版本的chrome版本高于57，windows版本的chrome版本高于58。

使用selenium无界面运行chrome的代码如下:

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

# 实例化一个启动参数对象
chrome_options = Options()
# 设置浏览器以无界面方式运行
chrome_options.add_argument('--headless')
# 官方文档表示这一句在之后的版本会消失，但目前版本需要加上此参数
chrome_options.add_argument('--disable-gpu')
# 设置浏览器参数时最好固定好窗口大小，窗口大小不同会在解析网页时出现不同的结果
chrome_options.add_argument('--window-size=1366,768')
# 启动浏览器
browser = webdriver.Chrome(chrome_options=chrome_options)
```

运行上述代码，则会打开一个无界面chrome浏览器的空白页，去掉headless那一句可以看到效果。

### 3.2. Selenium简单例子

这是一个打开百度首页，在输入框中输入Python，并点击搜索的例子。

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

# 打开一个Chrome浏览器
browser = webdriver.Chrome()
# 请求百度首页
browser.get('https://www.baidu.com')
# 找到输入框位置
input = WebDriverWait(browser, 10).until(
                EC.presence_of_element_located((By.XPATH, '//*[@id="kw"]'))
            )
# 在输入框中输入Python
input.send_keys('Python')
# 找到输入按钮
button = WebDriverWait(browser, 10).until(
                EC.element_to_be_clickable(
                    (By.XPATH, '//*[@id="su"]'))
            )
# 点击一次输入按钮
button.click()
browser.quit()
```
