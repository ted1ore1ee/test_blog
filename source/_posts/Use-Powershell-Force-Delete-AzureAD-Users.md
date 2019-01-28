---
title: Use Powershell Force Delete AzureAD Users
toc: true
comment: true
date: 2019-01-25 17:04:06
tags:
	- Microsoft
	- powershell
	- AAD
categories: tech
permalink: powershell-forcedel-aadusers
updated:
---
前言
----
>最近一直鼓捣ADFS服务器，各种虚拟机装了又删
>国际版国内版订阅也是换着花样的各种Dirsync
>导致我的国际版订阅上同步了一堆乱七八糟的用户
>这次要有其他用处了发现删除目录时提示自定义域名在使用中
>找了下文档，用powershell删除用户发现特别简单
<!-- more -->

参考文档
----
**https://docs.microsoft.com/en-us/office365/enterprise/turn-off-directory-synchronization**
**https://docs.microsoft.com/zh-cn/office365/enterprise/powershell/connect-to-office-365-powershell**
**https://blogs.technet.microsoft.com/jeffgilb/2017/03/09/deleting-azure-active-directory/**

安装MSOnline模块
----
打开powershell，输入`Install-Module MSOnline`
弹出来的提示框中输入两次`Y`
等待两分钟安装完成即可
![](https://blog2019.pek3b.qingstor.com/powershelldelaaduser/Snipaste_2019-01-25_17-15-11.png)

连接到Azure云
----
powershell中输入`Connect-MsolService`
注意：这里因为是国际版环境，所以后面不用接参数，假如是国内版或者其他版本需要后面接上参数，如下图
![](https://blog2019.pek3b.qingstor.com/powershelldelaaduser/Snipaste_2019-01-25_17-02-51.png)

在弹出来的对话框中输入你的管理员账号和密码即可
![](https://blog2019.pek3b.qingstor.com/powershelldelaaduser/Snipaste_2019-01-25_17-06-29.png)

停止DirSync同步
----
powershell中输入`Set-MsolDirSyncEnabled -EnableDirSync $false`
弹出来的提示框中输入`Y`
登录portal可看到AAD connect中同步已经显示停止
![](https://blog2019.pek3b.qingstor.com/powershelldelaaduser/Snipaste_2019-01-25_17-34-14.png)

删除AAD用户
----
powershell中输入`Get-MsolUser | Export-CSV C:\users.csv`
将AAD用户导出到本地为csv文档
![](https://blog2019.pek3b.qingstor.com/powershelldelaaduser/Snipaste_2019-01-25_17-21-47.png)

`Import-CSV C:\users.csv | Remove-MsOlUser –Force`
使用force参数强制删除用户即可

登录portal可看到除了管理员用户之外的所有AAD用户均被删除
![](https://blog2019.pek3b.qingstor.com/powershelldelaaduser/Snipaste_2019-01-25_17-22-30.png)
