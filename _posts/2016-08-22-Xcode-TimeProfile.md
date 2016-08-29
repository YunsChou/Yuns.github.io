---
layout: post
title: Xcode中TimeProfile使用
date: 2016-08-22 21:30:46
tags: Xcode
---

### 一、对Xcode进行设置

1、如果想要在TimeProfile中直观的查看方法耗时，需要对Xcode进行设置

在Xcode->Build Setting->Debug Information Format中设置选项为：**DWARF with DSYM File**

![img](/assets/images/2016/Xcode中TimeProfile使用-1.png))

2、不为该选项的话，在TimeProfile中就只能看到一堆线程

### 二、对TimeProfile进行设置

1、在TimeProfile的Call Tree中，右侧面板有三个检查器：record setting（记录设置）、display setting（展示设置）、还有extends detail（扩展详情）

我们选择display setting，并在该选择器中勾选**Separate by Thread和Hide System Libraries**（两个最基本的选项）

![img](/assets/images/2016/Xcode中TimeProfile使用-2.png)

3、这样就可以逐级查看每个方法的耗时了

![img](/assets/images/2016/Xcode中TimeProfile使用-3.png)