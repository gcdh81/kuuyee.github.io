---
layout: post
title: ".io域名申请并指向Github Pages"
description: ""
category: 
tags: [MIT]
---
{% include JB/setup %}

### .io域名申请并指向Github Pages
`.io`的价值正在与日俱增真正的全球域名新贵，为拥挤不堪的域名世界开创了一片崭新的天地，为您提供多样的选择、新的契机！对全世界所有的人来说，.io是互联网上能够用来表示信息、知识的最直接、最直观的符号。

`.io`还可被理解为[inputoutput]即“输入输出接口”之意。

去[now.cn](now.cn)查了一下，竟然一个`.io`域名一年要600多，还是淘宝吧，找到一家[神海科技](http://www.idc0310.com/)，一年255，所以果断出手。

#### DNS解析
神海科技并不直接提供DNS解析服务，所采用[https://www.dnspod.cn/](https://www.dnspod.cn/)。

主DNS： f1g1ns1.dnspod.net
从DNS： f1g1ns2.dnspod.net

然后登录[https://www.dnspod.cn/](https://www.dnspod.cn/)，支持QQ登录，登录后提示添加域名，这里以`www.example.io`为例。

### Github Pages配置
登录Github，进入某个项目也可以新建一个项目，然后进入项目配置，启用Github Pages，超级简单，这里不详细描述。

> 参见： [https://pages.github.com/](https://pages.github.com/)

然后在项目根目录下添加文件CNAME，文件内容为：`www.example.io`。

你的项目主页可以访问了：[http://username.github.io/projectname](http://username.github.io/projectname)

在[https://www.dnspod.cn/](https://www.dnspod.cn/)中更改域名`www.example.io`的IP地址，指向域名username.github.io对应的IP地址（注意不是github.com的IP地址）。

完成域名的DNS指向后，可试着用ping或dig命令确认域名www.example.io和username.github.io指向同一IP地址。

    $ dig @8.8.8.8 -t a www.example.io
    ...
    ; ANSWER SECTION:
    www.example.io.     81078   IN      A       222.232.190.78
    
    $ dig @8.8.8.8 -t a username.github.io
    ...
    ; ANSWER SECTION:
    username.github.io.      43200   IN      A       222.232.190.78

现在访问`www.example.io`应该指向http://username.github.io/projectname了！
