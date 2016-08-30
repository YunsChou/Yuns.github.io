---
layout: post
title: 【openshift-1】openshift免费服务+创建应用
date: 2016-08-30 19:00:00
tags: openshift
---

### 一、注册openshift账号：快去官网注册吧

### 二、开启你的个人域名：如果在这一步没有单独填写个人域名，在创建第一个应用的时候也会要求填写，可直接进入步骤3

### 三、创建第一个应用：这里以创建一个Python应用为例

##### 1、添加一个应用：【add application】

![img](/assets/images/2016/openshift-guide-1-1.png)

##### 2、选择你的应用类型：很多博客使用wordpress

![img](/assets/images/2016/openshift-guide-1-2.png)

##### 3、填写应用名称：我选择的应用类型为python，名称默认为python（可以修改）

说明：如果步骤2还没有填写个人域名，在Public URL处，应用名称和个人域名都是必填的（应用名：python-域名：yunschou-后缀默认：rhcloud.com）. 也就是说，可以在首次创建应用的时候填写个人域名

![img](/assets/images/2016/openshift-guide-1-3.png)

下面还有些配置，默认就好，点击创建应用【created application】进入下一步

##### 小技巧：点击创建应用后，可能需要等很久（可能是我这网络或被墙的问题），等待1分钟左右还没有进入，可以直接返回，此时在你的application中，应用已经创建成功了

### 四、访问你的域名吧：[我的域名](https://python-yunschou.rhcloud.com/)

格式为：appname-domain.rhcloud.com（domain.rhcloud.com是不可访问的）

因为你没有上传任何代码，首次进入显示为服务器默认的页面，如下：

![img](/assets/images/2016/openshift-guide-1-4.png)

##### 小技巧：在国内访问偶尔会被墙，改为https访问，基本上没问题：[添加https的域名](https://python-yunschou.rhcloud.com/)

