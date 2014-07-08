---
layout: post
title: "RHEL6 instll and config Vagrant VM"
description: ""
category: 
tags: [vm,vagrant]
---
{% include JB/setup %}

### 安装VirtualBox

首先安装依赖包

```
yum install gcc kernel-headers kernel-devel fontforge binutils glibc-headers glibc-devel
```

安装vritaulbox

```
cd /etc/yum.repo.d
wget -c "wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo"
yum install VirtualBox-4.3
```

virtrulbox在linux下需要配置

```
export KERN_DIR=/usr/src/kernels/2.6.32-431.11.2.el6.x86_64 //这个路径可能会不通，先查看一下内核版本
/etc/init.d/vboxdrv setup 
```

### 安装Vagrant

下载 ： http://www.vagrantup.com/downloads

```
yum install vagrant_1.6.3_x86_64.rpm
```

创建vagrant 用户

```
# Add vagrant user
/usr/sbin/groupadd vagrant
/usr/sbin/useradd vagrant -g vagrant -G wheel
echo "vagrant"|passwd --stdin vagrant
echo "vagrant        ALL=(ALL)       NOPASSWD: ALL" >> /etc/sudoers.d/vagrant
chmod 0440 /etc/sudoers.d/vagrant
```
### 验证安装

```
su - vagrant
vagrant -v
Vagrant 1.6.3
```
