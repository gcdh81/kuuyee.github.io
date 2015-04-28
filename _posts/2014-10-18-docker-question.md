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


### Docker容器不能修改内核参数
错误描如下：

    -bash-4.1# sysctl -p
    error: "Read-only file system" setting key "net.ipv4.ip_forward"
    error: "Read-only file system" setting key "net.ipv4.conf.default.rp_filter"
    error: "Read-only file system" setting key "net.ipv4.conf.default.accept_source_route"
    error: "Read-only file system" setting key "kernel.sysrq"
    error: "Read-only file system" setting key "kernel.core_uses_pid"
    error: "net.ipv4.tcp_syncookies" is an unknown key
    error: "net.bridge.bridge-nf-call-ip6tables" is an unknown key
    error: "net.bridge.bridge-nf-call-iptables" is an unknown key
    error: "net.bridge.bridge-nf-call-arptables" is an unknown key
    error: "Read-only file system" setting key "kernel.msgmnb"
    error: "Read-only file system" setting key "kernel.msgmax"
    error: "Read-only file system" setting key "kernel.shmmax"
    error: "Read-only file system" setting key "kernel.shmall"

**解决办法**

参考文档: [http://tonybai.com/2014/10/14/discussion-on-the-approach-to-modify-system-variables-in-docker/](http://tonybai.com/2014/10/14/discussion-on-the-approach-to-modify-system-variables-in-docker/)

容器启动时加入参数`--privileged`特权模式

    docker run -d --privileged -p 22 kuuyee/centos6:ofm-base

这样就可以配置内核参数了

    -bash-4.1# echo 68719476736 > /proc/sys/kernel/shmmax
    -bash-4.1# sysctl -p
    net.ipv4.ip_forward = 0
    net.ipv4.conf.default.rp_filter = 1
    net.ipv4.conf.default.accept_source_route = 0
    kernel.sysrq = 0
    kernel.core_uses_pid = 1
    error: "net.ipv4.tcp_syncookies" is an unknown key
    error: "net.bridge.bridge-nf-call-ip6tables" is an unknown key
    error: "net.bridge.bridge-nf-call-iptables" is an unknown key
    error: "net.bridge.bridge-nf-call-arptables" is an unknown key
    kernel.msgmnb = 65536
    kernel.msgmax = 65535
    kernel.shmmax = 68719476736 //设置成功
    kernel.shmall = 4294967296


### Docker Registry Push 报错
错误描如下：

    FATA[0002] Error: Invalid registry endpoint https://0.0.0.0:5000/v1/: Get https://0.0.0.0:5000/v1/_ping: EOF. If      this private registry supports only HTTP or HTTPS with an unknown CA certificate, please add `--insecure-registry     0.0.0.0:5000` to the daemon's arguments. In the case of HTTPS, if you have access to the registry's CA        
    certificate, no need for the flag; simply place the CA certificate at /etc/docker/certs.d/0.0.0.0:5000/ca.crt 

**解决办法**
修改Docker配置文件

    vim /etc/default/docker

增加以下一行

    DOCKER_OPTS="$DOCKER_OPTS --insecure-registry=104.131.173.242:5000"

重启Docker
sudo service docker restart
