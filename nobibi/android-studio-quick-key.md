---
title: Android Studio 常用快捷键整理
date: 2016-11-11 15:59:57
tags: 操作手册
categories: 工具手册
---

　　俗话说：工欲善事必先利器。在掌握工具的过程中，如果对其快捷键很熟练，会使我们的开发效率大大提升。

## 常用快捷键

#### 1.alt+insert

> 生成构造方法和set get

#### 2.alt+1

> 打开和关闭导航目录

#### 3.alt + shift + up / down

> 上下移动行

#### 4.ctrl+d

> 复制一行

#### 5.ctrl+y

> 删除行

#### 6.ctrl+q

> 打开帮助文档

#### 7.ctrl + o / i

> overide / implement

#### 8.ctrl + shift + space

> 类型补全 User u=new

#### 9.ctrl+alt+space

> 代码提示

#### 10.ctrl+alt+l
> 格式化代码

#### 11.F2
> 快速定位代码错误处

## 提高输入效率的快捷键

#### (1) psvm 
> 可以快速敲出main方法

#### (2) fori 和 foreach
> 可以快速敲出for循环

#### (3) .null和.nn(注意有一点，在变量名后使用，以下类似）
> 例如 if(a==null)  可以输入a.null就可以快速选出来

#### (4)const 
> 随机生成一个符合Android style的final int，再也不用自己写值了，顺便说下，Android官方不推荐使用enum<center>

#### (5) key 
> 生成一个KEY_开头的final String, Bundle KEY专用<center>

#### (6) fbc
> 快速敲出findViewById

#### (7) rgs和Sfmt
> 快速敲出 x.getString(R.string.); 和 String.format("",)

## 自定义快捷方式

如果觉得自带的快捷方式无法满足你的需求，可以试试自定义
Setting > Editor > Live Templates 右上角添加，还可以修改默认的方式，这里还有很多还没列出的快捷方式。
