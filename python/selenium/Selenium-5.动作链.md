# Selenium笔记（5）动作链

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

## 1. 简介

一般来说我们与页面的交互可以使用`Webelement`的方法来进行点击等操作。但是，有时候我们需要一些更复杂的动作，类似于拖动，双击，长按等等。

这时候就需要用到我们的`Action Chains`（动作链）了。

***

## 2. 例子

```python
from selenium.webdriver import ActionChains

element = driver.find_element_by_name("source")
target = driver.find_element_by_name("target")

actions = ActionChains(driver)
actions.drag_and_drop(element, target)
actions.perform()
```

在导入动作链模块以后，需要声明一个动作链对象，在声明时将webdriver当作参数传入，并将对象赋值给一个actions变量。

然后我们通过这个actions变量，调用其内部附带的各种动作方法进行操作。

**注：在调用各种动作方法后，这些方法并不会马上执行，而是会按你代码的顺序存储在ActionChains对象的队列中。当你调用perform()时，这些动作才会依次开始执行。**

***

## 3. 常用动作方法

- `click`(*on_element=None*)

  左键单击传入的元素，如果不传入的话，点击鼠标当前位置。

- `context_click`(*on_element=None*)

  右键单击。

- `double_click`(*on_element=None*)

  双击。

- `click_and_hold`(*on_element=None*)

  点击并抓起

- `drag_and_drop`(*source*, *target*)

  在source元素上点击抓起，移动到target元素上松开放下。

- `drag_and_drop_by_offset`(*source*, *xoffset*, *yoffset*)

  在source元素上点击抓起，移动到相对于source元素偏移xoffset和yoffset的坐标位置放下。

- `send_keys`(**keys_to_send*)

  将键发送到当前聚焦的元素。

- `send_keys_to_element`(*element*, **keys_to_send*)

  将键发送到指定的元素。

- `reset_actions`()

  清除已经存储的动作。
