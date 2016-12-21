---
layout: post
title: Xcode中Targets使用(多图，非WI-FI慎入)
date: 2016-08-23 00:00:00
tags: Xcode
---

## 目的

使用 `targets` 进行多版本管理

## 场景举例

新浪微博的第三方客户端`weico`，`普通版`是免费的，`Pro版`下载是收费的。`Pro版`对比`普通版`，APP图标和启动图等素材是不一样的，且`Pro版`在`普通版`功能上会增加一些特殊功能。

如果开发人员将这两个APP分开维护，当接到新的需求时，先在一个APP开发好，再将代码copy至另一个工程，这样的工作我们每个人应该都能做。

但是想象一下，当项目越来越大、需求修改频繁时，`两个工程间`来回copy代码也是不少的工作量，而且这样重复的劳动完全没有任何价值，对于项目进度和开发者本身都是灾难。

也许会有同学提出，使用组件化方案来解决问题。的确，组件化可以重复利用开发好的模块，但在面对两个工程不同的需求时（如不同的图片、颜色等），我们就要在模块中增加接口来应对，尤其在面对不同业务逻辑时（比如是否开放功能等），我们就不得不把一个完整的组件再细分下去，但任然避免不了在两个工程间来回折腾，这样会让我们很被动。

> 场景小结：基于同一份代码，产出不同的product，可以利用targets设置`不同的编译条件`来实现

## 实战操作

### 一、主要步骤分为：

1. 创建工程
2. 使用`Duplicate`添加targets
3. 修改`targets名称`
4. 修改info.plist文件路径，并`重命名info.plist`
5. 使用`add Files to 'TargetsDemo'...`将info.plist文件添加至Xcode
6. 使用`Choose info.plist File...`为对应的targets添加的info.plist文件
7. 为每个targets添加对应的`app图标`（和启动图）
8. 为每个targets设置`预编译宏`
9. 修改`Schemes名称`
10. 使用预编译宏为targets添加`预编译条件`
11. 不同targets运行后应该是`不同的product`

### 二、操作截图

* 创建工程

![img](/assets/images/2016/Xcode中Targets使用-1.png)

* 使用`Duplicate`添加targets

![img](/assets/images/2016/Xcode中Targets使用-2.png)

* 选择`Duplicate Only`（按需选择）

![img](/assets/images/2016/Xcode中Targets使用-3.png)

* 新增targets后如下：多了targets和对应的info.plist文件

![img](/assets/images/2016/Xcode中Targets使用-4.png)

* 修改target名称

![img](/assets/images/2016/Xcode中Targets使用-5.png)

* 修改后如下

![img](/assets/images/2016/Xcode中Targets使用-6.png)

* 修改info.plist文件路径，并重命名

![img](/assets/images/2016/Xcode中Targets使用-7.png)

![img](/assets/images/2016/Xcode中Targets使用-8.png)

* Xcode中提示找不到被重命名的info.plist文件（删除红色的文件）

![img](/assets/images/2016/Xcode中Targets使用-9.png)

* 使用`add Files to 'TargetsDemo'...`从工程文件夹下添加被重命名的info.plist文件

![img](/assets/images/2016/Xcode中Targets使用-10.png)

* 添加完成后如下

![img](/assets/images/2016/Xcode中Targets使用-11.png)

* 在Targets_A和Targets_B中使用`Choose info.plist File...`添加对应的info.plist文件

![img](/assets/images/2016/Xcode中Targets使用-12.png)

* 添加完成后如下

![img](/assets/images/2016/Xcode中Targets使用-13.png)

* 为targets添加app图标（启动图类似）

![img](/assets/images/2016/Xcode中Targets使用-14.png)

* 重命名原`appicon`名称

![img](/assets/images/2016/Xcode中Targets使用-15.png)

* 重命名后如下

![img](/assets/images/2016/Xcode中Targets使用-16.png)

* 为targets选择对应的app图标

![img](/assets/images/2016/Xcode中Targets使用-17.png)

* 为targets设置预编译宏

![img](/assets/images/2016/Xcode中Targets使用-8.png)

* 添加完成后如下

![img](/assets/images/2016/Xcode中Targets使用-9.png)

* 使用`Manage Schemes…`修改Schemes名称

![img](/assets/images/2016/Xcode中Targets使用-20.png)

![img](/assets/images/2016/Xcode中Targets使用-21.png)

* 修改后，点击`Close`

![img](/assets/images/2016/Xcode中Targets使用-22.png)

* 修改完成后如下

![img](/assets/images/2016/Xcode中Targets使用-23.png)

* 使用预编译宏为targets添加条件

![img](/assets/images/2016/Xcode中Targets使用-24.png)

* 不同targets运行后应该是不同的product

![img](/assets/images/2016/Xcode中Targets使用-25.png)

