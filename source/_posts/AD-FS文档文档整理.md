---
title: AD FS文档文档整理
toc: true
comment: true
date: 2019-01-23 16:36:56
tags:
	- microsoft
	- adfs
	- IT Support
categories: tech
permalink: 
updated:
---
前言
----
> 去年年底在实验环境下做的结合国内和国际版Azure并不算成功
> 总是卡在某一个会出问题
> 这次经过长久的实验和总结
>终于成功的实现了和两个版本的azure的单点登录
<!-- more -->

实验环境
----
**4台虚拟机，均为windows server 2019 datacenter版本**
**一个公网认证的通配符证书（*.tediorelee.cn.pfx**
**一个个人域名（tediorelee.cn**

服务器结构
----
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-43-13.png)

安装步骤
----
1.首先两台作为DC的服务器修改名字，安装ad域功能，两台服务器组成AD forest（tediorelee.cn）
2.在AD域用户和计算机里，新建一个部门，新建一个用户，两台服务器同步即可
3.在两台DC上安装AAD connect工具，分别与国内版和国际版azure同步，安装选项详见以前文档
4.登录chinacloud和global的azure，看到aad用户里同步成功即可
5.在adfs服务器上，加入域（ps：adfs服务器不要安装在DC上）修改计算机名
6.adfs上添加ad联合身份认证服务功能，导入我们的域名通配符证书，详情配置参照旧文档
7.在DC-Global上，运行aad connect，选择“更改用户登录”，用户登录方式选择“使用AD FS进行联合身份认证”，后面跟着配置即可
8.在aad connect工具中，选择“管理联合身份认证服务”-“更新AD FS ssl证书“，导入你的通配符证书并更新
9.在aad connect工具中，选择“管理联合身份认证服务”-“联合AzureAD 域”，配置你的adfs和aad域
10.在adfs服务器上，打开ADFS 管理，检查是否添加成功信赖方信任，如图：
![](https://blog2019.pek3b.qingstor.com/1-23/Untitled%20picture.png)
11.在DC-Chinacloud上，步骤同上，联合国内版aad域，但是这时会报个错，无法使用aad connect工具自动添加国内版的信赖方信任，故在ADFS管理中手动添加（具体配置参照tab《ADFS安装相关》）

截图展示
----
登录[国际版azure](https://portal.azure.com)
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-48-12.png)
在登录框中输入你的域账号
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-48-35.png)
跳转到AD FS 服务器认证
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-48-44.png)
输入你的域账号账号和密码
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-48-53.png)
成功登录国际版Azure
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-49-09.png)

登录[国内版azure](https://portal.azure.cn)![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-50-03.png)
同样输入你的域账号和密码
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-50-20.png)
跳转到AD FS服务器认证
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-50-33.png)
由于之前已经通过了单点登录的认证，这里直接进入。
成功登录国内版Azure
![](https://blog2019.pek3b.qingstor.com/1-23/Snipaste_2019-01-23_16-51-02.png)
