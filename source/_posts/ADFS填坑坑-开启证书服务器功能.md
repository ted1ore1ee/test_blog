---
title: ADFS填坑坑-开启证书服务器功能
toc: true
comment: true
date: 2019-01-14 18:24:46
tags:
	- ADFS
	- microsoft
	- 域
categories: tech
permalink: adfs-cert
updated:
---
> 参考资料：
> http://blog.51cto.com/gaowenlong/1920117
> http://blog.51cto.com/gshao/1788027
> https://support.microsoft.com/en-us/help/3061192/step-by-step-video-set-up-adfs-for-office-365-for-single-sign-on
<!-- more -->

# 前言
在服务器上开启证书服务
点击添加角色和功能
找到active directory证书服务
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-36-27.png)

一路下一步，在选择服务和角色选项这里勾选如图所示
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-37-09.png)

下一步直到开始安装，勾选如果需要，自动重启
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-37-34.png)

重启完成以后，点击配置证书服务
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-41-32.png)

一路默认，这里勾选刚才安装的三个服务
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-42-39.png)

一路默认，选择证书并稍后为SSL分配
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-43-10.png)

win+R运行certlm.msc
即可看到证书服务已安装完成
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-45-20.png)