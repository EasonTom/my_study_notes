一个python项目写完需要在服务器端一直运行，需要用到如下命令：
    
    python /data/python/server.py >python.log &

说明：     
1、 > 表示把标准输出（STDOUT）重定向到 那个文件，这里重定向到了python.log     
2、 & 表示在后台执行脚本这样可以到达目的。

但是，我们退出shell窗口的时候，必须用exit命令来退出，否则，退出之后，该进程也会随着shell的消失而消失（退出、关闭）

而且，在python运行中却查看不到输出！

因为：
python的输出有缓冲，导致python.log3并不能够马上看到输出。

使用-u参数，使得python不启用缓冲。

所以改正命令，就可以正常使用了

    nohup python -u test.py >> out.log 2>&1 &
    
说明：
nohup：程序不会被打断，只有使用kill杀死。

-u:不启用缓冲

`>>`：重定向，它和`>`的区别在于`>>`不会覆盖原有文件，会在原文件的基础上继续写入。

2>&1：此选项可以写入程序自己的日志中

&：后台运行

最后，使用 jobs -l 可查看后台正在执行的程序

jobs命令只看当前终端生效的，关闭终端后，在另一个终端jobs已经无法看到后台跑得程序了，此时利用ps（进程查看命令）

用ps -def | grep查找进程很方便，最后一行总是会grep自己
