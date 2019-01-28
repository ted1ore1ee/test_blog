---
title: ADFS填坑-配置ADFS_2
toc: true
comment: true
date: 2019-01-14 18:25:50
tags:
	- ADFS
	- microsoft
	- 域
categories: tech
permalink: 
updated:
---
前言
----
>紧接上一步
>我们已经成功的导入了证书
>点击配置adfs服务
>注意我们是创建第一个联合服务器
<!-- more -->

> 参考资料：
> http://blog.51cto.com/gaowenlong/1920117
> http://blog.51cto.com/gshao/1788027
> https://support.microsoft.com/en-us/help/3061192/step-by-step-video-set-up-adfs-for-office-365-for-single-sign-on

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-31-30.png)

注意证书这里选择我们刚才申请的第三方证书

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-32-50.png)

这里提示密钥，我们打开powershell
键入命令
```
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))
```  

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-34-47.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-35-22.png)

选择管理员账户并输入密码

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-35-44.png)

数据库选择使用windows内部数据库即可

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-38-46.png)

先决条件检查完成。等待配置即可

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_13-39-17.png)

打开dns配置
在正向查找区域中右键新建区域

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-25-52.png)

选择主要区域
其余默认

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-26-22.png)

输入你的FQDN，保存即可

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-26-42.png)

在刚才添加的地址上面右键新建A记录
IP地址指向ADFS服务器的IP

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-30-42.png)

测试登录
浏览器输入
```
https://adfs.tediorelee.cn/adfs/ls/IdpInitiatedSignon.aspx
```  

注意这里因为非windows server 2012所以默认IdpInitiatedSignon没有开启
打开powershell输入以下命令
```
Set-AdfsProperties -EnableIdPInitiatedSignonPage:$true

Set-AdfsProperties -EnableRelayStateForIdpInitiatedSignOn:$true
```  

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-31-15.png)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs3/Snipaste_2018-11-22_23-36-26.png)