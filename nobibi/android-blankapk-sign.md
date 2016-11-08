---
title: Android 如何给空包签名
date: 2016-11-03 13:04:45
tags: Android
categories: 上架相关
---
　　有时候我们在应用市场上线app，该应用市场可能会自动从其他应用市场拉取应用，这个时候，就会需要一个认领操作。认领时需要下载空包，然后给空包签名，再上传上去审核。

　　本文将给大家介绍android给空白包签名的相关知识，感兴趣的朋友一起学习吧！

## 关于签名的一些重要点：

> * 所有的应用必须签名(android 有默认签名)。
> * 测试和调试应用，构建工具用指定的调试密钥(android sdk 构建工具创建的)签名你的应用。
> * 在发布给终端用户之前要用合适的密钥签名应用，不能用调试密钥签名将要发布的应用。
> * 可以用自己签名的证书签名自己的应用。
> * Android 系统仅仅会在应用安装的时候检查证书的有效期。如果应用在安装之后过期，那么应用还会正常运行。
> * 我们可以用标准的工具-Keytool 和 Jarsigner - 生成密钥和签名应用。
> * 在完成签名之后，发布之前，需要使用zipalign 工具优化最终的apk 包。

> 注：Android 系统不能安装和运行没有正确签名的包。

## 如何签名：
```java
jarsgner-verbose-keystore[keystorePath]-singnedjar [apkOut] [apkln] [alias]
```
> jarsgner命令格式：-verbose输出详细信息-keystore密钥库位置-alias demo.keystore 别名 demo.keystore
-keyalg RSA 使用RSA算法对签名加密
-validity 40000 有效期限4000天
-keystore demo.keystore
D:\>jarsigner -verbose -keystore demo.keystore -signedjar demo_signed.apk demo.apk demo.keystore
/* 说明：-verbose 输出签名的详细信息 */
例如
D:\>jarsigner -verbose -keystore demo.keystore -signedjar demo_signed.apk demo.apk demo.keystore
android给未签名的apk签名命令。
### 1.准备文件
1、tap_unsign.apk（未签名的apk） 
2、shanhy.keystore（签名证书文件）
### 2.命令语法：
jarsigner -verbose -keystore [keystorePath] -signedjar [apkOut] [apkIn] [alias]
### 3.例 子：
```java
jarsigner -verbose -keystore G:\shanhy.keystore -signedjar G:\signed.apk G:\tap_unsign.apk shanhy
```
> 
[keystorePath] 后面是绝对路径G:\shanhy.keystore
[apkOut] 生成签名的apk的位置
[apkIn] 参数代表在腾讯应用中心下载的未签名apk，默认名称为tap_unsign.apk
[alias] 是G:\shanhy.keystore 的别名
jarsigner这个exe在C:\Program Files\Java\jdk1.7.0_10\bin文件夹下。所以要用cmd进入这个文件夹
然后使用上面的命令就可以了。

### 4.注意事项：

> * 1.如果签名文件末尾没有.keystore，使用的时候也就不要加了。
> * 2.注意文件的路径是绝对路径（带盘符的）。