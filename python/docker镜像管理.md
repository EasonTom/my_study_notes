# Docker系统管理

## 1 简介

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 `Linux `机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。该容器基于`Linux`系统

### 1.1 Docker的应用场景

- Web 应用的自动化打包和发布。

- 自动化测试和持续集成、发布。

- 在服务型环境中部署和调整数据库或其他的后台应用。

- 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

### 1.2 Docker 的优点
1. 简化程序：
Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的	任务，在Docker容器的处理下，只需要数秒就能完成。

2. 避免选择恐惧症：
如果你有选择恐惧症，还是资深患者。Docker 帮你	打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。

3. 节省开支：
一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。

## 2 Docker架构
Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。

![Dcoker架构](http://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)

- Docker 镜像(Images)

Docker 镜像是用于创建 Docker 容器的模板。

- Docker 容器(Container)

容器是独立运行的一个或一组应用。

- Docker 客户端(Client)

Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。

- Docker 主机(Host)

一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。

- Docker 仓库(Registry)

Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。

Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。

- Docker Machine

Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。

## 3 安装与配置

### 3.1 安装：
Docker 要求 Ubuntu 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的 Ubuntu 版本是否支持 Docker。

通过 uname -r 命令查看你当前的内核版本

    uname -r

直接安装

    sudo apt-get install docker.io

使用脚本安装

    wget -q0- https://get.docker.com/ | sh

启动docker后台服务

    sudo service docker start

测试运行

    sudo docker run hello-world

### 3.2 添加用户组：

docker命令会与服务器进行socket通信，这时候只有root用户和docker用户组的用户才有使用权限，所以需要将当前用户加入docker用户组：

```
sudo usermod -aG docker $USER
```

添加后重启才会生效

### 3.3 镜像加速

本地没有镜像时会从docker官网上下载镜像，但是国外网站下载较慢，所以换个网站加速下载。

+ 打开阿里云服务器网站，找到镜像加速服务
+ 打开`/etc/docker/daemon.json`，如果没有就创建
+ 写入加速器地址

```
{
  "registry-mirrors": ["https://r8458fw7.mirror.aliyuncs.com"]
}
```

+ 重启服务

## 4 常用命令

docker 客户端非常简单 ,我们可以直接输入 docker 命令来查看到 Docker 客户端的所有命令选项。

```
(py3env) tys@tys:~$ docker

Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/home/tys/.docker")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/tys/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/tys/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/tys/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  config      Manage Docker configs
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```
使用`docker COMMAND --help`命令查看`COMMAND`命令详细用法。

### 4.1 镜像

镜像：特殊的文件系统，除了提供容器运行时的容器、库、资源等文件，还包括运行需要的参数、环境变量、用户等。但是不包含动态数据。

当运行容器时，使用的镜像如果在本地中不存在，docker 就会自动从 docker 镜像仓库中下载，默认是从 Docker Hub 公共镜像源下载。

- 查找镜像

我们可以从 Docker Hub 网站来搜索镜像，Docker Hub 网址为： https://hub.docker.com/
我们也可以使用 docker search 命令来搜索镜像。比如我们需要一个httpd的镜像来作为我们的web服务。我们可以通过 docker search 命令搜索 httpd 来寻找适合我们的镜像。

    docker search httpd

- 获取镜像

```
docker pull ubuntu:16.04
```



- 查看本地镜像

```
docker image ls
```

```
(py3env) tys@tys:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mypython            latest              11a6165c2501        8 weeks ago         927MB
python              latest              2fbd95050b66        2 months ago        927MB
hello-world         latest              fce289e99eb9        2 months ago        1.84kB

```
REPOSITORY：表示镜像的仓库源

TAG：镜像的标签，同一仓库源可以有多个TAG，代表这个仓库源的不同个版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。如果不指定TAG，默认使用latest（最近的）。

IMAGE ID：镜像ID，唯一

CREATED：镜像创建时间

SIZE：镜像大小

- 删除本地镜像：

```
docker rmi image-id
```

如果删除正在使用的镜像会删除失败，可以先停止镜像在删除或者使用参数`-f`强制删除。

- 创建镜像
当我们从docker镜像仓库中下载的镜像不能满足我们的需求时，我们可以通过以下两种方式对镜像进行更改。

1. 从已经创建的容器中更新镜像，并且提交这个镜像
2. 使用 Dockerfile 指令来创建一个新的镜像（见下定制镜像）

- 更新镜像

更新镜像之前，我们需要使用镜像来创建一个容器。

    runoob@runoob:~$ docker run -t -i ubuntu:15.10 /bin/bash
    

在运行的容器内使用 apt-get update 命令进行更新。

在完成操作之后，输入 exit命令来退出这个容器。

此时ID为e218edb10161的容器，是按我们的需求更改的容器。我们可以通过命令 docker commit来提交容器副本。

```
runoob@runoob:~$ docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2
sha256:70bf1840fd7c0d2d8ef0a42a817eb29f854c1af8f7c59fc03ac7bdee9545aff8
```
各个参数说明：

-m:提交的描述信息

-a:指定镜像作者

e218edb10161：容器ID

runoob/ubuntu:v2:指定要创建的目标镜像名

然后使用新镜像重启容器

### 4.2 容器

查看容器：

```
docker ps
```

系统会列出当前正在运行的容器

-a  查看所有运行过的容器



运行容器：

```
docker run 镜像名：版本号 linux命令
```

版本号和linux命令可省，linux命令默认为`/bin/bash`

如果镜像在本地不存在，系统会去docker系统仓库中下载并运行。

-t  启动终端

-i  接受输入

--name=name  指定运行时容器名

-p 80:80  转发容器的端口

-d  后台运行容器，返回容器的id



启动容器完成后查看文件内容：

```
docker run 镜像名：版本号 cat /etc/os-release
```



停止容器:

```
docker stop containerid
```



删除容器：

```
docker rm containerid
```

如果删除正在使用的容器会删除失败，可以先停止使用再删除或者使用参数`-f`强制删除。



查看容器的日志信息：

```
docker logs containerid
```





## 5 定制镜像 

除了使用定制好的镜像外，我们也可以通过定制实现符合自己环境的镜像。
在docker里面通过build方法来生成镜像,在生成镜像之前，我们需要一个Dockerfile脚本，脚本中包含的是一条一条的指令,用来的表示镜像的构建。

这里我们以原先的nginx镜像为基础,使用Dockerfile重新定制一下，
* 1.建立一个Dockerfile文件
* 2.在Dockerfile中编写 `FROM nginx` `RUN echo '<h1>Hello DockerFile!</h1>' > /usr/share/nginx/html/index.html`
  这个 Dockerfile 很简单，一共就两行。涉及到了两条指令，FROM 和 RUN。

### FROM 指定基础镜像
所谓定制镜像，那一定是以一个镜像为基础，在其上进行定制。
而 FROM 就是指定基础镜像，因此一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。

### RUN 执行指令
RUN 指令是用来执行命令行命令的。由于命令行的强大能力，RUN 指令在定制镜像时是最常用的指令之一。

这样我们就生成了一个新的镜像。这个Dockerfile比较简单 其中涉及到两个指令`FROM`和`RUN`。
在定制镜像的时候，一般是以一些基础镜像为基础,在这之上进行定制。
这里的`FROM nginx` 就是在`nginx`镜像的基础上进行的定制。
而`RUN` 指执行命令 类似于在shell中执行。
里我们使用了 docker build 命令进行镜像构建。其格式为：
`docker build [选项] <上下文路径/URL/->`
需要注意的是这里的上下文路径 不是生成镜像存放的位置，而是在生成镜像发送给docker引擎的路径。

### 构建镜像 

Dockerfile文件完成之后，通过`Docker build`命令进行构建镜像。
```
wangxian@wangxian:~/wangxian/Docker_test/code1$ docker build -t my_nginx:v1 .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM nginx
 ---> cd5239a0906a
Step 2/2 : RUN echo '<h1>Welcome to Docker!!<h1>' > /usr/share/nginx/html/index.html
 ---> Running in 4d51e06eccd5
Removing intermediate container 4d51e06eccd5
 ---> 9c8135c4b40e
Successfully built 9c8135c4b40e
Successfully tagged my_nginx:v1
wangxian@wangxian:~/wangxian/Docker_test/code1$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
my_nginx            v1                  9c8135c4b40e        5 seconds ago       109MB
ubuntu              latest              113a43faa138        2 weeks ago         81.2MB
nginx               latest              cd5239a0906a        2 weeks ago         109MB
```
在这里我们指定了最终镜像的名称 -t my_nginx:v1，构建成功后，我们可以像之前运行nginx那样来运行这个镜像.
```
wangxian@wangxian:~/wangxian/Docker_test/code1$ docker run -d -p 80:80 my_nginx:v1 
f30c9abd13de09e783f8b7db11833f31180becdb1bc5abc2243dd28f178e8592
```
### 设置镜像标签
我们可以使用 docker tag 命令，为镜像添加一个新的标签。

    docker tag imageid username/imagename:tag
    
imageid：镜像id
username：docker账号名
imagename:tag   镜像名和版本，这里可以指定版本

### 上下文环境


### Dockerfile常用指令

在定制镜像时,除了上面所讲到的FROM和RUN之外,还有一些其他的常用的指令。

1. COPY 复制文件
      COPY 指令将从构建上下文目录中<源路径>的文件复制到新的一层的镜像内的<目标路径>位置。
```
FROM nginx 
RUN mkdir app
COPY ./index.html ./app

FROM ubuntu:16.04
COPY ./code.txt /home/
```

2. CMD容器启动命令
  cmd给出的是一个容器的默认的可执行体。也就是容器启动以后，默认的执行的命令。重点就是这个“默认”。意味着，如果docker run没有指定任何的执行命令或者dockerfile里面也没有entrypoint，那么，就会使用cmd指定的默认的执行命令执行。

CMD 指令的格式有两种
```
shell 格式：CMD <命令>
exec 格式：CMD ["可执行文件", "参数1", "参数2"...]

第一种
FROM ubuntu
CMD echo 'Hello CMD!!'
wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run e8c9913f85b4
Hello CMD!
运行之后,会执行语句。但是不支持添加参数

第二种
FROM ubuntu
CMD ["/bin/bash", "-c", "echo 'hello cmd!'"]

wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v1 
Hello CMD!!
wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v1 echo hhhh
hhhh
```
从结果可以发现 使用第二种方式 支持在运行容器是 添加参数 但是会覆盖掉CMD本身的命令。

3. ENTRYPOINT 入口命令
  ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数。不同的是当指定了 ENTRYPOINT 后，CMD 的含义就发生了改变，不再是直接的运行其命令，而是将 CMD 的内容作为参数传给 ENTRYPOINT 指令，换句话说实际执行时，将变为`ENTRYPOINT  CMD`.

```
FROM ubuntu
CMD ["Hello CMD!!"]
ENTRYPOINT ["echo"]

wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v2 
Hello CMD!!
wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v2 aaaa
aaaa
```

## 6 docker部署

1. 编写项目运行的py文件

在项目根目录创建项目运行文件main.py

```
from scrapy.cmdline import execute
execute(['scrapy', 'crawl', 'news_spider'])
```

2. 创建Dockfile文件

在scrapy项目根目录下中创建并编写Dockfile文件：

```
FROM python3.6  # 指定环境
COPY . /code  	# 将代码传入容器
WORKDIR /code  # 指定工作目录
RUN pip install scrapy
CMD ['python', 'main.py']
```

3. 生成镜像



commit修改镜像。



## 7 仓库

[官方仓库](https://www.docker.com/)：

下载 pull

上传 push，需要指定名字，通过`docker tag`命令修改镜像名称和版本，详见上面。

**上传之前先要登录Docker账户**

`docker login`  登录

`docker logout`  退出登录

私有仓库：

可以通过docker.registry创建私有仓库。

## 8 [Docker命令大全](http://www.runoob.com/docker/docker-command-manual.html)