# Pyhton虚拟环境

在开发Python应用程序的时候，系统安装的Python解释器是有限的。不同的应用程序的开发环境可能不一样。如果每个应用程序的开发都用系统的Python解释器，可能会出现环境兼容的问题。

这种情况就需要每个应用程序都有一套“独立”的Python环境，可以通过创建虚拟环境来达到该目的。

Python虚拟环境的主要目的是为了给不同的工程创建互相独立的运行环境。在虚拟环境下，每一个工程都有自己的依赖包，而与其它的工程无关。不同的虚拟环境中同一个包可以有不同的版本。并且，虚拟环境的数量没有限制

其原理就是使用工具把系统Python复制一份到虚拟环境，用命令进入一个虚拟环境时，工具会修改相关环境变量，让命令python和pip均指向当前的虚拟环境。

其好处在于：

- 不会污染系统环境
- 不同项目环境的隔离

## 1 virtualenv

这是一个第三方的虚拟环境创建的库，首先需要安装：

    pip install virtualenv

假定我们需要开发一个新项目，首先需要创建一套独立的Python环境。
    
    mkdir myproject # 创建项目目录
    cd myproject/ # 进入目录
    virtualenv envname # 在当前文件夹下创建虚拟环境，自定义名称

virtualenv为创建虚拟环境的命令，它还有以下参数（`virtualenv -h`查看）：
```
Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -v, --verbose         Increase verbosity.
  -q, --quiet           Decrease verbosity.
  -p PYTHON_EXE, --python=PYTHON_EXE
                        The Python interpreter to use, e.g.,
                        --python=python3.5 will use the python3.5 interpreter
                        to create the new environment.  The default is the
                        interpreter that virtualenv was installed with
                        (/usr/bin/python)
  --clear               Clear out the non-root install and start from scratch.
  --no-site-packages    DEPRECATED. Retained only for backward compatibility.
                        Not having access to global site-packages is now the
                        default behavior.
  --system-site-packages
                        Give the virtual environment access to the global
                        site-packages.
  --always-copy         Always copy files rather than symlinking.
  --relocatable         Make an EXISTING virtualenv environment relocatable.
                        This fixes up scripts and makes all .pth files
                        relative.
  --no-setuptools       Do not install setuptools in the new virtualenv.
  --no-pip              Do not install pip in the new virtualenv.
  --no-wheel            Do not install wheel in the new virtualenv.
  --extra-search-dir=DIR
                        Directory to look for setuptools/pip distributions in.
                        This option can be used multiple times.
  --download            Download preinstalled packages from PyPI.
  --no-download, --never-download
                        Do not download preinstalled packages from PyPI.
  --prompt=PROMPT       Provides an alternative prompt prefix for this
                        environment.
  --setuptools          DEPRECATED. Retained only for backward compatibility.
                        This option has no effect.
  --distribute          DEPRECATED. Retained only for backward compatibility.
                        This option has no effect.
  --unzip-setuptools    DEPRECATED.  Retained only for backward compatibility.
                        This option has no effect.

```
常用参数：

-p：指定需要复制的本地Python解释器路径，默认路径是`/usr/bin/python`

--no-site-packages：创建虚拟环境时不带第三方库。

新建的虚拟环境放在当前目录下的evnname的文件夹下。

然后进入虚拟环境：

    source envname/bin/activate
    
进入后发现在命令提示符前面会有一个包含虚拟环境名称的前缀，表示当前的Python环境。然后就可以在该环境下执行各种操作而不会影响到系统的Python环境。

退出虚拟环境

    deactivate
    
## 2 virtualenvwrapper

虚拟环境的引入解决了我们关于环境冲突的问题，但是它同时也带来了一个问题，就是虚拟环境过多所带来的管理问题。virtualenvwrapper就是专门用来解决虚拟环境管理问题的一个工具。我们可以很方便地用它来实现对虚拟环境的创建，删除，拷贝，并且可以轻松地在不同环境间进行切换。

首先需要安装virtualenvwrapper，安装之前要确定virtualenv已经安装：

    pip install virtualwrapper
    


安装完成后，需要将virtualenvwrapper.sh脚本添加到shell启动文件中。

首先确定virtualenvwrapper.sh的位置

    tys@tys:/$ which virtualenvwrapper.sh
    /usr/local/bin/virtualenvwrapper.sh

然后在shell启动文件~/.bashrc下添加

```
export WORKON_HOME=$HOME/.virtualenvs
Optionalexport PROJECT_HOME=$HOME/projects
Optionalsource /usr/local/bin/virtualenvwrapper.sh
```
$HOME 代表用户主目录

