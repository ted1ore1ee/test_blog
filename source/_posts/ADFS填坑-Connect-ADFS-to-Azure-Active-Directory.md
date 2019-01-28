---
title: ADFS填坑-Connect ADFS to Azure Active Directory
toc: true
comment: true
date: 2019-01-14 18:30:50
tags:
	- ADFS
	- microsoft
	- 域
categories: tech
permalink: adfs-aad
updated:
---
前言
----
>国内版的azure目前可以一元试用，适合个人测试用途，我这里自行注册了一个账号，将用这个账号测试我们整合aad和adfs的正确性
<!-- more -->

参考资料：
----
http://blog.51cto.com/gaowenlong/1920117
http://blog.51cto.com/gshao/1788027
https://support.microsoft.com/en-us/help/3061192/step-by-step-video-set-up-adfs-for-office-365-for-single-sign-on


打开网址[azure](https://portal.azure.cn)登录你的账号

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-40-55.png)

dashboard中找到AAD

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-42-22.png)

找到AAD Connect，下载官方的同步工具

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-42-42.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-43-53.png)

安装软件，一路下一步，选择自定义配置(因为我自己已经配好了，借用网上博客的图[原文链接](http://blog.51cto.com/gshao/1788055))

![alt](http://s3.51cto.com/wyfs02/M02/82/96/wKioL1dcPPHjjf4IAAEgEyTn6q4323.png)

输入你的azure账号密码（图上请自行替换）

![alt](http://s3.51cto.com/wyfs02/M02/82/96/wKioL1dcPPKTDf0uAAC1Gi9E3lU342.png)

连接目录这里同理

![alt](http://s3.51cto.com/wyfs02/M00/82/97/wKioL1dcPPOyCBk_AADWE0-D1F8119.png)

用户主题名称勾选userPrincipalName

![alt](http://s3.51cto.com/wyfs02/M00/82/97/wKioL1dcPPShQV5TAAFtgEShIAw250.png)

选择要同步的OU
这里我只用选择Users（视情况而定）
![alt](http://s3.51cto.com/wyfs02/M01/82/97/wKioL1dcPPWQ5WVyAAEBUCOJEzY288.png)

一路下一步
ADFS场选择现有的

![alt](http://s3.51cto.com/wyfs02/M00/82/97/wKioL1dcPPixoh1sAADk-Sh4HLY443.png)

输入域管理员凭据

![alt](http://s3.51cto.com/wyfs02/M01/82/98/wKiom1dcO-uCQvGdAADgFsFqoJA231.png)

一路下一步，等待配置完成即可
登录azure，在AAD用户里即可看到同步上来的用户了
PS：这里我自己AD用户里面新建了一个test组并新建了一个user名为mangolee，具体视情况而定

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-55-10.png)

测试登录，登录azure，用我们测试用的mangolee账号登录

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-57-39.png)

可以看到这里自动跳转到adfs认证

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-57-46.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-57-57.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-58-10.png)