# 简介

Podman，Skopeo和Buildah这三个工具都是符合OCI计划下的工具（github/containers）。主要是由RedHat推动的，他们配合可以完成Docker所有的功能，而且不需要守护程序或访问有root权限的组，更加安全可靠，是下一代容器容器工具。

# 安装
## ubuntu安装
适用于ubuntu18.04

```
# 更新源
sudo apt update
# 添加存储库
sudo apt -y  install software-properties-common
sudo add-apt-repository -y ppa:projectatomic/ppa
# 安装
sudo apt -y install podman
```


# Podman
[官方地址](https://podman.io)

Podman可以替换Docker中了大多数子命令（RUN，PUSH，PULL等）。Podman不需要守护进程，而是使用用户命名空间来模拟容器中的root，无需连接到具有root权限的套接字保证容器的体系安全。Podman专注于维护和修改OCI镜像的所有命令和功能，例如拉动和标记。它还允许我们创建，运行和维护从这些图像创建的容器。
## 安装
### ubuntu
```
sudo apt-get update -qq
sudo apt-get install -qq -y software-properties-common uidmap
sudo add-apt-repository -y ppa:projectatomic/ppa
sudo apt-get update -qq
sudo apt-get -qq -y install podman
```

# Buildah

Buildah用来构建OCI图像。虽然Podman也可以用户构建Docker镜像，但是构建速度超慢，并且默认情况下使用vfs存储驱动程序会耗尽大量磁盘空间。 buildah bud（使用Dockerfile构建）则会非常快，并使用覆盖存储驱动程序。

Buildah专注于构建OCI镜像。 Buildah的命令复制了Dockerfile中的所有命令。可以使用Dockerfiles构建镜像，并且不需要任何root权限。 Buildah的最终目标是提供更低级别的coreutils界面来构建图像。Buildah也支持非Dockerfiles构建镜像，可以允许将其他脚本语言集成到构建过程中。 Buildah遵循一个简单的fork-exec模型，不以守护进程运行，但它基于golang中的综合API，可以存储到其他工具中。

# Skopeo

Skopeo是一个工具，允许我们通过推，拉和复制镜像来处理Docker和OC镜像。

# Podman和Buildah对比

Buildah构建容器，Podman运行容器，Skopeo传输容器镜像。这些都是由Github容器组织维护的开源工具（github/containers）。这些工具都不需要运行守护进程，并且大多数情况下也不需要root访问权限。Podman和Buildah之间的一个主要区别是他们的容器概念。 Podman允许用户创建"传统容器"。虽然Buildah容器实际上只是为了允许将内容添加回容器图像而创建的。一种简单方法是buildah run命令模拟Dockerfile中的RUN命令，而podman run命令模拟功能中的docker run命令。简而言之，Buildah是创建OCI图像的有效方式，而Podman允许我们使用熟悉的容器cli命令在生产环境中管理和维护这些图像和容器。