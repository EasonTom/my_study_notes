桌面图标是存放在/usr/share/applications目录下的desktop文件。

我们到/usr/share/applications目录下，创建一个desktop为后缀的文件，文件名就起软件名，并且从网上找到一个可以用来显示的图标。

然后编辑Desktop文件：
```

[Desktop Entry]

Type=Application

Name=ICQ

Icon=/home/lemontea/tjzwork/app/icq/icq.png

Exec=/home/lemontea/tjzwork/app/icq/icq

Terminal=false

Categories=Network;InstantMessaging
```


第一行是必须的，就像shell脚本要加入#!/bin/bash一样，用于系统识别

第二行Type一般填写Application就可以了

第三行Name自己随意填，用于显示和搜索

第四行Icon是指应用图标的路径

第五行Exec是指应用可执行文件路径

第六行Terminal表示启动时是否需要显示终端，建议设置为false

第七行是指这个应用的分类，由于icq是网络聊天工具，这里就设置为Network;InstantMessaging

例如生成pycharm的桌面图标：

```
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=bash /opt/pycharm-2019.1.3/bin/pycharm.sh
Icon=/opt/pycharm-2019.1.3/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;
```


打开应用程序搜索框，输入desktop文件中的Name，如果找得到，就表示之前的desktop文件生效了。

现在点击这个图标就可以打开对应的应用了，你也可以将图标拖放到左侧边缘的快速启动栏中。

语法：


字段|含义
-|-
[Desktop Entry] | 文件头
Encoding |	编码
Name |	应用名称
Name[xx] |	不同语言的应用名称
Comment |	描述
Exec |	执行的命令
Icon |	图标路径
Terminal |	是否使用终端
Type |	启动器类型
Categories |	应用的类型（内容相关）