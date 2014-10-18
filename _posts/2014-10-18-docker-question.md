---
layout: post
title: "Docker问题汇总"
description: "Docker问题汇总"
category: 
tags: [docker]
---
{% include JB/setup %}

### SELinux policy denies access
错误描如下：

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
