---
title: ADFS填坑-搭建CA服务器
toc: true
comment: true
date: 2019-01-14 18:23:26
tags:
	- ADFS
	- microsoft
	- 域
categories: tech
permalink: adfs-ca
updated:
---

> 参考资料：
> http://blog.51cto.com/gaowenlong/1920117
> http://blog.51cto.com/gshao/1788027
> https://support.microsoft.com/en-us/help/3061192/step-by-step-video-set-up-adfs-for-office-365-for-single-sign-on
<!-- more -->

# 所需环境
一个ActiveDirectory域管理员账户
SSL服务器身份验证的公共可信证书
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/1.png)

# 开始安装

**准备一个本地vmware虚拟机作为CA服务器（从新安装），一个azure服务器作为联合认证服务器
vmware虚拟机中选择添加角色和功能，安装ActiveDirectory域服务配置**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/2.png)

**设置密码**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/3.png)

**提示没有DNS，直接下一步，会自动创建**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/4.png)

**域名会自动生成，直接下一步**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/5.png)

**路径可以默认也可以自定义**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/6.png)

**后面的条件检查一路默认，等待安装即可（重启）**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/7.png)

**重启完成后，点击添加要管理的其他服务器**

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/8.png)

点击查找，选中搜索到的服务器，双击添加

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/9.png)

在本地服务器中即可看到域

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs/10.png)