# Selenium笔记（4）Webelement

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

这是通过find方法找到的页面元素对象，此对象提供了多种方法，让我们可以与页面元素进行交互，例如点击、清空。

***

## 1. 方法

1. `clear()`清空

   如果当前元素中有文本，则清空文本

2. `click()`单击

   点击当前元素

3. `get_attribute(name)`获取属性

   获取元素的attribute/property

   优先返回完全匹配属性名的值，如果不存在，则返回属性名中包含name的值。

4. `screenshot(filename)` 获取截图

   获取当前元素的截图，保存为png，最好用绝对路径。

   此方法在Chrome中无法使用。

5. `send_keys(value)` 模拟键入元素

   给当前元素模拟输入

6. `submit()`提交表单

   提交表单

在页面元素中，同样提供`find_elements_by_*`等查找方法，可以将查找范围限制到当前元素。

***

## 2. 属性

1. `text`

   获取当前元素的文本内容

2. `tag_name`

   获取当前元素的标签名

3. `size`

   获取当前元素的大小

4. `screenshot_as_png`

   将当前元素截屏并保存为png格式的二进制数据

5. `screenshot_as_base64`

   将当前元素截屏并保存为base64编码的字符串

6. `rect`

   获取一个包含当前元素大小和位置的字典

7. `parent`

   获取当前元素的父节点

8. `location`

   当前元素的位置


***

## 3. Keys

我们经常需要模拟键盘的输入，当输入普通的值时，在`send_keys()`方法中传入要输入的字符串就好了。

但是我们有时候会用到一些特殊的按键，这时候就需要用到我们的Keys。

### 3.1. 例子

```python
from selenium.webdriver.common.keys import Keys

elem.send_keys(Keys.RETURN)
```

### 3.2. 组合按键

而有时候我们又会需要使用到多个按键的组合按键，比如ctrl+a，那么在参数中依次传入即可。

```python
from selenium.webdriver.common.keys import Keys

elem.send_keys(Keys.CONTROL, 'a')
```

### 3.3. Keys属性

这个`Keys`类有很多属性，每个属性对应一个按键。所有的属性如下所示：

```python
ADD = u'\ue025'
ALT = u'\ue00a'
ARROW_DOWN = u'\ue015'
ARROW_LEFT = u'\ue012'
ARROW_RIGHT = u'\ue014'
ARROW_UP = u'\ue013'
BACKSPACE = u'\ue003'
BACK_SPACE = u'\ue003'
CANCEL = u'\ue001'
CLEAR = u'\ue005'
COMMAND = u'\ue03d'
CONTROL = u'\ue009'
DECIMAL = u'\ue028'
DELETE = u'\ue017'
DIVIDE = u'\ue029'
DOWN = u'\ue015'
END = u'\ue010'
ENTER = u'\ue007'
EQUALS = u'\ue019'
ESCAPE = u'\ue00c'
F1 = u'\ue031'
F10 = u'\ue03a'
F11 = u'\ue03b'
F12 = u'\ue03c'
F2 = u'\ue032'
F3 = u'\ue033'
F4 = u'\ue034'
F5 = u'\ue035'
F6 = u'\ue036'
F7 = u'\ue037'
F8 = u'\ue038'
F9 = u'\ue039'
HELP = u'\ue002'
HOME = u'\ue011'
INSERT = u'\ue016'
LEFT = u'\ue012'
LEFT_ALT = u'\ue00a'
LEFT_CONTROL = u'\ue009'
LEFT_SHIFT = u'\ue008'
META = u'\ue03d'
MULTIPLY = u'\ue024'
NULL = u'\ue000'
NUMPAD0 = u'\ue01a'
NUMPAD1 = u'\ue01b'
NUMPAD2 = u'\ue01c'
NUMPAD3 = u'\ue01d'
NUMPAD4 = u'\ue01e'
NUMPAD5 = u'\ue01f'
NUMPAD6 = u'\ue020'
NUMPAD7 = u'\ue021'
NUMPAD8 = u'\ue022'
NUMPAD9 = u'\ue023'
PAGE_DOWN = u'\ue00f'
PAGE_UP = u'\ue00e'
PAUSE = u'\ue00b'
RETURN = u'\ue006'
RIGHT = u'\ue014'
SEMICOLON = u'\ue018'
SEPARATOR = u'\ue026'
SHIFT = u'\ue008'
SPACE = u'\ue00d'
SUBTRACT = u'\ue027'
TAB = u'\ue004'
UP = u'\ue013'
```
