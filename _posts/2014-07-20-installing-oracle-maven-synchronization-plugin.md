---
layout: post
title: "Installing Oracle Maven Synchronization Plug-In"
description: "配置Oracle Maven 同步插件"
category: 
tags: [CI,ORACLE,Maven]
---
{% include JB/setup %}

参考文档：http://docs.oracle.com/middleware/1213/core/MAVEN/config_maven.htm#MAVEN8853

确保已经配置了Maven资源库服务器Archiva和安装了Jdev 12.1.3

**1.配置环境变量**

编辑`~/.bashrc`，加入如下变量

```bash
export JAVA_HOME=/home/oracle/jdk1.7.0_55
export ORACLE_HOME=/opt/oracle/middleware12c
export M2_HOME=$ORACLE_HOME/oracle_common/modules/org.apache.maven_3.0.5
export ANT_HOME=/ciroot/apache-ant-1.8.4
export PATH=$ANT_HOME/bin:$JAVA_HOME/bin:$M2_HOME/bin:$PATH
```

让变量生效

```bash
source ~/.bashrc
```

**2.安装同步插件**

deploy到Archiva资源库中

```bash
cd $ORACLE_HOME/oracle_common/plugins/maven/com/oracle/maven/oracle-maven-sync/12.1.3
mvn deploy:deploy-file -DpomFile=oracle-maven-sync-12.1.3.pom -Dfile=oracle-maven-sync-12.1.3.jar -Durl=http://192.168.0.117:8080/repository/internal -DrepositoryId=internal
```

也可以install到本地

```bash
cd $ORACLE_HOME/oracle_common/plugins/maven/com/oracle/maven/oracle-maven-sync/12.1.3
mvn install:install-file -DpomFile=oracle-maven-sync.12.1.3.pom -Dfile=oracle-maven-sync.12.1.3.jar
```

测试安装是否成功，执行如下命令

```
mvn help:describe -Dplugin=com.oracle.maven:oracle-maven-sync:12.1.3-0-0 -Ddetail

//最终会看到如下输出
[INFO] com.oracle.maven:oracle-maven-sync:12.1.3-0-0

Name: Oracle Maven Synchronization Plugin
Description: (no description available)
Group Id: com.oracle.maven
Artifact Id: oracle-maven-sync
Version: 12.1.3-0-0
Goal Prefix: oracle-sync

This plugin has 2 goals:

oracle-sync:help
  Description: Display help.
  Implementation: com.oracle.maven.sync.ODMHelp
  Language: java

  This mojo doesn't use any parameters.

oracle-sync:push
  Description: Install to the local repository and optionally deploy to a
    remote repository from the specified oracle home
    
    
    The plugin will use your current Maven settings to determine the path to
    the local repository and, optionally, a remote deployment repository. For
    details on how to configure Maven's repository settings, see the Maven
    settings reference: http://maven.apache.org/settings.html
    
    You can specify the parameters on the command line like this:
    -DserverId=archiva-internal -DdryRun=false -DfailOnError=false
    
    To override the localRepository target used by the plugin, you can specify
    the following option on the command-line:
    -Dmaven.local.repo=/alternate/path/to/repository
    
    To supply an alternate settings.xml for purposes of this operation, use the
    --settings option. For example:
    
     mvn --settings /alternate/path/settings.xml ... 
    ...or in your POM like this:
     <plugin>
         <groupId>com.oracle.maven</groupId>
         <artifactId>oracle-maven-sync</artifactId>
         <version>12.1.3-0-0</version>
         <configuration>
           <oracleHome>/home/mark/Oracle/Middleware</oracleHome>
           <failOnError>false</failOnError>
         </configuration>
       </plugin> 
  Implementation: com.oracle.maven.sync.ODMPushMojo
  Language: java

  Available parameters:

    dryRun (Default: false)
      User property: dryRun
      If set to 'true' this goal execution will only log push actions but will
      not actually make any changes.

    failOnError (Default: true)
      User property: failOnError
      If set to 'true' the plugin will stop and return an error immediately
      upon the first failure to deploy an artifact. Otherwise, the plugin will
      log the error and attempt to complete deployment of all other artifacts.

    localRepository (Default: ${localRepository})
      Provide an alternate local repository path to install pushed artifacts
      to.

    oracleHome
      User property: oracleHome
      Path to the Oracle home.
      Required.

    overwriteParent (Default: false)
      User property: overwriteParent
      If true, the plugin will overwrite POM artifacts with ancestry to
      oracle-common if they exist in the target repository. The default value
      of false will prevent automatic overwrite of customized POM contents. If
      any such POMs are encountered during plugin execution, an error will be
      thrown and handled according to the failOnError flag value. To carry over
      changes, save the existing POMs, run the push goal with
      overwriteParent=true and manually transfer the changes to the newly
      pushed POMs.

    pushDuplicates (Default: false)
      User property: pushDuplicates
      Push all duplicate locations. If multiple POMs with different Maven
      coordinates (GAV) are assigned to the same location path, the plugin will
      push them all to the destination repository if this flag is true.
      If the value is false, the push operation will fail. Set failOneError to
      false if you would like to skip all duplicates except the GAV in the set
      of duplicates that is encountered first.

    serverId
      User property: serverId
      This is the ID of the server (repository) in your settings.xml file -
      where you have specified the remote Maven repository and its
      authentication information. The plugin will only install to the local
      repository if this parameter is not set.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.123s
[INFO] Finished at: Thu Jul 17 18:47:49 CEST 2014
[INFO] Final Memory: 9M/28M
[INFO] ------------------------------------------------------------------------
```


