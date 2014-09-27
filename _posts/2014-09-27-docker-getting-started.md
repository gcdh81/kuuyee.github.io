---
layout: post
title: "Docker试用"
description: "Docker的目标是实现轻量级的操作系统虚拟化解决方案"
category: 
tags: [VM,Docker]
---
{% include JB/setup %}

### 创建镜像
可以Docker Hub获取已有镜像并更新，也可以利用本地文件系统创建一个。

从Docker Hub上获取


    sudo docker run -t -i learn/tutorial /bin/bash

由于国内网络的问题，你懂得!很多时候从Docker Hub获取会失败。因此需要在本地磁盘上创建。

本地创建也分为两种方式，一种是全新创建，这种相对比较麻烦，另一种比较方便，就是下载openvz模板，然后从本地导入。

#### 本地导入openvm模板

从[openvm](https://openvz.org/Download/template/precreated)下载镜像，比如下载一个centos-6-x86_64的镜像，然后执行如下命令导入


    sudo cat centos-6-x86_64.tar.gz  |docker import - centos:6
    155228d528ca2057ef7fe18f208b23c02adf195a5d0e03af96c4a4bb7d572349

查看下导入的镜像


    sudo docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
    centos              6                   155228d528ca        About a minute ago   603 MB


#### 修改导入的镜像
下面启动导入的镜像并做一些动作。首先启动：


    sudo docker run -t -i centos:6 /bin/bash
    bash-4.1# 

然后我们在启动的centos容器里安装一个git软件。


    sudo yum install git

然后退出。

#### 提交修改过的镜像容器
前面我们已经在centos容器上做了改动，安装了一个git软件，我需要把做过的操作保存起来，这对自动化部署和运维非常有意义。命令如下：


    docker commit -m 
