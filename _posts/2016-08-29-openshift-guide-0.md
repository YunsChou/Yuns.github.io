---
layout: post
title: 【openshift-0】openshift之python生成在线API
date: 2016-08-29 20:00:00
tags: openshift
---

**【序言】**

### 一、背景

`每一个程序员都有一个成为全栈工程师的梦想！`

作为一名iOS开发工程师，并不满足于只做移动端开发，也想有朝一日灵感闪现，敲出无拘无束的`在线API`

### 二、目标

学习过一段时间Ruby/PHP，要不觉得相关资料过少，要不觉得自己有点混乱(应该是没有实际项目练过手)，现在差不多荒废了

自从接触Python后，感觉敲起来得心应手（可能也有前期学习Ruby/PHP做了铺垫），一个字`爽`！（人生苦短，我用python）

本着够用的心态，并没有深入地学习Python，从 [廖雪峰老师的博客](http://www.liaoxuefeng.com/) 下学习了Python和相关模块的基础知识（进程和线程、异步IO、实战三大章节都略过了，以后有项目机会的话，希望能够补上）。然后结合慕课视频及网络资料，奔着自己的目标，着重学习了：`WSGI接口`的编写、爬虫框架`BeautifulSoup`的简单使用、Web框架`Flask`的简单使用、Python操作`MySQL`数据库

感觉学会Flask已经能够做很多事情了，确实是一个上手简单并且非常厉害的`微框架`

**目标：通过openshift的提供的免费服务，用python开发在线可访问的API**

### 三、准备知识

必备知识：git的简单使用

语言及框架：python基础，flask、beautifulsoup简单使用

### 四、阅读指引

该系列共分为4篇，其中第一篇和第二篇是对openshift的操作

第一篇：[【openshift-1】openshift免费服务+创建应用](http://yunschou.github.io/2016/08/openshift-guide-1/)

第二篇：[【openshift-2】为Python应用添加MySQL+phpMyAdmin](http://yunschou.github.io/2016/08/openshift-guide-2/)

第三篇：[【openshift-3】添加Flask等第三方库+部署自己的应用](http://yunschou.github.io/2016/09/openshift-guide-3/)

第四篇：[【openshift-4】实现简单爬虫功能+将爬取的数据生成在线API](http://yunschou.github.io/2016/09/openshift-guide-4/)

第二篇`【openshift-2】为Python应用添加MySQL+phpMyAdmin`与当前所实现功能并无直接关联，为扩展练习（也可跳过阅读，并不影响）

### 五、Code链接

博客中的代码：[python-openshift-online-API](https://github.com/YunsChou/PythonCode/tree/master/python-openshift-online-API)

