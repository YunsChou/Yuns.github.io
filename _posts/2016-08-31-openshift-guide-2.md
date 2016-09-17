---
layout: post
title: 【openshift-2】为Python应用添加MySQL+phpMyAdmin
date: 2016-08-31 20:00:00
tags: openshift
---

### 一、添加MySQL

应用创建好后，默认样式如下图：

![img](/assets/images/2016/openshift-guide-2-1.png)

database有三种供选择，因为我使用的是MySQL数据库，所以点击‘Add MySQL5.5’添加

### 二、添加phpMyAdmin

添加MySQL数据库后，应用样式如下图：

![img](/assets/images/2016/openshift-guide-2-2.png)

点击红框处，添加phpMyAdmin工具

### 三、使用phpMyAdmin对MySQL进行管理

添加phpMyAdmin后，应用样式如下图：

![img](/assets/images/2016/openshift-guide-2-3.png)

点击phpMyAdmin后的按钮，跳转到phpMyAdmin界面

登录帐号和密码在MySQL后面，只不过密码不是‘show’，而是点击'show'按钮之后会出现