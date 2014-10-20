---
layout: post
title: "Docker问题汇总"
description: "Docker问题汇总"
category: 
tags: [Docker]
---
{% include JB/setup %}

### SELinux policy denies access
错误描述如下：

    2014/10/18 05:36:10 Error response from daemon: Cannot start container acefc5dfa9373410cf7f7253fb07d6cc00ec0bd26c29af9162ecb6204a6c0dc7: SELinux policy denies access.


**解决办法**

关闭SELinux

    setenforce 0

彻底关闭

    [root@localhost ~]# vi /etc/selinux/config  
    
    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #     enforcing - SELinux security policy is enforced.
    #     permissive - SELinux prints warnings instead of enforcing.
    #     disabled - No SELinux policy is loaded.
    SELINUX=disabled //把这里设置成desabled，然后重启服务器
    # SELINUXTYPE= can take one of these two values:
    #     targeted - Targeted processes are protected,
    #     minimum - Modification of targeted policy. Only selected processes are protected.
    #     mls - Multi Level Security protection.
    SELINUXTYPE=targeted


### Docker容器不能SSH登录
错误描如下：

    [docker@127.0.0.1 ~]$ sudo ssh docker@127.0.0.1 -p 50122    
    docker@127.0.0.1's password: 
    Connection to 127.0.0.1 closed.

输入密码后直接退出。

**解决办法**

参考文档: [http://stackoverflow.com/questions/18173889/cannot-access-centos-sshd-on-docker](http://stackoverflow.com/questions/18173889/cannot-access-centos-sshd-on-docker)

编辑容器的`/etc/ssh/sshd_config` 设置`UsePAM no`

    vim /etc/ssh/sshd_config
    
    UsePAM no //设置成no
    #UsePAM yes
    
    # Accept locale-related environment variables
    AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
    AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
    AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
    AcceptEnv XMODIFIERS

最后重启sshd服务

    service sshd restart

