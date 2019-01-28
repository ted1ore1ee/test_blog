
---
title: Hexo博客添加https解析
date: 2018-9-3 20:35:21
categories: Hexo博客折腾
toc: true
permalink: hexo-https
tags:  
  - hexo
  - 博客
  - ssl证书
  - https
---
简 介
----

> **前段时间成功把hexo部署到了腾讯云服务器上，但是现在大多网站都采用了https协议访问，就空余时间申请了个免费证书上
> 传到服务器并开启了https**
> **所需准备：**
>  - 腾讯云主机+hexo
>  - ssl证书（可免费申请）
>  - winscp

<!-- more -->

申请SSL证书
------------
腾讯云可以免费申请一年的ssl证书，[地址如下](https://console.cloud.tencent.com/ssl)
![1](https://tedioreleeblog.pek3b.qingstor.com/1.png)
审核只花了我几个小时，收到短信后打开腾讯云后台，下载证书压缩包
解压以后，用winscp上传到服务器`/etc/ngix/cert`目录下
如果没有cert目录可自行手动创建`mkdir cert`


Ngix配置
------------
打开你自己的网站配置文件，以我自己为例`/etc/ngix/sites-aviliable/default`
添加如下配置：

```
server {
    listen 443;  //开启443https默认端口
    server_name tediorelee.cn;   //指定域名
    ssl on;
    root /var/www/hexo;   //指定网站路径
    index index.html index.htm;
    ssl_certificate  cert/1_xxxx.crt;    //指定上传的证书文件名
    ssl_certificate_key cert/2_xxxx.key;  //指定上传的证书秘钥名
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        root /var/www/hexo;  
        index index.html index.htm;
    }
}
```
保存退出
命令行输入`ngix -t`检查配置文件正确性
无错误后，`/etc/init.d/ngix restart`重启ngix服务
访问域名，前缀加上https即可

![2](https://tedioreleeblog.pek3b.qingstor.com/2.png)
![3](https://tedioreleeblog.pek3b.qingstor.com/3.png)
**注：我自己配置的时候，看了很多教程，发现ssl_certificate语句后面不需要接绝对路径，相对路径即可，使用了绝对路径反而检查会报错。**

Https优化
------------
添加完证书后，你会发现访问原先的http也会进到博客，我们可以继续配置ngix参数使其自动跳转到https
打开`/etc/ngix/sites-avilable/default`
删掉以前配置的80端口的配置

```
server {
    listen 80 default_server;
    return 301 https://yourdomin.cn$request_uri; //改为自己的域名
}

```
重启ngix服务`/etc/init.d/restart`
现在就可以默认跳转https了！

