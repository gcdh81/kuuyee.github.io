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
  
