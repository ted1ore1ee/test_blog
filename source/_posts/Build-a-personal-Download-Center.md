---
title: Build a personal Download Center
toc: true
comment: true
date: 2019-01-28 10:41:10
tags:
	- onedrive
	- 下载
categories: tech
permalink: setup-pyone
updated:
---
前言
----
>onedrive现在可以用教育邮箱免费嫖永久1TB
>除开其他不说，onedrive的同步/下载功能还是挺不错的
>pyone是一个大佬开发的基于Python-Flask的onedrive文件本地化浏览系统@Abbey
>结合云服务器和自定义域名就可以做一个自己的下载站了！
<!-- more -->

参考文档
----
**https://wiki.pyone.me/**
**https://media.weibo.cn/article?jumpfrom=weibocom&id=2309404332460509925014&display=0&retcode=6102**

准备
----
 - 云服务器（centos）x1
 - 域名（download.xxxx.cn）x1
 - onedrive账号 x1

申请一个onedrive
----
根据微博大佬[@倒吸一口凉屁](https://media.weibo.cn/article?jumpfrom=weibocom&id=2309404332460509925014&display=0&retcode=6102)的教程
随机生成一个fake邮箱，步骤简单易懂
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_10-56-37.png)

服务器安装宝塔面板
----
在宝塔[主页](https://www.bt.cn/bbs/thread-19376-1-1.html)
用SSH连接云主机
输入`yum install -y wget && wget -O install.sh`
自动安装宝塔面板
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-07-24.png)

安装完成后，浏览器打开`http://ip:8888`登录后台，用户密码前面会自动生成，请记得及时修改
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-10-50.png)

安装Pyone
----
所有步骤参考[PyoneWiKi](https://wiki.pyone.me/pyone-an-zhuang/xia-zai-yuan-ma-yi-jian-an-zhuang/)
完成之后，打开`http://ip:34567`即可进入后台
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-16-53.png)

PS:这里我使用一键安装脚本出现了问题，有一个依赖包没有安装
导致我排错了很久，建议手动输入`pip install -r requirements.txt`一次

绑定网盘
----
获取应用机密（client_secret)和应用ID（client_id)
详情参考[PyoneWiKi](https://wiki.pyone.me/pyone-an-zhuang/bang-ding-wang-pan.html)
![](https://wiki.pyone.me/.gitbook/assets/pyone-bang-ding-liu-cheng.gif)

绑定域名
----
在宝塔面板里新建你的网站目录，设置里添加反向代理，并修改nginx配置文件
详情参考[PyoneWiKi](https://wiki.pyone.me/pyone-an-zhuang/bang-ding-yu-ming.html)
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-29-55.png)
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-30-04.png)

添加SSL证书
----
1.在腾讯云上申请一个你的下载站域名证书，或者去[FreeSSL](https://freessl.cn/)申请一个免费证书
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-33-06.png)

2.进入宝塔面板后台，打开你的网站，找到SSL-其他证书
输入你的证书和密钥，保存即可，记得勾选**强制HTTPS**
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-33-22.png)

其他
----
绑定多网盘和其他错误信息请参考[PyoneWiKi](https://wiki.pyone.me)
如果所有步骤均无误，浏览器输入`https://yourdomian`即可访问
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-44-37.png)

下载&在线播放测试
----
使用idm随意下载一个文件，可见在100M网络下，速度已经很不错了
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-50-00.png)

随便传了一集美剧，在线播放加载也挺快
![](https://blog2019.pek3b.qingstor.com/pyone/Snipaste_2019-01-28_11-53-09.png)