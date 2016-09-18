---
layout: post
title: 【openshift-4】实现简单爬虫功能+将爬取的数据生成在线API
date: 2016-09-17 15:00:00
tags: openshift
---

**前提**：请先学习爬虫框架`BeautifulSoup`和`flask中jsonify`的简单使用

1、如何简单的使用爬虫框架BeautifulSoup，可以通过慕课网上的课程学习：[Python开发简单爬虫](http://www.imooc.com/learn/563)

2、使用jsonify将数据转为`JSON`格式

### 一、实现简单的爬虫功能

#### 1、选定爬取内容，并对网页代码进行分析

我们以爬取 [python中文开发者社区](http://www.pythontab.com/) 首页热点文章为例，示意图如下：

网页左侧红框内是要爬取的内容，网页右侧是使用Chrom浏览器查看HTML代码

![img](/assets/images/2016/openshift-guide-4-1.png)

可以看到，我们要爬取的内容是在```<div class='news-hot'></div>```中，这里我们只爬取目标文章的`URL和标题`

#### 2、爬虫结构简介

学习过慕课网的视频后，应该对爬虫的结构一定了解，这里用文字归纳一下

一般爬虫的结构主要分为三大部分：`爬虫调度器`、`爬虫部分`、`数据输出`

其中`爬虫部分`又包括三个模块：`URL管理器`、`网页下载器`、`网页解析器`，示意图如下：

![img](/assets/images/2016/openshift-guide-4-2.png)

图片取自于*[CSDN博客](http://blog.csdn.net/oChangWen/article/details/51957360)*，可到作者该博客中深入了解

#### 3、添加BeautifulSoup依赖

如何添加第三方依赖库，已在[上篇博客](http://yunschou.github.io/2016/09/openshift-guide-3/)中有讲到，添加方法同Flask

![img](/assets/images/2016/openshift-guide-4-3.png)

#### 4、编写爬虫

因为这里只讲解如何操作，所以我们以最简单的方式来实现

所有的爬虫操作写在一个文件中，在保留实现思路的情况下提高易读性(步骤过于简写，并不标准，也没做异常处理，仅用于理解参考)

思路：`接收外部传入的URL`->经过爬虫处理->`返回处理后的数据`

创建爬虫文件`hellospider.py`，代码如下（截图）：

![img](/assets/images/2016/openshift-guide-4-4.png)

### 二、将爬取的数据生成在线API

#### 1、在`helloflask.py`中添加调用爬虫的路径`/hot`，代码如下（截图）：

![img](/assets/images/2016/openshift-guide-4-5.png)

#### 2、将修改后的应用部署到openshift上

此时，项目目录结构如下：

![img](/assets/images/2016/openshift-guide-4-6.png)

终于到了激动人心的时刻，将本地修改后的项目`push`到线上，快查看你自己实现的API吧

我的线上API访问结果如下：[https://python-yunschou.rhcloud.com/hot](https://python-yunschou.rhcloud.com/hot)

![img](/assets/images/2016/openshift-guide-4-7.png)











