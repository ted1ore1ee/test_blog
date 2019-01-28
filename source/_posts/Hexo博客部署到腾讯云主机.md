---
title: Hexo博客部署到腾讯云主机
date: 2018-05-22 18:16:16
categories: Hexo博客折腾
permalink: hexo-tencentserver
toc: true
tags:
  - hexo
  - 博客
  - 腾讯云
---
简 介
----

>**前段时间将hexo博客部署到了gitpages上，毕竟免费，新手用来练练手
>后来成功以后发现了几个问题，github访问有时候很慢，不如国内的服务器稳定
>手机端打开更是慢如蜗牛，所以用学生优惠在腾讯云买了云主机和域名，
>想来部署在服务器上肯定会好很多吧**
>**所需准备：**
> - 腾讯云主机
> - 本地搭建好的hxeo博客
> - 域名

<!-- more -->

创建远程仓库
------------

先在服务器上安装git和nginx服务，百度即可。\

```
apt-get install git
apt-get install nginx
```

创建一个/var/www/hexo目录，用于ngix托管

```
sudo mkdir -p /var/www/hexo
```

创建一个仓库目录，修改目录权限和所有权

```
sudo chown -R \$USER:\$USER /var/www/hexo
```
```
sudo chmod -R 755 /var/www/hexo
```

然后修改 Nginx 的default的设置

```
sudo vim /etc/nginx/sites-available/default
```

将 root 指向/var/www/blog目录

```
server {

listen 80 default_server;

listen [::]:80 default_server ipv6only=on;

server_name xx.xx.xx \#如果你有域名此处改为你的域名 若没有直接删除这行

root /var/www/hexo;

index index.html index.htm;

 }
 ```

注：找教程的过程中，看到有些写的是修改etc下的一个配置文件，好像还要自行创建，不知对错没有尝试

保存退出并重启ngix服务

```
sudo /etc/init.d/nginx restart
```

创建git仓库目录
------------

创建一个名为blog的git仓库
```
mkdir /var/repo
cd /var/repo
git init --bare hexoBlog.git
```

创建git hook
------------

在hexoBlog下创建一个hook，可以将静态html文件推送到服务器的路径（/var/www/blog）里

在自动生成的 hooks 目录下创建一个新钩子脚本文件：

sudo vim /var/repo/hexoBlog.git/hooks/post-receive

在该文件中添加下面代码，指定 Git 的工作树(源代码)和 Git 目录(配置文件等)

```
#!/bin/bash

git --work-tree=/var/www/hexo --git-dir=/var/repo/hexoBlog.git checkout -f
```

保存退出，给该文件加权限使其变成可执行文件

```
chmod +x /var/repo/hexoBlog.git/hooks/post-receive
```

SSH 配置
--------

在服务器和本地都生成密钥
```
ssh-keygen -t rsa -C "example@gmail.com"
```
一路回车确定，生成完毕之后，打开服务器ssh目录下的authorized_keys文件，添加本地的id_rsa.pub文件中的信息进去

Git 部署
--------

配置git同步的用户名和邮箱
```
$ git config --global user.name "xxxx"
$ git config --global user.email "xxx@example.com"
```

编辑本地 blog 文件中的站点配置文件_config.yml
```
deploy:

type: git

repo: 服务器用户名\@服务器的公网 IP 地址:/var/repo/hexoBlog

branch: master
```

保存退出。

安装好后可以测试部署

```
hexo clean && hexo g && hexo d
```

注：我自己没有设置ssh免密登录，所以部署的时候会提示输入服务器登录密码，还有config.yml文件里的服务器用户名用root，用普通用户的话会提示报错，因为权限不足，我的云主机默认是无法直接root登录的，所以修改/etc/ssh/下的配置文件里的permissionrootlogin字段为yes即可直接用root登录

绑定域名
--------

登录腾讯云后台管理，实名认证后即可使用，（暂时未备案）

点击添加解析

![1](https://tedioreleeblog.pek3b.qingstor.com/Hexo%E5%8D%9A%E5%AE%A2%E9%83%A8%E7%BD%B2%E5%88%B0%E8%85%BE%E8%AE%AF%E4%BA%91%E4%B8%BB%E6%9C%BA/1.jpg)

按提示添加解析之后，手动新建一个解析，记录类型选为CNAME，记录值填写你的域名

保存即可

![2](https://tedioreleeblog.pek3b.qingstor.com/Hexo%E5%8D%9A%E5%AE%A2%E9%83%A8%E7%BD%B2%E5%88%B0%E8%85%BE%E8%AE%AF%E4%BA%91%E4%B8%BB%E6%9C%BA/2.jpg)

返回本地hexo路径，在sources下新建一个CNAME文件，编辑器打开里面填上你的域名xxxx.cn保存退出即可

重新部署一遍即可生效

```
hexo clean && hexo g && hexo d
```
