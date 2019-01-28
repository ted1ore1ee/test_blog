---
title: ADFS填坑-整合国内版和国际版Azure
toc: true
comment: true
date: 2019-01-14 18:40:50
tags:
	- ADFS
	- microsoft
	- 域
categories: tech
permalink: adfs-azure
updated:
---
前言
----
> 上一篇文章中我们做到了使用AAD Connect把本地用户和Azure连接起来
> 但是众所周知国内版和国际版的Azure是单独隔离的
> 所以这次我们将使用另一个单独的服务器，加入到同一个域，组成林
>一个林下的两个AD服务器会相互同步，然后两个服务器单独与国内版和国际版的Azure同步，以做到整合国内版和国际版的目的
<!-- more -->

准备工作
----

新建一个用来作为同步国际版AD的服务器，我们取名叫AD2
将AD2的dns指向域控的ip，把AD2加入域（tediorelee.cn）

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-29-26.png)

AD2上安装Active Directory角色和功能
配置步骤这里省略

登录国际版Azure
使用自己的账号并创建一个新的AAD目录
这里我起名为azure_test
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-33-10.png)

新建一个用户mysql@lovezq4ever.onmicrosoft.com(注意域名规范)
并授予该用户全局管理员权限

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-33-34.png)

然后下载AAD Connect工具，安装完成后准备开始配置
同理选择自定义配置，其余默认下一步
在连接到Azure AD这里输入刚才创建的全局管理员账号以及密码（这里是<u>mysql@lovezq4ever.onmicrosoft.com</u>）

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-36-54.png)

连接目录这里点击添加目录，输入你的DC账户及密码
选中下拉菜单中的现有林

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-40-37.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-40-44.png)

在域和OU筛选这里点击下拉箭头，只勾选你想要同步的目录即可（注意这里要与国内版的保持一致否则会出问题）

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-42-50.png)

其余默认设置即可，等待配置完成

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-44-42.png)

可以看到已经配置完成

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-46-21.png)

验证是否整合成功
----

登录[国际版azure](portal.azure.com)
在用户中我们可以看到已经同步过来的ADFS服务器中的用户mangolee

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-48-43.png)

**验证用本地AD用户mangolee登录Azure是否会跳转到ADFS服务器：**

登录[Azure](portal.azure.com)
输入用户名<u>mangolee@tediorelee.cn</u>和密码
可以看到这里显示跳转到组织登录页（即ADFS服务器）

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-53-34.png)

输入域中的用户名和账户即可登录

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-53-40.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs4/Snipaste_2018-11-23_15-54-09.png)