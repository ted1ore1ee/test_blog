---
title: 腾讯云使用ASF云挂卡
date: 2018-8-11 16:16:20
categories: Steam相关
toc: true
permalink: steam-asf
tags:
  - hexo
  - steam
  - ASF
  - 挂卡
---
简 介
----
>**
>趁着腾讯云搞学生优惠买了云服务器除了存博客之外，突然想到以前steam用asf挂卡的时候能不能用云服务器来实现云端挂卡呢？
>**
>**科普：**
> - steam等级只能通过合成徽章来获得经验升级
> - 每合成一个徽章可以得到100经验值
> - 级别越高，升一级需要的经验值对应也越高

<!-- more -->

*ASF（全称ArchiSteamFarm） 是一款由 C# 编写的挂卡工具，能同时挂载多个 Steam 账号，现在在 Linux 服务器中配置 ASF 云挂卡。它不像 Idle Master 那样，同一时间只能为一个账号挂卡，需要后台运行 Steam 客户端，需启动额外进程模拟正在游戏状态。ASF 不需要后台运行任何 Steam 客户端，不需要启动额外进程，而且能为不限数目的 Steam 账号同时挂卡。不仅如此，该软件还能在服务器和其他非桌面电脑上运行，并拥有完整支持 Mono 的特性，这能让其在 Linux、Windows 和 OS X 等平台支持 Mono 的操作系统上运行。ASF 存在的基础要归功于 SteamKit2。*

下载ASCF
----
修改服务器的host，因为现在steam服务器处于被墙的状态，所以一般是打不开的
我们可以使用AnotherSteamCommunityFix这个工具来达到不修改hosts使得服务器可以访问steam社区的功能
github链接[AnotherSteamCommunityFix](https://github.com/zyfworks/AnotherSteamCommunityFix)
下载后，用winscp工具上传到服务器任意位置，unzip解压缩后得到ascf文件
修改ascf文件的权限，使得其可以运行`chmod +x ./ascf`
注：因为涉及到修改系统文件hosts等，所以务必使用root权限

下载ASF
----
github项目地址[ArchiSteamFarm](https://github.com/JustArchi/ArchiSteamFarm/releases)
找到最新版本的压缩包，下载后同样使用winscp上传到服务器任意位置
unzip解压后，打开web网页配置页面[WebConfigGenerator](https://justarchinet.github.io/ASF-WebConfigGenerator/#/)
![1](https://tedioreleeblog.pek3b.qingstor.com/%E8%85%BE%E8%AE%AF%E4%BA%91%E4%BD%BF%E7%94%A8ASF%E4%BA%91%E6%8C%82%E5%8D%A1/1.png)
点击机器人，新建一个bot，名字随便写，login那里填写你的steam账户名，passwd填写你的密码，选中enable的√，点击下载，得到一个json文件
![2](https://tedioreleeblog.pek3b.qingstor.com/%E8%85%BE%E8%AE%AF%E4%BA%91%E4%BD%BF%E7%94%A8ASF%E4%BA%91%E6%8C%82%E5%8D%A1/2.png)
把下载得到的json文件用winscp上传到asf路径下的config文件夹

启动挂卡
----
这里要用到linux的screen命令，先启动ascf社区访问程序，命令窗口中输入```nohup sudo ./ascf &```
这样ascf程序就默认在后台一直运行，就算```ctrl+c```结束程序也不会影响了
新建一个`screen -S ASF`
进入到 ASF 所在目录`cd /ASF #`
添加可执行文件 ArchiSteamFarm 权限：`chmod +x ArchiSteamFarm.sh`
执行程序：`./ArchiSteamFarm.sh`
当前页面按 `ctrl +a +d`进入后台
恢复 screen 请终端输入：`screen -r ASF`



**最后，打开你的steam，是不是看到你的状态是正在玩游戏了！
asf挂卡速度可观，而且你要玩游戏直接挤掉就行了，不玩了他会自动连上继续帮你挂卡！
帮朋友挂他说一晚上起来挂了40来张LOL**