然后运行：

    source ~/.bashrc
    
然后就会在$WORKON_HOME所在的路径下创建集中存放虚拟环境的目录.virtualenvs，之后创建的虚拟环境都会存放在该路径线下。

接下来就可以使用命令管理虚拟环境。

virtualenvwrapper是virtualenv的扩展库，方便管理虚拟环境。

在终端中输入`virtualenvwrapper`可以查看介绍与命令

```
tys@tys:~$ virtualenvwrapper

virtualenvwrapper is a set of extensions to Ian Bicking's virtualenv
tool.  The extensions include wrappers for creating and deleting
virtual environments and otherwise managing your development workflow,
making it easier to work on more than one project at a time without
introducing conflicts in their dependencies.

For more information please refer to the documentation:

    http://virtualenvwrapper.readthedocs.org/en/latest/command_ref.html

Commands available:

  add2virtualenv: add directory to the import path

  allvirtualenv: run a command in all virtualenvs

  cdproject: change directory to the active project

  cdsitepackages: change to the site-packages directory

  cdvirtualenv: change to the $VIRTUAL_ENV directory

  cpvirtualenv: duplicate the named virtualenv to make a new one

  lssitepackages: list contents of the site-packages directory

  lsvirtualenv: list virtualenvs

  mkproject: create a new project directory and its associated virtualenv

  mktmpenv: create a temporary virtualenv

  mkvirtualenv: Create a new virtualenv in $WORKON_HOME

  rmvirtualenv: Remove a virtualenv

  setvirtualenvproject: associate a project directory with a virtualenv

  showvirtualenv: show details of a single virtualenv

  toggleglobalsitepackages: turn access to global site-packages on/off

  virtualenvwrapper: show this help message

  wipeenv: remove all packages installed in the current virtualenv

  workon: list or change working virtualenvs

```

常见命令：

创建新虚拟环境mkvirtualenv：

```
tys@tys:~$ mkvirtualenv --help
Usage: mkvirtualenv [-a project_path] [-i package] [-r requirements_file] [virtualenv options] env_name

 -a project_path

    Provide a full path to a project directory to associate with
    the new environment.

 -i package

    Install a package after the environment is created.
    This option may be repeated.

 -r requirements_file

    Provide a pip requirements file to install a base set of packages
    into the new environment.

virtualenv help:

Usage: virtualenv [OPTIONS] DEST_DIR

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -v, --verbose         Increase verbosity.
  -q, --quiet           Decrease verbosity.
  -p PYTHON_EXE, --python=PYTHON_EXE
                        The Python interpreter to use, e.g.,
                        --python=python3.5 will use the python3.5 interpreter
                        to create the new environment.  The default is the
                        interpreter that virtualenv was installed with
                        (/usr/bin/python)
  --clear               Clear out the non-root install and start from scratch.
  --no-site-packages    DEPRECATED. Retained only for backward compatibility.
                        Not having access to global site-packages is now the
                        default behavior.
  --system-site-packages
                        Give the virtual environment access to the global
                        site-packages.
  --always-copy         Always copy files rather than symlinking.
  --relocatable         Make an EXISTING virtualenv environment relocatable.
                        This fixes up scripts and makes all .pth files
                        relative.
  --no-setuptools       Do not install setuptools in the new virtualenv.
  --no-pip              Do not install pip in the new virtualenv.
  --no-wheel            Do not install wheel in the new virtualenv.
  --extra-search-dir=DIR
                        Directory to look for setuptools/pip distributions in.
                        This option can be used multiple times.
  --download            Download preinstalled packages from PyPI.
  --no-download, --never-download
                        Do not download preinstalled packages from PyPI.
  --prompt=PROMPT       Provides an alternative prompt prefix for this
                        environment.
  --setuptools          DEPRECATED. Retained only for backward compatibility.
                        This option has no effect.
  --distribute          DEPRECATED. Retained only for backward compatibility.
                        This option has no effect.
  --unzip-setuptools    DEPRECATED.  Retained only for backward compatibility.
                        This option has no effect.

```


查看当前有哪些虚拟环境：`workon`
创建虚拟环境，默认创建在home目录下的`.virtualenvs`文件夹下：

```
mkvirtualenv -p /usr/bin/python3 envname
```

`/usr/bin/python3`:指定本地python路径方便复制

`envname`:虚拟环境名称

进入虚拟环境：`workon envname`
退出虚拟环境：`deactive`
删除虚拟环境：`rmvirtualenv envname`
