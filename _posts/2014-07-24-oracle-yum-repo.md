---
layout: post
title: "ORACLE官方YUM源配置"
description: "ORACLE官方YUM源配置"
category: 
tags: [Linux,YUM]
---
{% include JB/setup %}

安装Oracle软件经常要解决系统环境依赖的问题，其实Oracle提供了一个YUM服务器，方便我们安装依赖库。

官方地址：http://public-yum.oracle.com

**1.安装源文件**

Oracle Linux 7


                    # cd /etc/yum.repos.d
                    # wget http://public-yum.oracle.com/public-yum-ol7.repo


Oracle Linux 6


                    # cd /etc/yum.repos.d
                    # wget http://public-yum.oracle.com/public-yum-ol6.repo


Oracle Linux 5


                    # cd /etc/yum.repos.d
                    # wget http://public-yum.oracle.com/public-yum-el5.repo


Oracle Linux 4, Update 6 or Newer


                    # cd /etc/yum.repos.d
                    # mv Oracle-Base.repo Oracle-Base.repo.disabled
                    # wget http://public-yum.oracle.com/public-yum-el4.repo


**2.安装GPG Key**

Red Hat Enterprise Linux 4, Update 6 or Compatible


                    # wget http://public-yum.oracle.com/RPM-GPG-KEY-oracle-el4 -O /usr/share/rhn/RPM-GPG-KEY-oracle
                    # gpg --quiet --with-fingerprint /usr/share/rhn/RPM-GPG-KEY-oracle
                    pub  1024D/B38A8516 2006-09-05 Oracle OSS group (Open Source Software group) 
                         Key fingerprint = 1122 A29A B257 825F 322C  234E 2E2B CDBC B38A 8516
                    sub  2048g/0042D4F4 2006-09-05 [expires: 2011-09-04]


Red Hat Enterprise Linux 5 or Compatible


                    # wget http://public-yum.oracle.com/RPM-GPG-KEY-oracle-el5 -O /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
                    # gpg --quiet --with-fingerprint /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
                    pub  1024D/1E5E0159 2007-05-18 Oracle OSS group (Open Source Software group) 
                          Key fingerprint = 99FD 2766 28EE DECB 5E5A  F5F8 66CE D3DE 1E5E 0159
                    sub  1024g/D303656F 2007-05-18 [expires: 2015-05-16]      


Red Hat Enterprise Linux 6 or Compatible


                    # wget http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6 -O /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
                    pub  2048R/EC551F03 2010-07-01 Oracle OSS group (Open Source Software group) 
                          Key fingerprint = 4214 4123 FECF C55B 9086  313D 72F9 7B74 EC55 1F03

