---
layout: post
title: "VNC远程登录配置"
description: "VNC远程登录配置"
category: 
tags: [Linux,VNC]
---
{% include JB/setup %}

CentOS最小化安装是没有图形页面的，下面列出要使用VNC远程登录的基本配置。

**1.配置源**
首先设置下163的源，让下载速度快些，编辑`/etc/yum.repos.d/CentOS-Base.repo`文件，加入如下内容：


  [base]
  name=CentOS-6- Base - 163.com
  baseurl=http://mirrors.163.com/centos/6/os/$basearch/
  gpgcheck=1
  gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6
   
  [updates]
  name=CentOS-6- Updates - 163.com
  baseurl=http://mirrors.163.com/centos/6/updates/$basearch/
  gpgcheck=1
  gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6
   
  [extras]
  name=CentOS-6- Extras - 163.com
  baseurl=http://mirrors.163.com/centos/6/extras/$basearch/
  gpgcheck=1
  gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6
   
  [centosplus]
  name=CentOS-6 - Plus - 163.com
  baseurl=http://mirrors.163.com/centos/6/centosplus/$basearch/
  gpgcheck=1
  enabled=0
  gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6
   
  [contrib]
  name=CentOS-6 - Contrib - 163.com
  baseurl=http://mirrors.163.com/centos/6/contrib/$basearch/
  gpgcheck=1
  enabled=0
  gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6


**2.开始安装桌面**


  yum groupinstall "Desktop"


**3.安装VNC**


  yum install vnc
  yum install tigervnc-server


**4.启动VNC Server**


  vncserver
  
  //设定一个vnc登录密码
  You will require a password to access your desktops.
  
  Password:
  Verify:
  xauth:  creating new authority file /root/.Xauthority
  
  New 'adfdev.zngh.com:1 (root)' desktop is adfdev.zngh.com:1
  
  Creating default startup script /root/.vnc/xstartup
  Starting applications specified in /root/.vnc/xstartup
  Log file is /root/.vnc/adfdev.zngh.com:1.log


**5.下载VNC客户端**

[VNC-Viewer-5.2.0-Windows-64bit.zip](http://www.realvnc.com/download/binary/1542/)

可以用VNC-Viewer登录了！
