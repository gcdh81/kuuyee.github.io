---
layout: post
title: "Jenkins配置smpt发送邮件"
description: "Jenkins配置smpt发送邮件,使用QQ邮件服务器"
category: 
tags: [jenkins]
---
{% include JB/setup %}

### 邮件部分配置如下

**SMTP服务器** ：`smtp.qq.com`

**用户默认邮件后缀** ：`@qq.com`

**使用SMTP认证** ：选中

**用户名** ： `12345678`

**密码** ： `xxxxxxxx`

**端口** : `25`

**Reply-To Address** ： `12345678@qq.com`

**通过发送测试邮件测试配置** ：选中

**Test e-mail recipient** ： `xxxx@126.com`

点击测试配置发送邮件，结果报错如下：

    Failed to send out e-mail
    
    com.sun.mail.smtp.SMTPSendFailedException: 501 mail from address must be same as authorization user
    ;
      nested exception is:
    	com.sun.mail.smtp.SMTPSenderFailedException: 501 mail from address must be same as authorization user
    
    	at com.sun.mail.smtp.SMTPTransport.issueSendCommand(SMTPTransport.java:2057)
    	at com.sun.mail.smtp.SMTPTransport.mailFrom(SMTPTransport.java:1580)
    	at com.sun.mail.smtp.SMTPTransport.sendMessage(SMTPTransport.java:1097)
    	at javax.mail.Transport.send0(Transport.java:195)

这个问题折腾的我死去活来的，最后发现竟然不是SMTP配置不对，而是在另一项配置里的邮件地址没填，导致这个问题，Fuck Jenkins!

**解决办法**

找到Jenkins Location配置，配置如下：

**System Admin e-mail address** ： `12345678@qq.com` //这个地址必须和**Reply-To Address**一样

这样就O了！
