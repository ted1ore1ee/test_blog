---
title: ADFS填坑-配置ADFS_1
toc: true
comment: true
date: 2019-01-14 18:25:19
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
>我们上几个步骤里已经搭好了DC和CA的环境，接下来就可以安装adfs功能了
<!-- more -->

参考资料：
----
http://blog.51cto.com/gaowenlong/1920117
http://blog.51cto.com/gshao/1788027
https://support.microsoft.com/en-us/help/3061192/step-by-step-video-set-up-adfs-for-office-365-for-single-sign-on

找到添加角色和功能，安装active directory 联合身份认证服务
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-47-22.png)

下一步默认安装，等待安装完成
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-51-33.png)

# 请求公网证书

安装完成以后，我们打开证书服务，给adfs请求一个公网SSL证书
win+r打开certlm.msc
点击个人-证书
![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-53-50.png)

空白处右键，所有任务-高级操作-创建自定义请求
选中不使用注册策略继续

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-54-21.png)

模板选择旧密钥

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-54-38.png)

点开详细信息的小箭头
点击属性

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-58-33.png)

如图所示，写上友好名称和描述

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_11-59-06.png)

使用者里，添加国家，公用名和单位信息

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-00-59.png)

私钥里点击第一个下拉箭头，去掉默认勾选
勾上最后一个（如图所示）

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-02-11.png)

密钥选项里修改大小为2048
并使其可导出

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-02-30.png)

保存请求文件

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-05-05.png)

# 在第三方证书颁发机构申请一个证书
这里我用的是[freessl](https://freessl.cn/)

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-06-32.png)

选择我有CSR
复制刚才创建的CSR进去
点击创建

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-07-10.png)

DNS验证，把提供的TXT记录值写进你的域名解析里（这里略过）

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-10-47.png)

验证完成后，下载证书并复制到服务器
在个人-证书下右键，所有任务-导入
根据向导导入刚才的证书文件

![alt](https://tedioreleeblog.pek3b.qingstor.com/adfs2/Snipaste_2018-11-22_12-14-53.png)