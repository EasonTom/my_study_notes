# Selenium笔记（7）常见的坑

本文集链接：[https://www.jianshu.com/nb/25338984](https://www.jianshu.com/nb/25338984)

## 1. 用Xpath查找数据时无法直接获取节点属性

通常在我们使用xpath时，可以使用`@class`的方式直接获取节点的属性，如下所示：

```python
page.xpath('//div/a/@class')
```

但在`Selenium`中不支持这种用法，只能在找到节点后，使用`get_attribute(name)`方法来获取属性：

```python
page.xpath('//div/a').get_attribute('class')
```

同样的，`Selenium`同样不支持Xpath中的`string()`，`text()`这类的方法，只能获取元素节点。

***

## 2. 使用了WebDriverWait以后仍然无法找到元素

有很多时候，一个简单的元素，明明也加了显式等待，但就是找不到，代码在仔细查看过后也没有问题后，多半是以下这几种情况：

1. 由于分辨率设置的原因，查找的元素当前是不可见的。
2. 某些页面的元素是需要向下滚动页面才会加载的。
3. 由于某些其他元素的短暂遮挡，所以无法定位到。

### 2.1. 分辨率原因

这时候应该设置好分辨率，使当前元素能够显示到页面中。

### 2.2. 需要滚动页面

有些页面为了性能的考虑，页面下方不在当前屏幕中的元素是不会加载的，只有当页面向下滚动时才会继续加载。

而selenium本身不提供向下滚动的方法，所以我们需要去用JS去滚动页面：

```python
driver.excute_script("window.scrollTo(0, document.body.scrollHeight)")
```

网上查到的一些滚动方式在Chrome上无效。但这一句是有效的。

### 2.3. 由于其他元素的遮挡

有时候因为一些弹出元素的原因，如果还使用`EC.presence_of_element_located()`的话，我们需要定位的元素就无法被找到，这个时候我们就应该改变我们判断元素的方法：

```python
element = WebDriverWait(driver, 10).until(
    EC.visibility_of_element_located((By.XPATH, ''))
)
```

使用`EC.visibility_of_element_located()`方法可以在等待到当前元素可见后，才获取元素。

在我们找不到元素，或者跟元素无法交互时，应该多去根据当前的情况，灵活选择显式等待的判断方式。