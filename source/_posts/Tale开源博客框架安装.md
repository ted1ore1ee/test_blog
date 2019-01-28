---
title: Tale开源博客框架安装
toc: true
comment: true
date: 2019-01-14 18:18:54
tags: 
	- blog
	- tale
categories: tech
permalink: tale
updated:
---
简介
----
> 这个人呐总是不安于现状的，所以我之前尝试了hexo博客之后，又发现了一款由国人开发的开源博客名叫tale，于是就迫不及待的开始了安装、尝鲜。
----
<!-- more -->

# tale特性
  - 设计简洁，界面美观
  - Markdown 文章发布
  - 自定义文章链接
  - 支持多主题
  - 支持插件扩展
  - 支持 Emoji 表情
  - 支持网易云音乐播放
  - 支持附件和数据库备份
  - 部署简单，不依赖 Tomcat
  - 无需数据库，内嵌 Sqlite

# 环境准备
以我自己的腾讯云为例，ubuntu16.04的操作系统
先安装JDK
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt update
sudo apt install -y oracle-java8-installer
sudo apt install -y oracle-java8-set-default
java -version
```

后安装nginx

```
sudo apt-get install nginx -y
```

# 安装 Tale 博客

修改Nginx配置
`vim /etc/nginx/sites-aviliable/defalut`

修改为：

```
server {
  	listen 80;
	
  	location / {
	try_files $uri $uri/ =404;
  }
}
server{
	listen 443;
    	server_name www.tediorelee.cn,132.232.20.202;     # 填写绑定证书的域名
    	ssl on;
    	ssl_certificate cert/1_www.tediorelee.cn_bundle.crt;
    	ssl_certificate_key cert/2_www.tediorelee.cn.key;
    	ssl_session_timeout 5m;
    	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;    # 按照这个协议配置
    	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;     # 按照这个套件配置
    	ssl_prefer_server_ciphers on;
    location / {
        proxy_pass http://127.0.0.1:9000;
    	proxy_read_timeout 300;
    	proxy_connect_timeout 300;
    	proxy_redirect     off;

    	proxy_set_header   X-Forwarded-Proto $scheme;
    	proxy_set_header   Host              $http_host;
    	proxy_set_header   X-Real-IP         $remote_addr;
    }
}
```

**注：因为我自己的域名已经申请了免费的证书所以我就直接添加到了`/etc/nginx/cert`目录下并在配置文件中写明了https的配置信息**

启动tale
```
wget -qO- git.io/fxsWx | bash
cd tale
sh tool start
```

访问ip:9000即可开始安装

脚本说明：
  - start: 启动 tale
  - stop: 停止 tale
  - restart: 重启 tale
  - status: 查看 tale 运行状态
  - log: 查看 tale 运行日志
  - upgrade: 升级 tale，会自动备份

# 效果图
![](https://tedioreleeblog.pek3b.qingstor.com/%E6%9D%82%E7%89%A9/TIM%E6%88%AA%E5%9B%BE20181029161421.png)

