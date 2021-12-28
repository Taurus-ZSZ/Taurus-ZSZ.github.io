---
title: Archlinu-install-note
date: 2021-09-13 06:45:21
tags:
    -- Archlinux
categories:
    -- linux
---

[TOC]

# Archlinux 安装笔记

## 问题记录

1. 在使用live系统完成了分区与引导器的安装后，由于我没有安装sudo,dhcpcd导致了我启动系统后无法配置网络，无法连接到网络，无法使用visudo
2. 在发现系统的配置错误后，可以重新通过live镜像进行修改。
3. 为什么那个自由软件者的kde桌面安装教程没有安装xorg
4. 显示主文件磁盘空间不足/分配了10G
5. 



# Arch Linux 日常使用

## 更新系统

```shell
pacman -Syyu    #升级系统中全部包
```

## 安装软件

```shell
pacman -S package_name1 package_name2 ...
pacman -S --noconfirm  gcc 
# man pacman 为什么不能使用
```



## 下载文件

### linux wget指定下载目录和重命名 

当我们在使用wget命令下载文件时，通常会需要将文件下载到指定的目录，这时就可以使用 -P 参数来指定目录，如果指定的目录不存在，则会自动创建。

示例：

wget -P /usr/local/src http://download.redis.io/releases/redis-4.0.9.tar.gz


如果需要对下载的文件进行重命名，则可以通过 -O 参数指定文件名，需要注意的是，如果重命名中包含路径，那么该路径必须实现创建好。

示例：

wget http://download.redis.io/releases/redis-4.0.9.tar.gz -O /usr/local/src/redis.tar.gz

经测试 小写的和大写的都可以 但是大写的会显示下载过程 小写的不会

## 安装激活xmind

```shell
编辑
/etc/environment
添加
VANA_LICENSE_MODE=true
VANA_LIENCE_TO=ZSZ
```





## arch Linux 安装pip的一种方法

文档链接：https://pip.pypa.io/en/stable/installation/

​	

### ⚠警告

```shell
[root@ae5d7bf3b1fc python_pip]# wget 	https://bootstrap.pypa.io/get-pip.py
[root@ae5d7bf3b1fc python_pip]# python3 ./get-pip.py 
Collecting pip
  Downloading pip-21.2.4-py3-none-any.whl (1.6 MB)
     |████████████████████████████████| 1.6 MB 24.0 MB/s 
Collecting setuptools
  Downloading setuptools-58.2.0-py3-none-any.whl (946 kB)
     |████████████████████████████████| 946 kB 32.6 MB/s 
Collecting wheel
  Downloading wheel-0.37.0-py2.py3-none-any.whl (35 kB)
Installing collected packages: wheel, setuptools, pip
Successfully installed pip-21.2.4 setuptools-58.2.0 wheel-0.37.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```





创建我的archlinux 镜像

```dockerfile
FROM archlinux
MAINTAINER zsz<2668838984@qq.com>

ENV MYPATH /home

WORKDIR $MYPATH

RUN pacman -Syuu

RUN pacman -S python3
RUN pacman -S 
```



## 报错

```shell
root@d61d4b3a39dc:/migen# python3 ./setup.py 
Traceback (most recent call last):
  File "./setup.py", line 4, in <module>
    from setuptools import setup
ModuleNotFoundError: No module named 'setuptools'
root@d61d4b3a39dc:/migen# python3 ./setup.py develop --user
Traceback (most recent call last):
  File "./setup.py", line 4, in <module>
    from setuptools import setup
ModuleNotFoundError: No module named 'setuptools'

#安装python3-setuptools
## Ubuntu
apt-get install python3-setuptools
#使用脚本安装
wget  https://bootstrap.pypa.io/get-pip.py
chmod +x ./get-pip.py
python get-pip.py

apt-get install python3-pip
#安装
apt-get install verilator

#archlinux
pacman -S python-setuptools
#这条命令安装后，执行./litex_setup.py init install --user 时出错。 又试了一遍又可以了！！！！
#通过安装pip使用pip install setuptools 后成功安装。
```



# ubuntu 更新软件

```shell

apt-get update: 升级安装包相关的命令,刷新可安装的软件列表(但是不做任何实际的安装动作)

apt-get upgrade: 进行安装包的更新(软件版本的升级)

apt-get dist-upgrade: 进行系统版本的升级(Ubuntu版本的升级)

do-release-upgrade: Ubuntu官方推荐的系统升级方式,若加参数-d还可以升级到开发版本,但会不稳定

```

​	







