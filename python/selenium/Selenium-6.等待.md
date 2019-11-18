# Selenium笔记（6）等待

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

## 1. 简介

在selenium操作浏览器的过程中，每一次请求url，selenium都会等待页面加载完毕以后，才会将操作权限再次交给我们的程序。

但是，由于ajax和各种JS代码的异步加载问题，所以我们在使用selenium的时候常常会遇到操作的元素还没有加载出来，就会引发报错。为了解决这个问题，`Selenium`提供了几种等待的方法，让我们可以等待元素加载完毕后，再进行操作。

***

## 2. 显式等待

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("http://somedomain/url_that_delays_loading")
try:
    element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "myDynamicElement"))
    )
finally:
    driver.quit()
```

在这个例子中，我们在查找一个元素的时候，不再使用`find_element_by_*`这样的方式来查找元素，而是使用了`WebDriverWait`。

try代码块中的代码的意思是：在抛出元素不存在异常之前，最多等待10秒。在这10秒中，`WebDriverWait`会默认每500ms运行一次until之中的内容，而until中的`EC.presence_of_element_located`则是检查元素是否已经被加载，检查的元素则通过`By.ID`这样的方式来进行查找。

就是说，在10秒内，默认每0.5秒检查一次元素是否存在，存在则将元素赋值给element这个变量。如果超过10秒这个元素仍不存在，则抛出超时异常。

***

### 2.1. Expected Conditions

`Expected Conditions`这个类提供了很多种常见的检查条件可以供我们使用。

- title_is
- title_contains
- presence_of_element_located
- visibility_of_element_located
- visibility_of
- presence_of_all_elements_located
- text_to_be_present_in_element
- text_to_be_present_in_element_value
- frame_to_be_available_and_switch_to_it
- invisibility_of_element_located
- element_to_be_clickable
- staleness_of
- element_to_be_selected
- element_located_to_be_selected
- element_selection_state_to_be
- element_located_selection_state_to_be
- alert_is_present

例子：

```python
from selenium.webdriver.support import expected_conditions as EC

wait = WebDriverWait(driver, 10)
# 等待直到元素可以被点击
element = wait.until(EC.element_to_be_clickable((By.ID, 'someid')))
```

***

## 3. 隐式等待

隐式等待指的是，在`webdriver`中进行`find_element`这一类查找操作时，如果找不到元素，则会默认的轮询等待一段时间。

这个值默认是0，可以通过以下方式进行设置：

```python
from selenium import webdriver

driver = webdriver.Chrome()
driver.implicitly_wait(10) # 单位是秒
driver.get("http://somedomain/url_that_delays_loading")
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```
