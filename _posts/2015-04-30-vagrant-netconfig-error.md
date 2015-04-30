---
layout: post
title: "Vagrant up netconfig error"
description: ""
category: 
tags: [vm,vagrant]
---
{% include JB/setup %}

### Vagrant启动Box网卡配置报错

**错误信息**

    ==> centos7-docker: Configuring and enabling network interfaces...
    The following SSH command responded with a non-zero exit status.
    Vagrant assumes that this means the command failed!
    
    ARPCHECK=no /sbin/ifup eth1 2> /dev/null
    
    Stdout from the command:

**问题分析**：

Vagrant升级到1.6.5以上版本就没问题了
