# Build-Spark-Develop-Environment
从零开始的Spark环境搭建
# 从零开始的Spark环境搭建

-----
##1.操作系统环境准备

###(1）安装VMWare
```sh
下载地址：https://www.vmware.com/cn.html
安装过程略
```

###(2)下载操作系统并安装
CentOS 6.9下载地址
```sh
http://mirrors.163.com/centos/6.9/isos/x86_64/CentOS-6.9-x86_64-LiveDVD.iso
```

##2.环境配置
Spark官方要求的JDK、Scala版本
>Spark runs on Java 7+, sh 2.6+ and R 3.1+. For the Scala API, Spark 1.5.0 uses Scala 2.10. You will need to use a compatible Scala version (2.10.x).
###(1)JDK 1.8 安装
安装Java SE Development Kit 8u151，下载地址
```sh
下载地址：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
```

在根目录下创建sparkLearning目前，后续所有相关软件都放置在该目录下，代码如下：
```sh
[root@slave01 /]# mkdir /sparkLearning
[root@slave01 /]# ls
bin   etc             lib         media  proc  selinux        sys  var
boot  hadoopLearning  lib64       mnt    root  sparkLearning  tmp
dev   home            lost+found  opt    sbin  srv            usr
```
将共享目录中的jdk安装包复制到/sparkLearning目录
```sh
[root@slave01 share]# cp /mnt/hgfs/share/jdk-8u151-linux-x64.tar.gz /sparkLearning/
[root@slave01 share]# cd /sparkLearning/
//解压
[root@slave01 sparkLearning]# tar -zxvf jdk-8u151-linux-x64.tar.gz
```
设置环境变量：
```sh
[root@slave01 sparkLearning]# vim /etc/profile
```
在文件最后添加：
```sh
export JAVA_HOME=/sparkLearning/jdk1.8.0_151
export PATH=${JAVA_HOME}/bin:$PATH
```
测试配置是否成功：
```sh
//使修改后的配置生效
[root@slave01 sparkLearning]# source /etc/profile
//环境变量是否已经设置
[root@slave01 sparkLearning]# $JAVA_HOME
bash: /sparkLearning/jdk1.8.0_151: is a directory
//测试java是否安装配置成功
[root@slave01 sparkLearning]# java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

###(2)scala-2.13.0-M1 安装

```sh
//复制文件到sparkLearning目录下
[root@slave01 sparkLearning]# cp /mnt/hgfs/share/scala-2.13.0-M1.tgz  .
//解压
[root@slave01 sparkLearning]# tar -zxvf scala-2.13.0-M1.tgz > /dev/null


[root@slave01 sparkLearning]# vim /etc/profile
```
将/etc/profile文件末尾内容修改如下：
```sh
export JAVA_HOME=/sparkLearning/jdk1.8.0_151
export SCALA_HOME=/sparkLearning/scala-2.13.0-M1
export PATH=${JAVA_HOME}/bin:${SCALA_HOME}/bin:$PATH
```
测试Scala是否安装成功
```sh
[root@slave01 sparkLearning]# source /etc/profile
[root@slave01 sparkLearning]# $SCALA_HOME
bash: /sparkLearning/scala-2.13.0-M1: is a directory
[root@slave01 sparkLearning]# scala -version
Scala code runner version 2.13.0-M1 -- Copyright 2002-2013, LAMP/EPFL
```

### (3)Hadoop 2.7.4搭建
Hadoop 2.7.4基本目录浏览
```sh
root@slave01 bin]# cp /mnt/hgfs/share/hadoop-2.7.4.tar.gz /sparkLearning/
[root@slave01 bin]# cd /sparkLearning/
[root@slave01 sparkLearning]# tar -zxvf hadoop-2.7.4.tar.gz > /dev/null
[root@slave01 sparkLearning]# cd hadoop-2.7.4
[root@slave01 hadoop-2.7.4]# ls
bin  include  libexec      NOTICE.txt  sbin
etc  lib      LICENSE.txt  README.txt  share
cd 
[root@slave01 hadoop-2.7.4]# cd etc/hadoop/
[root@slave01 hadoop]# ls
capacity-scheduler.xml      hdfs-site.xml               mapred-site.xml.template
configuration.xsl           httpfs-env.sh               slaves
container-executor.cfg      httpfs-log4j.properties     ssl-client.xml.example
core-site.xml               httpfs-signature.secret     ssl-server.xml.example
hadoop-env.cmd              httpfs-site.xml             yarn-env.cmd
hadoop-env.sh               log4j.properties            yarn-env.sh
hadoop-metrics2.properties  mapred-env.cmd              yarn-site.xml
hadoop-metrics.properties   mapred-env.sh
hadoop-policy.xml           mapred-queues.xml.template
```
将Hadoop 2.7.4添加到环境变量
使用命令：vim /etc/profile 将环境变量信息修改如下：
```sh
export JAVA_HOME=/sparkLearning/jdk1.8.0_151
export SCALA_HOME=/sparkLearning/scala-2.13.0-M1
export HADOOP_HOME=/sparkLearning/hadoop-2.7.4
export PATH=${JAVA_HOME}/bin:${SCALA_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:$PATH
```
配置Hadoop的JDK路径
使用命令：vim hadoop-env.sh 将环境变量信息修改如下，在export JAVA_HOME修改为：
```sh
export JAVA_HOME=/sparkLearning/jdk1.8.0_151
```
##3.实战
参考
```sh
http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html
```
