---
layout: post
title: "Linux代理服务Squid安装"
description: "Linux代理服务Squid安装"
category: 
tags: [Linux,squid]
---
{% include JB/setup %}

### 安装Squid

    yum install squid

### 配置Squid

    [root@vagrant2 ~]# vim /etc/squid/squid.conf
    
    # We strongly recommend the following be uncommented to protect innocent
    # web applications running on the proxy server who think the only
    # one who can access services on "localhost" is a local user
    #http_access deny to_localhost
    
    #
    # INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
    #
    
    # Example rule allowing access from your local networks.
    # Adapt localnet in the ACL section to list your (internal) IP networks
    # from where browsing should be allowed
    http_access allow localnet
    http_access allow localhost
    
    # And finally deny all other access to this proxy
    http_access deny all
    
    # Squid normally listens to port 3128
    http_port 3128
    
    # We recommend you to use at least the following line.
    hierarchy_stoplist cgi-bin ?
    
    # Uncomment and adjust the following to add a disk cache directory.
    #cache_dir ufs /var/spool/squid 100 16 256
    
    # Leave coredumps in the first cache dir
    coredump_dir /var/spool/squid
    
    #加入下面的参数设置
    minimum_object_size 0 KB      
    maximum_object_size 4096 KB
    cache_swap_low 90
    cache_swap_high 95
    
    # Add any of your own refresh_pattern entries above these.
    refresh_pattern ^ftp:           1440    20%     10080
    refresh_pattern ^gopher:        1440    0%      1440
    refresh_pattern -i (/cgi-bin/|\?) 0     0%      0
    refresh_pattern .               0       20%     4320

### 启动Squid服务

    service squid start

注意Squid的默认端口为`3128`

### 使用代理服务

假设Squid服务器的IP地址为`192.168.1.100`，那么在需要设置HTTP代理服务的地方输入`http://192.168.1.100:3128`即可。
