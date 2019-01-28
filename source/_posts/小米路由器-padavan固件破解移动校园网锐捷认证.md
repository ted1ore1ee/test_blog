---
title: 小米路由器+pandavan固件破解移动校园网锐捷认证
date: 2018-5-9 15:09:45
categories: 路由器破解
permalink: mirouter-pandavan
toc: true
tags:
  - hexo
  - 小米路由器
  - 锐捷认证
  - 校园网
---
简 介
----
>**学校的移动网太不人性化了，几十块一个月还只能用网线连电脑拨号不能使用路由器，手机只能开热点回想起高三买的小米路由
>器还在家里吃灰
>立刻打电话给母上叫她给我把路由器寄过来，ps：小米路由器刷机还是挺舒服
>这次准备用恩山论坛的pandavan固件刷机，使路由器可以模拟锐捷认证
>达到使用wifi的目的**
>**所需准备：**
> - 小米路由器x1
> - 网线
> - breed和pandavan固件

<!-- more -->

前期准备
------------
初始化小米路由器，由于我第一次刷入固件失败，后面重新找了好久的老版本小米固件刷进去才支持刷机，听说现在新的固件都不能这样玩了LOL
**Breed是什么?**
breed被称作“不死”，意思所有的操作都在breed里面完成
> “Bootloader 意思为引导加载器，即为用于加载操作系统的程序。它是一大类此类功能程序的统称。现在的 BIOS、UEFI、GRUB、RedBoot、U-Boot、CFE、Breed 等都是 Bootloader。”

**Breed 拥有以下新特性：**
*实时刷机进度，进度条能准确反映刷机进度
Web 页面快速响应
最大固件备份速度，依 Flash 而定，一般能达到 1MB/s
免按复位键进入 Web 刷机模式
Telnet 功能，免 TTL 进入 Breed 命令控制台
复位键定义测试功能
固件启动失败自动进入 Web 刷机模式
可自定义位置和大小的环境变量块*
找到适合自己路由器的breed固件，地址如下```http://breed.hackpascal.net/```

**Pandavan固件下载地址**```http://opt.cn2qq.com/padavan/```、

刷机
------------
先刷入breed不死固件
电脑网络连接设置为自动获取 IP 地址
打开 CMD，运行 ping 192.168.1.1 -t
按住复位键或者WPS键再给路由通电，如果看到路由器的部分或全部LED连闪4次，或 ping 通即表明进入 Web 刷机模式
注：我的小米路由器自带了usb接口，故我拷贝固件包到U盘插上就可以直接刷
成功后如图：
![1](https://tedioreleeblog.pek3b.qingstor.com/%E5%B0%8F%E7%B1%B3%E8%B7%AF%E7%94%B1%E5%99%A8-padavan%E5%9B%BA%E4%BB%B6%E7%A0%B4%E8%A7%A3%E7%A7%BB%E5%8A%A8%E6%A0%A1%E5%9B%AD%E7%BD%91%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81/breed.png)

点击breed里面的固件更新，选择已经下载好的适合自己路由器的pandavan固件，点击确定等待刷机完成即可
![2](https://tedioreleeblog.pek3b.qingstor.com/%E5%B0%8F%E7%B1%B3%E8%B7%AF%E7%94%B1%E5%99%A8-padavan%E5%9B%BA%E4%BB%B6%E7%A0%B4%E8%A7%A3%E7%A7%BB%E5%8A%A8%E6%A0%A1%E5%9B%AD%E7%BD%91%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81/breed2.png)

之后，插上网线连接电脑
浏览器输入192.168.123.1
即可进入路由器管理界面
默认id和密码为admin
![3](https://tedioreleeblog.pek3b.qingstor.com/%E5%B0%8F%E7%B1%B3%E8%B7%AF%E7%94%B1%E5%99%A8-padavan%E5%9B%BA%E4%BB%B6%E7%A0%B4%E8%A7%A3%E7%A7%BB%E5%8A%A8%E6%A0%A1%E5%9B%AD%E7%BD%91%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81/padanvan.png)

模拟锐捷认证
------------
点击扩展功能，找到锐捷认证窗口
![4](https://tedioreleeblog.pek3b.qingstor.com/%E5%B0%8F%E7%B1%B3%E8%B7%AF%E7%94%B1%E5%99%A8-padavan%E5%9B%BA%E4%BB%B6%E7%A0%B4%E8%A7%A3%E7%A7%BB%E5%8A%A8%E6%A0%A1%E5%9B%AD%E7%BD%91%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81/ruijie.png)
*用户名输入你的学号（具体看运营商分配）
密码为宽带的密码
ip请手动输入锐捷客户端连接后的ip
网关、dns同理
心跳间隔选择2
客户端版本选择4.98
其他全部默认，点击确定*
**稍等片刻，你就可以用路由器上网了！破解成功**

pandavan其他玩法
------------
pandavan固件的功能和玩法多种多样，模拟锐捷只是其中一个而已
作为经常有翻墙需要的我，自然是可以用它自带的ss功能实现路由器翻墙上网了 
前提自己要有ssr账号LOL
![5](https://tedioreleeblog.pek3b.qingstor.com/%E5%B0%8F%E7%B1%B3%E8%B7%AF%E7%94%B1%E5%99%A8-padavan%E5%9B%BA%E4%BB%B6%E7%A0%B4%E8%A7%A3%E7%A7%BB%E5%8A%A8%E6%A0%A1%E5%9B%AD%E7%BD%91%E9%94%90%E6%8D%B7%E8%AE%A4%E8%AF%81/ssr.png)

由于我的小米路由器还有个USB接口，接上了一块硬盘以后再加上pandavan固件的功能，在寝室局域网的环境内就能实现手机平板访问、下载文件了！再也不用慢慢拷片到手机上了！
