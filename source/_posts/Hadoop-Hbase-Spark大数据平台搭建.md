
---
title: Hadoop-Hbase-Spark大数据平台搭建
date: 2018-6-22 17:08:34
categories: 大数据学习
toc: true
permalink: hadoop-hbase-spark
tags:
 - hadoop
 - hbase
 - spark
 - 虚拟机
 - 大数据
---
简 介
----
>**随着这学期对于hbase和spark的学习，加上上学期学习的hadoop知识，这次专周在这一年学习的基础上，使用虚拟机或者实机>搭建一个完全分布式大数据平台，涉及hadoop平台、hbase平台还有spark平台，使用一台虚拟机充当主节点master，两台虚拟机>充当子节点slave、slave1。**
>**准备：**
> - 参考前文hadoop伪分布式搭建
> - hbase和spark安装包
> - 三个虚拟机，一个充当master节点，两个slave节点

<!-- more -->

安装hadoop
----
### 配置环境变量
在/etc/profile中加上hadoop的环境变量，保存并退出，source /etc/profile生效
![1](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/1.jpg)

### 修改配置文件`hadoop-2.6.4/etc/hadoop`
配置完以后用scp命令发给各个slave节点

```
Core-site.xml
```
![2](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/2.jpg)

```
Hdfs-site.xml
```
![3](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/3.jpg)

```
Yarn-site.xml
```
![4](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/4.jpg)

```
Hadoop-env.sh
```
![5](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/5.jpg)

```
Slaves
```
![6](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/6.jpg)

```
Master
```
![7](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/7.jpg)

```
Mapred-site.xml
```
![8](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/8.jpg)

### 测试启动
首先初始化namenode节点，使用命令`hadoop namenode -format`,然后使用`start-all.sh`启动hadoop，启动完毕以后，jps查看节点，如图，启动成功
![9](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/9.jpg)

安装hbase
----
### 环境变量配置
使用root用户进入`/etc/profile`中添加hbase的环境变量，保存并退出，然后`source /etc/profile`使其环境变量生效，如下图所示
![10](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/10.jpg)

### 修改配置文件

```
Hbase-env.sh
```
指定了hbase的环境变量。
![11](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/11.jpg)

```
Hbase-site.xml
```
![12](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/12.jpg)

```
Regionservers
```
![13](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/13.jpg)

安装zookeeper
----
### 环境变量配置
使用root用户修改`/etc/profile`文件，添加zookeeper环境变量并保存退出，`source /etc/profile`使其生效，如图所示：
![14](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/14.jpg)

### 修改配置文件
将zookeeper-3.4.3/conf目录下的zoo_sample.cfg文件拷贝一份，命名为为“zoo.cfg”
指定datadir目录，在文件末尾添加上master和slave的ip并指定端口号,如图所示：、
在datadir目录下创建myid文件，在master主机里写上0，slave里写上1，依次增加。
![15](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/15.jpg)

安装spark
----
### 环境变量配置
使用root用户修改`/etc/profile`文件，添加zookeeper环境变量并保存退出，`source /etc/profile`使其生效，如图所示：
![16](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/16.jpg)

### 修改配置文件
Cp一份spark-env.sh，打开并在里面添加配置信息，
![17](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/17.jpg)

Cp一份slaves文件，在其中添加master和slave的主机名，
![18](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/18.jpg)

将所有修改过的配置文件scp到slave节点上，至此环境搭建完毕。

测试与总结
----
### 启动zookeeper
分别在master和slave上的zookeeper安装目录下执行`./bin/zkServer.sh start`启动zookeeper
Jps查看启动情况如图：
![19](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/19.jpg)

### 启动hadoop
在master上执行`start-all.sh`，启动hadoop集群，jps查看启动情况：
Master节点：
![20](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/20.jpg)
Slave节点：
![21](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/21.jpg)

### 启动hbase
在master上执行`start-hbase.sh`，jps查看启动情况：
Master节点：
![22](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/22.jpg)
Slave节点：
![23](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/23.jpg)

### Web页面查看
hadoop
两个slave节点启动成功：
![24](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/24.jpg)

hbase
Regionserver中可以看到有两个slave子节点
![25](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/25.jpg)

spark
可以看到两个子节点（worker）
![26](https://tedioreleeblog.pek3b.qingstor.com/Hadoop-Hbase-Spark%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%B9%B3%E5%8F%B0%E6%90%AD%E5%BB%BA/26.jpg)
