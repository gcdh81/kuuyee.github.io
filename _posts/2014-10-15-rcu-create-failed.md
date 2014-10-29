---
layout: post
title: "ORACLE RCU 安装问题汇总"
description: "ORACLE RCU 安装问题汇总"
category: 
tags: [ORACLE,RCU]
---
{% include JB/setup %}

### ORA-01450: maximum key length (6398) exceeded
报错内容如下：

    ORA-01450: maximum key length (6398) exceeded
    File:/install/rcu_11.1.1.8/rcuHome/rcu/integration/mds/sql/cremdsinds.sql
    Statement:create index MDS_COMPONENTS_N1
    on MDS_COMPONENTS ( COMP_LOCALNAME, COMP_CONTENTID, COMP_PARTITION_ID, ENTERPRISE_ID, COMP_NSID, COMP_SEQ, COMP_ID )
    
    
    ORA-01450: maximum key length (6398) exceeded
    File:/install/rcu_11.1.1.8/rcuHome/rcu/integration//opss/scripts/opss_tables.sql
    Statement:CREATE UNIQUE INDEX IDX_JPS_RDN_PDN ON JPS_DN (RDN, PARENTDN)
    
    ORA-01450: maximum key length (6398) exceeded
    File:/install/rcu_11.1.1.8/rcuHome/rcu/integration/webcenter/sql/oracle/activitystreaming-repository-create.sql
    Statement:CREATE TABLE WC_NAVIGATION_ACTIVITY
    (
       ID                 VARCHAR2(4000) NOT NULL,
       CONTEXT            VARCHAR2(2000),
       USER_ID            VARCHAR2(2000) NOT NULL,
       PAGE_URL           VARCHAR2(4000) NOT NULL,
       LABEL              VARCHAR2(4000),
       ICON_URI           VARCHAR2(4000),
       LAST_ACCESSED_TIME TIMESTAMP,
       EXT_ATTR1          VARCHAR2(4000),
       EXT_ATTR2          NUMBER,
       CONSTRAINT WC_ACTIVITY_PAGE_ACCESS_PK PRIMARY KEY
       (
         ID
       ) ENABLE,
       CONSTRAINT WC_ACTIVITY_PAGE_ACCESS_UN UNIQUE
       (
         USER_ID, PAGE_URL
       )
    )
  

**解决办法：**
调整数据库系统参数并重启：

    alter system set nls_length_semantics=byte;
  


### ORA-01882: timezone region not found
报错内容如下：

    ==> testsoa: ORA-01882: timezone region not found
    ==> testsoa: 
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIoer.processError(T4CTTIoer.java:450)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIoer.processError(T4CTTIoer.java:392)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIoer.processError(T4CTTIoer.java:385)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIfun.processError(T4CTTIfun.java:1018)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIoauthenticate.processError(T4CTTIoauthenticate.java:501)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIfun.receive(T4CTTIfun.java:522)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIfun.doRPC(T4CTTIfun.java:257)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIoauthenticate.doOAUTH(T4CTTIoauthenticate.java:437)
    ==> testsoa:    at oracle.jdbc.driver.T4CTTIoauthenticate.doOAUTH(T4CTTIoauthenticate.java:954)
    ==> testsoa:    at oracle.jdbc.driver.T4CConnection.logon(T4CConnection.java:639)
    ==> testsoa:    at oracle.jdbc.driver.PhysicalConnection.connect(PhysicalConnection.java:666)
    ==> testsoa:    at oracle.jdbc.driver.T4CDriverExtension.getConnection(T4CDriverExtension.java:32)
    ==> testsoa:    at oracle.jdbc.driver.OracleDriver.connect(OracleDriver.java:566)
    ==> testsoa:    at java.sql.DriverManager.getConnection(DriverManager.java:571)
    ==> testsoa:    at java.sql.DriverManager.getConnection(DriverManager.java:215)
    ==> testsoa:    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    ==> testsoa:    at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
    ==> testsoa:    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    ==> testsoa:    at java.lang.reflect.Method.invoke(Method.java:606)
    ==> testsoa: 
    ==> testsoa: java.sql.SQLException: java.sql.SQLException: ORA-00604: error occurred at recursive SQL level 1

**解决办法1：**
设置本地环境变量`export TZ=GMT;`

**解决办法2：**
要想从根本上解决，还是需要把环境变量搞定，执行以下操作


    1.
    cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
    2.
    vi /etc/sysconfig/clock
    ZONE=”Asia/Shanghai”
    UTC=false
    ARC=false
    3.
    date -s 10/27/2014
    date -s 09:51:00
    4.
    reboot


<hr class="bs-docs-separator">
