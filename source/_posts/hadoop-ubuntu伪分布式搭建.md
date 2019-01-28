---
title: hadoop+ubuntu伪分布式搭建
date: 2018-5-30 16:20:01
copyright: true
categories: 大数据学习
permalink: hadoop-ubuntu
toc: true
tags:
 - hexo
 - ubuntu
 - hadoop
---
简 介
----
>**大数据学习离不开hadoop集成环境，正好老师上课也给我们了搭建伪分布式hadoop环境的任务，正好可以一起完成**
>**所需准备：**
> - jdk1.8的安装包
> - hadoop2.6.4的安装包
> - 安装了ubuntu server14.04的虚拟机一个
> - 远程ssh工具等
> - winscp工具用于上传需要用到的文件

<!-- more -->

前期准备
----
ubuntu server需要安装好ssh客户端，在`/etc/ssh/sshd_config`里修改permissionrootlogin参数为yes
这样就可以使用root账户直接登录了
将准备好的jdk和hadoop安装包使用winscp上传到ubuntu里

安装JDK
----
解压缩jdk安装包，使用命令`tar -zxvf jdk1.8.0_121.tar.gz`
我解压到的是`/home/hadoop/jdk1.8.0_121`路径
此处图片
添加jdk环境变量
打开文件`vim /etc/profile`在末尾加上
```
export JAVA_HOME="/home/hadoop/jdk1.8.0_121"
export PATH=$JAVA_HOME/bin:$PATH
```
保存并退出，`source /etc/profile`使其生效
这时输入`java -version`即可查看到版本信息表示jdk安装成功

安装hadoop
----
解压缩hadoop`tar -zxvf hadoop-2.6.4.tar.gz`
创建存放hadoop文件的目录`sudo mkdir /opt/modules`
将hadoop文件夹的所有者指定为hadoop用户`sudo chown -R hadoop:hadoop /opt/modules`
配置Hadoop环境变量`vim /etc/profile`
末尾加上
```
export HADOOP_HOME="/home/hadoop/hadoop.2.6.4"
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```
保存并退出，`source /etc/profile`使其生效
**配置 *hadoop-env.sh*、*mapred-env.sh*、*yarn-env.sh*文件的*JAVA_HOME*参数**
编辑
```
vim  ${HADOOP_HOME}/etc/hadoop/hadoop-env.sh
vim  ${HADOOP_HOME}/etc/hadoop/mapred-env.sh
vim  ${HADOOP_HOME}/etc/hadoop/yarn-env.sh
```
创建临时目录：

```
sudo mkdir -p /opt/data/tmp
```
将临时目录的所有者修改为hadoop

```
sudo chown –R hadoop:hadoop /opt/data/tmp
```
*注：默认的hadoop.tmp.dir是/tmp/hadoop-${user.name},此时有个问题就是NameNode会将HDFS的元数据存储在这个/tmp目录下，如果操作系统重启了，系统会清空/tmp目录下的东西，导致NameNode元数据丢失，是个非常严重的问题，所有我们应该修改这个路径。*

 **配置 core-site.xml**
```
<configuration>
<property>
<name>hadoop.tmp.dir</name>
<value>/opt/data/tmp</value>?????????
</property>
<property>
<name>fs.defaultFS</name>????????????
<value>hdfs://hadoopm:9000</value>
</property>
</configuration>
```
![3](https://tedioreleeblog.pek3b.qingstor.com/hadoop-ubuntu%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E6%90%AD%E5%BB%BA/3.png)
**配置、格式化、启动HDFS**

```
vim ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
```
添加片段

```
<property>
       <name>dfs.replication</name>
       <value>1</value>
</property>
```
![4](https://tedioreleeblog.pek3b.qingstor.com/hadoop-ubuntu%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E6%90%AD%E5%BB%BA/4.png)
dfs.replication配置是HDFS存储时的备份数量，伪分布式环境只有一个节点，所以这里设置为1。
格式化HDFS

![5](https://tedioreleeblog.pek3b.qingstor.com/hadoop-ubuntu%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E6%90%AD%E5%BB%BA/5.png)
```
hdfs namenode –format
```
格式化后，查看core-site.xml里hadoop.tmp.dir（本例是/opt/data目录）指定的目录下是否有了dfs目录，如果有，说明格式化成功。

**启动NameNode**

```
/sbin/hadoop-daemon.sh start namenode
```
**启动DataNode**

```
/sbin/hadoop-daemon.sh start datanode
```
**启动SecondaryNameNode**

```
sbin/hadoop-daemon.sh start secondarynamenode
```
 **JPS命令查看是否已经启动成功，有结果就是启动成功了**
![6](https://tedioreleeblog.pek3b.qingstor.com/hadoop-ubuntu%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E6%90%AD%E5%BB%BA/6.png)
 
 **配置、启动YARN**
 默认没有mapred-site.xml文件，但是有个mapred-site.xml.template配置模板文件。复制模板生成mapred-site.xml
 

```
cp etc/hadoop/mapred-site.xml.template etc/hadoop/mapred-site.xml
```
添加配置如下：

```
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
```
配置yarn-site.xml

```

<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
<property>
<name>yarn.resourcemanager.hostname</name>
<value>bigdata-senior01.chybinmy.com</value>
 </property>
```
 **启动Resourcemanager**
 
```
sbin/yarn-daemon.sh start resourcemanager
```
**启动nodemanager**

```
sbin/yarn-daemon.sh start nodemanager
```
可以看到ResourceManager、NodeManager已经启动成功了。
![7](https://tedioreleeblog.pek3b.qingstor.com/hadoop-ubuntu%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E6%90%AD%E5%BB%BA/7.png)


 

