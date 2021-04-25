---
title: windows问题和解决方案、相应网站工具
date: 2021-02-21 12:51:30
tags: [Tools]
---

windows遇到的系统问题和对应解决方法积累。最近遇到了Powershell异常的问题，找了微软工程师没解决，最后自己在一个论坛上找到了类似的解决了，记录一下这次排坑经历，顺便以后积累一下其他常见问题。

<!--more-->

## 问题1 Powershell找不到驱动器

### 问题起因和描述

最近装了很多编辑器，配置了很多环境变量，也删除更新了很多文件，于是再打开Powershell时便遇到了以下问题。

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/windows%E7%8E%AF%E5%A2%83%E9%97%AE%E9%A2%98//2.png)

导致Vscode也用不了

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/windows%E7%8E%AF%E5%A2%83%E9%97%AE%E9%A2%98//1.png)

于是去找了微软官网找相关人员咨询，还接受了上海的工程师的电话帮助。但是都没有解决。顺便提一下，有问题的确可以和微软官方联系，不管你的windows版本，都会给你完整的咨询服务。不仅有机器自动服务，还有客服人员，技术人员的咨询支持。

工程师说这个没有通用解决办法，于是给我提供了四条基本没用看着很可怕的方法：使用系统修复命令，更新系统，重装系统，尝试卸载一些最近装的软件。

之后又找到一篇博客是关于vscode遇到这个问题的https://blog.csdn.net/yys190418/article/details/103767720

然而这个只是解决vscode中powershell用不了的小问题，对我的问题并不适用。

### 问题解决

最后，终于在下面这个网站上找到了问题。https://superuser.com/questions/1021310/powershell-a-drive-c-does-not-exist

原来就是环境变量的锅，只要将Path中每个变量前面的 `.` 去掉即可！根据我的实践是每个都要删，不光是开头的 `.` 以后配置环境真的要小心谨慎了，这次真的吓人

即如下图所示

<img src="https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/windows%E7%8E%AF%E5%A2%83%E9%97%AE%E9%A2%98//3.png" style="zoom:50%;" />

## 网站工具

### 社区

https://superuser.com/ 	貌似是在stackoverflow下属的一个机构的网站

https://stackoverflow.com/

### 微软网站

#### 联系微软 

https://support.microsoft.com/zh-cn/contactus 

https://support.microsoft.com/zh-cn/contact/virtual-agent/?flowId=smc-contactus-entitlement&partnerId=smc&referrer=www.microsoft.com 

#### 社区

https://answers.microsoft.com/zh-hans