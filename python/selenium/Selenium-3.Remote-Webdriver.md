# Selenium笔记（3）Remote Webdriver

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

## 1. 简介

`selenium.webdriver.remote.webdriver.WebDriver` 这个类其实是所有其他Webdriver的父类，例如`Chrome Webdriver`，`Firefox Webdriver`都是继承自这个类。这个类中实现了每个Webdriver间相通的方法。

***

## 2. 常用方法与属性

- `get(url)`

  在当前浏览器会话中访问传入的url地址。

  用法：

  ```python
  driver.get('https://www.baidu.com')
  ```

- `close()`

  关闭浏览器当前窗口。

- `quit()`

  退出webdriver并关闭所有窗口。

- `refresh()`

  刷新当前页面。

- `title`

  获取当前页的标题。

- `page_source`

  获取当前页渲染后的源代码。

- `current_url`

  获取当前页面的url。

- `window_handles`

  获取当前会话中所有窗口的句柄，返回的是一个列表。

***

## 3. 查找元素

`Webdriver`对象中内置了查找节点元素的方法，使用非常方便。

### 3.1. 单个查找

以下是查找单个元素的方法：

| 方法                                  | 作用                         |
| ------------------------------------- | ---------------------------- |
| `find_element_by_xpath`()             | 通过`Xpath`查找              |
| `find_element_by_class_name`()        | 通过`class属性`查找          |
| `find_element_by_css_selector`()      | 通过`css选择器`查找          |
| `find_element_by_id`()                | 通过`id`查找                 |
| `find_element_by_link_text`()         | 通过`链接文本`查找           |
| `find_element_by_name`()              | 通过`name属性`进行查找       |
| `find_element_by_partial_link_text`() | 通过`链接文本的部分匹配`查找 |
| `find_element_by_tag_name`()          | 通过`标签名`查找             |

查找后返回的是一个`Webelement`对象。

### 3.2. 多个查找

上面的方法都是将第一个找到的元素进行返回，而将所有匹配的元素进行返回使用的是`find_elements_by_*`方法。

此方法返回的是一个`Webelement`对象组成的列表。**注：将其中的element加上一个s，则是对应的多个查找方法。**

### 3.3. 私有方法

除了以上的多种查找方式，还有两种私有方法`find_element()`和`find_elements()`可以使用：

例子：

```python
from selenium.webdriver.common.by import By

driver.find_element(By.XPATH, '//button[text()="Some text"]')
driver.find_elements(By.XPATH, '//button')
```

`By`这个类是专门用来查找元素时传入的参数，这个类中有以下属性：

```python
ID = "id"
XPATH = "xpath"
LINK_TEXT = "link text"
PARTIAL_LINK_TEXT = "partial link text"
NAME = "name"
TAG_NAME = "tag name"
CLASS_NAME = "class name"
CSS_SELECTOR = "css selector"
```

***

## 4. 操作Cookie

- `add_cookie(cookie_dict)`

  给当前会话添加一个cookie。

  - cookie_dict: 一个字典对象，必须要有"name"和"value"两个键，可选的键有：“path”, “domain”, “secure”, “expiry” 。

  - 用法：

    ```python
    driver.add_cookie({‘name’ : ‘foo’, ‘value’ : ‘bar’})
    driver.add_cookie({‘name’ : ‘foo’, ‘value’ : ‘bar’, ‘path’ : ‘/’})
    driver.add_cookie({‘name’ : ‘foo’, ‘value’ : ‘bar’, ‘path’ : ‘/’, ‘secure’:True})
    ```

- `get_cookie(name)`

  按name获取单个Cookie，没有则返回None。

- `get_cookies()`

  获取所有Cookie，返回的是一组字典。

- `delete_all_cookies`()[¶](http://selenium-python.readthedocs.io/api.html#selenium.webdriver.remote.webdriver.WebDriver.delete_all_cookies)

  删除所有Cookies。

- `delete_cookie`(*name*)

  按name删除指定cookie。

***

## 5. 获取截屏

- `get_screenshot_as_base64()`

  获取当前窗口的截图保存为一个base64编码的字符串。

- `get_screenshot_as_file(filename)`

  获取当前窗口的截图保存为一个png格式的图片，filename参数为图片的保存地址，最后应该以.png结尾。如果出现IO错误，则返回False。

  用法：

  ```python
  driver.get_screenshot_as_file(‘/Screenshots/foo.png’)
  ```

- `get_screenshot_as_png()`

  获取当前窗口的截图保存为一个png格式的二进制字符串。

***

## 6. 获取窗口信息

- `get_window_position`(*windowHandle='current'*)

  获取当前窗口的x,y坐标。

- `get_window_rect()`

  获取当前窗口的x,y坐标和当前窗口的高度和宽度。

  ```python
  In [1]: driver.get_window_rect()
  Out[1]: {'height': 600, 'width': 800, 'x': 0, 'y': 200}
  ```

- `get_window_size`(*windowHandle='current'*)

  获取当前窗口的高度和宽度。

***

## 7. 切换框架或窗口

- `switch_to.frame`(*frame_reference*)

  在页面中，如果有iframe这样的页面子框架的话，selenium是无法搜索到子框架`frame`中的元素，并与之定位的。

  所以如果要操作`frame`中的元素，则首先要切换到这个`frame`中。

  首先我们需要使用上面提供的搜索方法`find_element_by_*`等来找到frame框架，然后传入到切换的方法中。

  ```python
  frame = driver.find_element_by_tag_name("iframe")
  driver.switch_to.frame(frame)
  ```

  还有一个方法可以切换回主界面：

  ```python
  driver.switch_to.default_content()
  ```

- `switch_to.window`(*window_name*)

  这个方法可以让我们在一个浏览器中的窗口中互相切换，这个方法中需要传入目标窗口的句柄，窗口句柄可以通过`driver.window_handles`等方法来进行获取。

  ```python
  windows = driver.window_handles
  # 切换到最新打开的窗口中
  switch_to.window(windows[-1])
  ```

***

## 8. 执行JS代码

- `execute_async_script(script, *args)`

  在当前的window/frame中`异步`执行JS代码。

  script：要执行的JS代码。

  *args：JS代码执行要传入的参数。

- `execute_script(script, *args)`

  在当前的window/frame中`同步`执行JS代码。

  script：要执行的JS代码。

  *args：JS代码执行要传入的参数。

  ```python
  driver.execute_script("window.scrollTo(0, document.body.scrollHeight)")
  ```
