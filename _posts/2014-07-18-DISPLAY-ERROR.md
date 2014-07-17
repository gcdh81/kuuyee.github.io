---
layout: post
title: "DISPLAY variable set incorrectly: :1.0(解决办法)"
description: "DISPLAY variable set incorrectly: :1.0"
category: 
tags: [Linux]
---
{% include JB/setup %}

Linux下安装ORACLE JDev报错：

```
>>> Ignoring failure(s) of required prerequisite checks.  Continuing...
Preparing to launch the Oracle Universal Installer from /tmp/OraInstall2014-07-17_05-19-30PM
Log: /tmp/OraInstall2014-07-17_05-19-30PM/install2014-07-17_05-19-30PM.log
No protocol specified
X-Server access is denied on host
[Fatal Error] DISPLAY variable set incorrectly: :1.0
[Resolution] Verify that your DISPLAY environment variable is set correctly, 
and that there is an X11 server on the system. If you are 
running the Oracle Installer as a different user or on a different host, 
you may need to use the xhost command to ensure that host/user 
has permission to write to your display.
```

**解决办法**

```bash
su - root
xhost + 
access control disabled, clients can connect from any host
```

`xhost +`这个命令将允许别的用户启动的图形程序将图形显示在当前屏幕上。一般与DISPLAY共同使用。

