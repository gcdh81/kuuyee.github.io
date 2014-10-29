---
layout: post
title: "Docker容器配置SSH登录"
description: "Docker容器配置SSH登录，并挂载本地目录"
category: 
tags: [Docker]
---
{% include JB/setup %}

### 容器启动

    sudo docker run -d -p 55522:22 --name kuuyee -v ~/data:/opt/data centos:v602 /usr/sbin/sshd -D
    66164d40fb6fb1f862d89521baa94db6ea6e0d43ca7478748ade5440b0f91ca5

参数说明

`-d` : Docker 容器在后台以守护态（Daemonized）形式运行

`-p` : （小写的）则可以指定要映射的端口，上面的命令绑定容器`22`端口到主机的`55522`端口。并且，在一个指定端口上只可以绑定一个容器。支持的格式有 `ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort。`

`--name`: 给容器指定一个名字

`-v` : 指定挂载一个本地主机的目录到容器中去，上面的命令加载主机的 `~/data` 目录到容器的 `/opt/data` 目录

### SSH客户端访问容器

    [docker@localhost data]$ ssh root@127.0.0.1 -p 55522
    The authenticity of host '[127.0.0.1]:55522 ([127.0.0.1]:55522)' can't be established.
    RSA key fingerprint is f5:e6:0a:fb:b9:f7:3b:e4:24:06:d3:80:0b:c3:0d:c0.
    Are you sure you want to continue connecting (yes/no)? yes

> 需要注意的是，通常获取的容器没有设定`root`密码，可以事先设定好。
