---
title: 使用arukas.io免费搭建SS科学上网服务
date: 2016-10-19 12:00:03
tags: 免费翻墙
categories: 翻墙工具
---

arukas.io是日本樱花公司推出的Docker服务，开发者可以购买此服务，将代码部署在其中运行。国内也有同类的产品，如网易的蜂巢、灵雀云，之前好多人用它们做运行免流服务。

arukas.io在今年8月就可以免费使用了，曾因国内用户疯狂注册而一度停止注册。注册时会显示：arukas.io在测试阶段将免费使用，每个用户可免费创建10个容器。据业内网友表示，此服务将持续到明年3月，也就是说我们可以免费使用大概半年。而且，由于arukas.io的线路是日本直连大陆的，速度没的说。

153.125.232.204这个是我之前测试时获得的IP，大陆PING平均值小于100ms。

下面简单介绍下操作步骤
---
### 1.注册
注册地址：[aruks.io](https://app.arukas.io/sign_up/)
### 2.进入控制面板，点Create创建一个应用
  
  依次输入
> * APP NAME：随便你起一个
> * Image：lowid/ss-with-net-speeder:latest
> * Instances：1（创建一个容器）
> * Memory：512M（免费当然越多越好）
> * Endpoint：不填写
> * Port：1111（端口）
> * ENV：不勾选
> * CMD：ssserver -p 1111 -k 123456 -m aes-256-cfb(123456为SS密码，自行修改)

如图所示：
![演示图](http://7xn5mu.com1.z0.glb.clouddn.com/ss3.png)

其中 31989就是ss上要填的端口 密码是123456 ip：153.125.234.172
### 3.启动服务，显示Running
然后，复制信息粘贴到SS客户端，

IP，端口，密码根据自己的情况查找，详见下图

观看youtube视频速度还是不错的。

SS客户端： 链接：[http://pan.baidu.com/s/1c2Jtg5M](http://pan.baidu.com/s/1c2Jtg5M) 密码：rd99