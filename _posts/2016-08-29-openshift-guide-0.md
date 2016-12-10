---
layout: post
title: 【openshift-序】使用Python在openshift上生成在线API
date: 2016-08-29 20:00:00
tags: openshift
---

**【序言】**

### 一、背景

> 每一个程序员都有一个成为全栈工程师的梦想！

作为一名移动端开发工程师，有时并不满足只做前端开发，也想一探后端究竟。也会时常想着，有朝一日灵感闪现，自己写前端和后端吧，实现内心那些*伟大的idea*。前端与后段交互需要**接口**，那么我们就要有能力为自己提供 **`在线API`**

### 二、目标

#### **目标：通过openshift提供的免费服务，使用Python开发在线可访问的API**

前期探索：工作之外学习过一段时间Ruby和PHP，要么觉得相关资料较少，要么觉得自己有点不适应(没有实际项目练过手)，学而不能时习之，现在差不多荒废了。自从接触Python后，感觉敲起来得心应手（可能与前期学习过Ruby/PHP也有一定关系），一个字：`爽`！（人生苦短，我用Python）

学习路线：本着实战够用的心态，先从 [廖雪峰老师的博客](http://www.liaoxuefeng.com/) 下学习了Python和相关模块的基础知识（进程和线程、异步IO、实战三大章节都略过了，以后有机会的话，希望能够补上），然后结合慕课视频及网络资料，着重学习了：`WSGI接口`的编写、爬虫框架`BeautifulSoup`的简单使用、Web框架`Flask`的简单使用、Python操作`MySQL`数据库

惊喜：感觉学会Flask已经能够做很多事情了，确实是一个上手简单并且非常强大的`微框架`



### 三、准备知识

必备知识：git的简单使用

语言及框架：Python基础，Flask和Beautifulsoup框架的简单使用

### 四、阅读指引

该系列共分为4篇，其中第一篇和第二篇是对openshift的操作

第一篇：[【openshift-1】openshift免费服务+创建应用](http://yunschou.github.io/2016/08/openshift-guide-1/)

第二篇：[【openshift-2】为Python应用添加MySQL+phpMyAdmin](http://yunschou.github.io/2016/08/openshift-guide-2/)

第三篇：[【openshift-3】添加Flask等第三方库+部署自己的应用](http://yunschou.github.io/2016/09/openshift-guide-3/)

第四篇：[【openshift-4】实现简单爬虫功能+将爬取的数据生成在线API](http://yunschou.github.io/2016/09/openshift-guide-4/)

第二篇`【openshift-2】为Python应用添加MySQL+phpMyAdmin`与当前所实现功能并无直接关联，为扩展练习（也可跳过阅读，并不影响）

### 五、Code链接

博客中的代码：[python-openshift-online-API](https://github.com/YunsChou/PythonCode/tree/master/python-openshift-online-API)

