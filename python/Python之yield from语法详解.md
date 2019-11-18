# yield from

[详解](https://www.cnblogs.com/wongbingming/p/9085268.html)

yield from 是在Python3.3才出现的语法。所以这个特性在Python2中是没有的。

yield from 后面需要加的是可迭代对象，它可以是普通的可迭代对象，也可以是迭代器，甚至是生成器。

区别是yield from后面加上可迭代对象，他可以把可迭代对象里的每个元素一个一个的yield出来，对比yield来说代码更加简洁，结构更加清晰。