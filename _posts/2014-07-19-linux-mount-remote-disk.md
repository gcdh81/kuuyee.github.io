---
layout: post
title: "Linux下挂载远程目录"
description: "Linux下挂载远程目录"
category: 
tags: [Architecture]
---
{% include JB/setup %}

**1.配置被挂载的机器**

假设本机IP为`192.168.0.202` ，编辑`/etc/exports`文件，加入如下内容:


    /opt/oracle 192.168.0.117(rw,all_squash,anonuid=901,anongid=1001)  //制定192.168.0.117那台机器可以挂载本机的/opt/oracle目录 901是117那台机上的oracle用户id
    /devsoft   10.128.143.135(rw,no_root_squash) 


**2. 启动NFS服务**


    service nfs start
    Starting NFS services:                                     [  OK  ]
    Starting NFS quotas:                                       [  OK  ]
    Starting NFS mountd:                                       [  OK  ]
    Starting NFS daemon:                                       [  OK  ]
    Starting RPC idmapd:                                       [  OK  ]


**3.在远程挂载本机目录**

root登录`192.168.0.117`服务器，执行如下命令：


    mount -t nfs 192.168.0.202:/opt/oracle  /opt/oracle


现在可以在`/opt/oracle`看到`192.168.0.202`服务器的内容了。
