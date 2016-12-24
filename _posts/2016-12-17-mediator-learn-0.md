---
layout: post
title: 【组件化-0】使自己的框架支持cocoapods
date: 2016-12-17 00:00:00
tags: 组件化
---

## 目的

1、使自己的框架支持cocoapods

2、使用pod管理组件化模块的准备知识

## 说明

1、从使用范围来看，主要分为两个方面：

* 供自己和团队使用（一般放在私有仓库中），pod引用时需要添加仓库路径


* 供其他开发者调用（框架提交至pod trunk），pod引用方式类似AFNetworking

>  注意：如果供自己和团队使用的代码放在GitHub的 **公有仓库** 中，那么外部开发者也是可以调用的，在调用时需要加上仓库路径 **（本文即采用这样的方法，如果是内部使用，换成私有仓库即可）**

2、从操作步骤来看，也分为两个方面：

* 2.1、仅将框架和`.podspec`配置文件上传至GitHub
* 2.2、在满足 2.1 条件的基础上，提交.podspec到`pod trunk`

***

## 实战操作

### 一、提交代码到github

1、在自己的github下添加一个`repository`（一并勾选好gitignore、readme和LICENSE）

2、clone这个`repository`到本地，此时这个文件夹（`根目录`）中应该只有README.md和LICENSE文件

3、准备要提交的工程或代码（包括要提交的库和demo），确保运行没问题

4、将工程拖入到clone下来的文件夹中（框架文件最好放入一个单独的实体文件夹），将包含框架的文件夹从工程内移动/剪切到根目录

5、此时运行Xcode工程，会因为缺少文件报错，我们删除掉工程中被移除的文件（红色）和虚拟文件夹，使用Xcode从根目录将框架文件夹（及文件）引用进来，再次编译，通过

6、使用 `git push` 将代码提交到github

### 二、创建`.podspec` 文件

1、在根目录下，使用命令创建 `.podspec` 文件：

```
pod spec create YSPodsMe
```

此时，根目录下文件夹及文件如下：

`LICENSE`和`README.md`是创建`repository`时就配置好的，`TestPodspecMe`是工程文件夹，`YSPodsMe`是框架文件夹，`YSPodsMe.podspec`就是 `.podspec` 文件

![img](/assets/images/2016/mediator-learn-0-1.png)

2、编辑 `.podspec` 内容，并保存。（删除注释，保留我们需要配置的字段）以下为示例：

```
Pod::Spec.new do |s|

  s.name         = "YSPodsMe"
  s.version      = "0.0.1"
  s.summary      = "A Test of YSPodsMe."
  s.description  = <<-DESC
  测试：自己使用的pod库,不上传至pod trunk
                   DESC

  s.homepage     = "https://github.com/YunsChou/YSTestPodsMe"
  s.license      = "MIT"
  s.author       = { "YunsChou" => "264775449@qq.com" }
  s.source       = { :git => "https://github.com/YunsChou/YSTestPodsMe.git", :tag => "#{s.version}" }
  s.source_files = "YSPodsMe/*"

  s.platform     = :ios, '7.0'
  s.requires_arc = true
end
```

3、验证 `.podspec` 文件是否可用，在根目录下用终端执行命令 ：

```
pod lib lint
```

4、验证通过后，使用 `git push` 将代码提交到github，然后在Xcode中使用`YSPodMe`：

![img](/assets/images/2016/mediator-learn-0-2.png)

>  小结：示例代码[YSTestPodsMe]([https://github.com/YunsChou/YSTestPodsMe](https://github.com/YunsChou/YSTestPodsMe))，不可以在 [https://cocoapods.org](https://cocoapods.org)中检索到，需要添加仓库引用路径。在Podfile中引用：
>
>  pod 'YSPodsMe', :git => 'https://github.com/YunsChou/YSTestPodsMe.git'
>
>  至此，如果该库只供自己或团队使用，到这里就算完成了

***

### 三、提交`.podspec`到`pod trunk`

1、为自己的项目打tag：

```
git tag "v0.0.1"
git push --tags
```

2、查看 `pod trunk` 的注册信息，执行命令：

```
pod trunk me
```

如果是首次提交代码至 `pod trunk` ，会提示开发者：

```
[!]You neet to register a seesion first.
```

![img](/assets/images/2016/mediator-learn-0-3.png)

3、注册 `pod trunk`  账号（如已经注册过，可跳过此环节），执行命令：

```
pod trunk register 2647754496@qq.com "YunsChou" --verbose
```

信息格式为：邮箱 + 用户名，注册成功后会发送一封邮件到你的`邮箱`，根据邮件信息进行验证，验证之后可以用`pod trunk me`查看自己的信息

![img](/assets/images/2016/mediator-learn-0-4.png)

4、将通过上述`一`和`二`步骤的`.podspec`文件提交至`pod trunk`  ：

```
pod trunk push YSPods.podspec
```

![img](/assets/images/2016/mediator-learn-0-5.png)

5、等待一段时间（可能是几分钟或几十分钟），在 [https://cocoapods.org](https://cocoapods.org) 中检索自己的框架：

![img](/assets/images/2016/mediator-learn-0-6.png)

> 小结：示例代码[YSTestPods]([[https://github.com/YunsChou/YSTestPods](https://github.com/YunsChou/YSTestPods))，可以在 [https://cocoapods.org](https://cocoapods.org)中检索到，不需要添加仓库引用路径，*本质上默认路径就是cocopods的路径*。在Podfile中引用：
>
> pod 'YSPods'
>
> 至此，使自己的框架通过cocoapods `仅支持内部使用`和 `开源给其他开发者使用`，到这里都完成了
>

