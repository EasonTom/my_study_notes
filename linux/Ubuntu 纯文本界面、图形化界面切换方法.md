# Ubuntu 纯文本界面、图形化界面切换方法
## 1 图形化界面与纯文本界面的动态切换方法

Ubuntu系统默认以图形化界面方式启动，进入图形化界面后，若要切换到纯文本界面，一般可以按“Ctrl + Alt + F1（或F2-F6）”快捷键，在文本终端中输入用户名、密码后登录即可，
该方法的缺点是，如果安装的是Ubuntu 16.04系统中文版，则切换到文本界面后，汉字会显示乱码（如上图所示）。更让人不能忍受的是，如果要执行管理员权限命令“sudo”，则根本无法完成，并且因为汉字显示乱码，错误原因无法查询。


在文本界面中，若需彻底关闭图形界面服务，则执行如下命令：


    sudo service lightdm stop


当然，要重新开启图形界面服务，则输入如下命令：

    sudo service lightdm start


最后，若要从文本界面切换回图形化界面，按下“Ctrl + Alt + F7”快捷键即可。
（如果不管用，终端输入命令`startx`启动x界面）
## 2 图形化界面下，修改系统登录界面为文本界面的配置方法

有时需要启动后直接就是文本界面，需要修改如下配置。


首先，按下“Ctrl+Alt+T”快捷键，打开一个命令终端，使用vi编辑器修改此文件：/etc/default/grub

    sudo vi /etc/default/grub

修改内容为：

1. 将此行用“#”注释：GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"；

2. 将GRUB_CMDLINE_LINUX="" 修改为：GRUB_CMDLINE_LINUX="text"；
3. 将#GRUB_TERMINAL=console前的“#”号去除，即反注释该行。


保存修改后的配置文件，重新回到命令行界面，按顺序执行如下命令（一次执行一条）：

    sudo update-grub 
    sudo systemctl set-default multi-user.target


最后关闭其他程序，输入如下命令重启电脑：
    
    shutdown -r now

重启后自动进入文本界面

## 3 文本界面下，修改系统登录界面为图形化界面的配置方法

如果安装的是Ubuntu 16.04系统中文版，则在文本界面中汉字仍会显示为乱码。若以普通用户登录，需执行管理员权限命令“sudo”时，同样无法完成，错误原因显示为乱码（与第一部分描述的问题现象一致）。要将系统登录界面切换为图形化界面，可以使用如下两种方法：一是以root用户登录（如果系统未启用root用户，则使用方法二），再修改相关配置文件。二是执行命令`sudo init 5`进入图形化界面，再修改相关配置文件。

创建root账户的方法：

    sudo passwd root

修改配置文件的方法同上：

    vi /etc/default/grub

1. 将此行的“#”号去除，即反注释该行：#GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"；
2. 将GRUB_CMDLINE_LINUX="text" 修改为：GRUB_CMDLINE_LINUX=""；
3. 将此行用“#”注释：GRUB_TERMINAL=console。


保存修改后的配置文件，重新回到命令行界面，按顺序执行如下命令（一次执行一条）：

    update-grub 
    systemctl set-default graphical.target

最后关闭程序重启电脑。