```shell
docker run -it -v /home/proton/app/config/config.toml:/app/config --name tsrrr 7bc49bdacfa2 /app/rly -config config/config.toml
docker run -it -v /home/proton/app/config/rly.toml:/bin/rly.toml --name tsdrrr 7bc49bdacfa2 /bin/rly -config config/rly.toml


在学习docker的时候，经常会编译docker镜像，很多都是基于上一个Dockerfile修改编译而来，因此出现了很多REPOSITORY 和 TAG 为 none 的镜像。每次 docker images 查看镜像，都会列出一长串，有的时候一屏还展示不全，所以就想要删除某些镜像。但是一个一个删又很费时，那就只有批量删除了。
使用 grep 函数查找出所有包含 none 的镜像，然后使用 awk 函数找出所有的镜像ID，并将它们作为参数，使用 docker rmi 命令删除所有包含 none 的镜像。

# 查询所有匹配上的docker镜像ID
$ docker images | grep none | awk '{print $3}'

docker ps -a | grep 7bc49bdacfa2 | awk '{print $3}'
docker images | grep none | awk '{print $3}' | xargs docker rmi

docker ps -a | grep 7bc49bdacfa2 | awk '{print $1}'
docker ps -a | grep 7bc49bdacfa2 | awk '{print $1}' | xargs docker rm

docker ps -a | grep e8e9f8df3fbc | awk '{print $1}'
docker ps -a | grep e8e9f8df3fbc | awk '{print $1}' | xargs docker rm


docker images | grep jhao104/proxy_pool:latest | awk '{print $3}' | xargs docker rmi
```

```shell
##rly 那个
apt-get install vim
apt-get install make
apt-get install yarn
apt-get install git
```



# 代理搭建proxy_pool



1、pull docker images

```shell
docker pull jhao104/proxy_pool
```

2、运行docker 注意这里的docker使用的是外部的redis,需要重新配置本地的redis

```shell
#我第一步运行的是 
docker pull jhao104/proxy_pool

#然后我就运行了
#注意默认的redis没有密码访问：
docker run --env DB_CONN=redis://@173.82.54.177:6379/0 -p 5010:5010 jhao104/proxy_pool:latest
```


# ubuntu 16.04 安装python3.6后无法打开终端
安装python3.6后 快捷键Ctrl+alt+T不行，在Applications里面点击图标也打不开：
##参考链接
安装python3.6 https://tding.top/archives/3868725a.html
解决参考连接：https://blog.csdn.net/weixin_39717318/article/details/111447840

1.  ctrl+alt+f1 ，输入gnome-terminal，查看无法启动的错误原因

$ gnome-terminal

2. 提示错误信息如下：Traceback (most recent call last):

File "/usr/bin/gnome-terminal", line 9, in

from gi.repository import GLib, Gio

File "/usr/lib/python3/dist-packages/gi/__init__.py", line 42, in

from . import _gi

ImportError: cannot import name '_gi'

3. 查找_gi的库文件$ ls -la /usr/lib/python3/dist-packages/gi/

_gi_cairo.cpython-35m-x86_64-Linux-gnu.so

_gi.cpython-35m-x86_64-Linux-gnu.so

........

4. 复制一份3.6使用的库文件

$ cd /usr/lib/python3/dist-packages/gi/

$ sudo cp _gi.cpython-35m-x86_64-Linux-gnu.so _gi.cpython-36m-x86_64-Linux-gnu.so

$ sudo cp _gi_cairo.cpython-35m-x86_64-Linux-gnu.so _gi_cairo.cpython-36m-x86_64-Linux-gnu.so

注意：
最后，还要注意一下，你的python3是安装在哪里的：

看看python3装在哪儿
由于我的python3.6的位置原因需要将gi/文件夹拷贝到
/usr/local/python3.6/lib/python3.6/site-packages/

所以需要：sudo cp -fr /usr/lib/python3/dist-packages/gi/ /usr/local/python3.6/lib/python3.6/site-packages/
1. alt+ctrl+f7返回，重新尝试打开终端，问题解决





## 从源代码安装Python 3.6之后，没有名为“ lsb_release”的模块

注意由于我的python3.6所在的位置是/usr/local/python3.6

通过 sys.path 找到了我的路径是
/usr/local/python3.6/lib/python3.6/site-packages
所以
```shell
sudo ln -s /usr/share/pyshared/lsb_release.py /usr/local/python3.6/lib/python3.6/site-packages/lsb_release.py
```

```shel
说明：

我们可以看到 /usr/bin/lsb_release

#!/usr/bin/python3 -Es

# lsb_release command for Debian
# (C) 2005-10 Chris Lawrence <lawrencc@debian.org>
#    This package is free software; you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation; version 2 dated June, 1991.
#    This package is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#    You should have received a copy of the GNU General Public License
#    along with this package; if not, write to the Free Software
#    Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA
#    02110-1301 USA
from optparse import OptionParser
import sys
import os
import re

import lsb_release

关键步骤是import lsb_release，但是问题是Python 3.6没有此模块。

因此，您必须已覆盖python3从python3.5到python3.6。这就是为什么你lsb_release坏了。

要进行验证，我们可以在中看到python3.6：

➜  ~ python3.6 
Python 3.6.4 (default, Feb  6 2018, 16:57:12) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import lsb_release
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'lsb_release'

然后在python3.5：

➜  ~ python3.5
Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import lsb_release
>>> lsb_release.__file__
'/usr/lib/python3/dist-packages/lsb_release.py'

该文件在哪里：

➜  ~ ll /usr/lib/python3/dist-packages/lsb_release.py
lrwxrwxrwx 1 root root 38 Jul   7  2016 /usr/lib/python3/dist-packages/lsb_release.py -> ../../../share/pyshared/lsb_release.py

因此，此模块lsb_release存在于中，python3.5但不存在于中python3.6。我们最终找到了它！

现在，通过添加到原始lsb_release.py文件的链接来修复它！

这个对我有用！
```

