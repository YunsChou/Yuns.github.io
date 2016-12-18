---
layout: post
title: 【组件化-1】CTMediator源码解读
date: 2016-12-18 00:00:00
tags: 组件化
---

### 目的

1、学习组件化的使用

2、结合CTMediator作者博客进行源码解读

### 说明

1、部分关键描述会直接引用作者原话（可能会附注自己的理解），以免有失偏颇

2、学习讲究循序渐进，如有理解不完整，待以后有更多体会再来补充

### 设计思路

CTMediator是一个单例，主要是基于Mediator模式和Target-Action模式，中间采用了runtime来完成调用

### 源码分析

> 提供的API：分别为`远程app调用入口`、`本地组件调用入口`、`释放某个target缓存`

```
// 远程App调用入口
- (id)performActionWithUrl:(NSURL *)url completion:(void(^)(NSDictionary *info))completion;
// 本地组件调用入口
- (id)performTarget:(NSString *)targetName action:(NSString *)actionName params:(NSDictionary *)params shouldCacheTarget:(BOOL)shouldCacheTarget;
// 释放某个target缓存
- (void)releaseCachedTargetWithTargetName:(NSString *)targetName;
```

> 其中，以**`本地组件调用入口`**最为核心，`远程app调用入口`是基于其上实现的，也就是`本地组件调用入口`是为`远程app调用入口`服务的

#### 一、本地组件调用入口`performTarget: action: params: shouldCacheTarget:`实现分析

1、传入参数分别为：`对象名称`、`动作名称`、`参数`、`是否缓存对象名称`

* `对象名称`：单独的`组件/模块`（repo）名，传入的`targetName`在CTMediator中会被拼接为字符串`Target_targetName`（可以理解为`类名`），然后通过`NSClassFromString`将字符串转为应的`类`。如：需要集成`A`模块。传入`A`，会被拼接成`Target_A`，然后被转换为**[`Target_A` Class]**

* `动作名称`：当拥有一个`对象`后，我们需要调用这个对象的`方法`来做一些事情，传入的`actionName`在CTMediator中会被拼接为字符串`Action_actionName:`（可以理解为`方法名`），然后通过`NSSelectorFromString`将字符串转为应的`对象方法`。如：`模块A`需要`被push`出来。传入`isPushed`，会被拼接成`Action_isPushed:`，然后被转换为**[`Action_isPushed` SEL]**

* `参数`：当使用`对象`执行`方法`时，我们可能还需要一些`参数`来进行逻辑判断或往下传递，参数以`NSDictionary`对象传入

* `是否缓存Target对象`：一个BOOL值。单例中维护着一个可变字典`cachedTarget`，目的是对初始化过的`Target对象`进行存储，字典中的`key`是字符串`Target_targetName`，`value`是`Target对象`

2、缓存及异常处理

* target：先去缓存中取，如果为空则创建；创建之后还为空，说明根据字符串`Target_targetName`无法找到对应的类，则返回nil（实际开发过程中可以事先给一个固定的target，专门用于在这个时候顶上，然后处理这种请求的）；根据BOOL值选择`是否缓存Target对象`
* action：通过`respondsToSelector`判断target对象是否响应action。如果响应了，则调用`performSelector: withObject:`去执行action方法，并返回该`对象方法返回的值`；如果没有响应，说明根据字符串`Action_actionName:`无法找到对应的`对象方法`，我们可以定义一个`notFound:`方法，如果可以响应，说明target对象中实现了这么一个方法，调用`performSelector: withObject:`去执行`notFound:`方法；如果target对象也不响应`notFound:`方法，删除`cachedTarget`中缓存的`Target对象`，并返回nil（也可以用一个固定的target顶上）

#### 本地组件实现源码分析小结：

* `Target_targetName`对应的是`Target_A`这个类，`Action_actionName`对应的是**`Target_A`**中的对象方法**`Action_isPushed:`**，参数使用字典对象放在对象方法`Action_isPushed:`之后
* 每一个`Target_targetName`对应一个单独的业务，如`Target_A`对应**`AViewController`**，并且将它们放在同一个`组件/模块`（repo）下

### 二、本地组件调用方式

业务需求：点击`Home页面`中某个按钮，跳转到`A页面`，我们把A页面拿出来做一个单独的`组件/模块`来调用

1、创建`Target_A`，并将`AViewController.h`放在同一个`组件/模块`下。在`Target_A`中声明并实现`- (UIViewController *)Action_isPushed:(NSDictionary *)params`，在头文件中引入`AViewController.h`，在`Action_isPushed:`中创建`AViewController`对象，并返回该对象

2、通过创建CTMediator的Category（分类／类别）`CTMediator+A`，来增加针对`A组件/模块`（repo）的方法，`CTMediator+A`也是一个单独的`组件/模块`（repo）

3、在`CTMediator+A中`中声明并实现`- (UIViewController *)A_viewController:(NSDictionary *)params`。在方法中调用`performTarget: action: params: shouldCacheTarget:`，传入参数分别为@"A"、@"isPushed"、params、NO，该方法调用后，正常情况下返回的是一个`AViewController`对象（通过runtime最终调用的是`Target_A`对象中的`Action_isPushed:`方法，返回`AViewController`对象）

4、在我们原来引入`AViewController.h`的类中，将其替换为`CTMediator+A`，并使用`[[CTMediator sharedInstance] A_viewController:params]`替换`[[AViewController alloc] init]`来创建`AViewController`对象和传递参数

#### 本地组件调用方式小结：

* 调用顺序：**`CTMediator+A`** 中通过`A_viewController:`调用`performTarget: action: params: shouldCacheTarget:`  ——> 在**`CTMediator`**中通过传入的targetName（A），找到**`Target_A`** 对象，通过传入的actionName（isPushed:），找到`Target_A`对象中的对象方法**`Action_isPushed:`** ——> `Target_A`对象调用`Action_isPushed:`方法，返回业务逻辑处理后的**`AViewController对象`**
* 模块分工：`CTMediator+A`决定调用哪个`Target`和`Action`，并将`参数`一并传递给`CTMediator`；`CTMediator`通过`字符串拼接和runtime`去找`Target_A`和对象方法`Action_isPushed:`，并使`Target_A`对象调用对象方法`Action_isPushed:`；`Target_A`在`Action_isPushed:`中通过接收到的参数处理各种业务逻辑，并返回`AViewController对象`