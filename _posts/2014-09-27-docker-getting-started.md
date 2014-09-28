---
layout: post
title: "Docker试用"
description: "Docker的目标是实现轻量级的操作系统虚拟化解决方案"
category: 
tags: [VM,Docker]
---
{% include JB/setup %}

### 安装Docker
官方推荐ubuntu安装docker，用惯了centos，所以选择在centos上安装。如果是centos6需要安装epel源，如果是centos7可以直接`yum install docker`,我的是centos6,安装过程如下


    $ cd /etc/yum.repos.d/
    $ sudo wget -c "http://mirrors.sohu.com/fedora-epel/6Server/x86_64/epel-release-6-8.noarch.rpm"
    $ sudo rpm -ivh epel-release-6-8.noarch.rpm 
    $ sudo yum install docker-io

如果使用docker报`dial unix /var/run/docker.sock: no such file or directory`的错误，可能是由于Docker守护程序没在运行。检查Docker守护程序的状态，确保先启动它。


    $ sudo service docker start
    Starting cgconfig service:                                 [  OK  ]
    Starting docker:                                           [  OK  ]

### 创建镜像
可以Docker Hub获取已有镜像并更新，也可以利用本地文件系统创建一个。

从Docker Hub上获取


    $ sudo docker run -t -i learn/tutorial /bin/bash

>如果你遇到如下的权限问题：
>
>
> >   dial unix /var/run/docker.sock: permission denied
>
>
>可以使用下面的命令修改权限并重启docker服务
>
>
>>    $ sudo chmod a+rw /var/run/docker.sock
>>    $ soudo service docker restart
>

由于国内网络的问题，你懂得!很多时候从Docker Hub获取会失败。因此需要在本地磁盘上创建。

本地创建也分为两种方式，一种是全新创建，这种相对比较麻烦，另一种比较方便，就是下载openvz模板，然后从本地导入。

#### 本地导入openvm模板

从[openvm](https://openvz.org/Download/template/precreated)下载镜像，比如下载一个centos-6-x86_64的镜像，然后执行如下命令导入


    $ sudo cat centos-6-x86_64.tar.gz  |docker import - centos:6
    155228d528ca2057ef7fe18f208b23c02adf195a5d0e03af96c4a4bb7d572349

如果遇到如下的错：


    unable to remount sys readonly: unable to mount sys as readonly max retries reached

碰到这个问题需要修改Docker的配置参数把/etc/sysconfig/docker文件中的other-args更改为：


    other_args="--exec-driver=lxc --selinux-enabled"



查看下导入的镜像


    $ sudo docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
    centos              6                   155228d528ca        About a minute ago   603 MB


#### 修改导入的镜像
下面启动导入的镜像并做一些动作。首先启动：


    $ sudo docker run -t -i centos:6 /bin/bash
    bash-4.1# 

有些镜像容器登录设置成进入bash，这样的话就不显示本次操作容器的ID，切换到其它用户下就可以看到ID了。


    bash-4.1# su -
    [root@ddaa7947c3b2 ~]# 

可以看到，`ddaa7947c3b2`就是本次操作的ID，然后我们在启动的centos容器里安装一个git软件。


    $ sudo yum install git

然后退出。

### 提交修改过的镜像容器
前面我们已经在centos容器上做了改动，安装了一个git软件，我需要把做过的操作保存起来，这对自动化部署和运维非常有意义。命令如下：


    $ sudo docker commit -m "Added git yum" -a "Docker KuuYee" ddaa7947c3b2 centos:6v2
    901d50978d5d02d4ad59d0bbc164e352e23a64ebfa263355d9c5e3c1e18da44b

再来看下docker容器列表


    $ sudo docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    centos              6v2                 901d50978d5d        6 minutes ago       804.7 MB
    centos              6                   155228d528ca        43 minutes ago      603 MB

可以看到多了一个`6v2`的centos容器。这就是我们保存了git安装操作的容器，下面我们登录这个容器，看看安装的git软件还在不！


    $ sudo docker run -t -i centos:6v2 /bin/bash
    bash-4.1# git --version
    git version 1.7.1

到此试用完毕，比起以前的虚拟机方案轻量多了，真的很棒！
