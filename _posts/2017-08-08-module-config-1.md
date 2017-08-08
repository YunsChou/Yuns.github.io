---
layout: post
title: 【组件化】组件化的配置及命令行的使用
date: 2017-08-08 00:00:00
tags: 组件化
---

### 一、组件的.podspec文件存放

1、**远程**：在代码托管平台上新建空一个项目（YsPodSpec），用来存放组件的**.podspec** 文件

2、**本地**：使用命令在本地创建一个仓库，并使该仓库指向远程仓库（YsPodSpec）

```
pod repo add [本地repo名称] [远程仓库http地址]
```

本地仓库（repo）的作用：用来存放本地组件工程的.podspec文件，将本地.podspec文件上传（push）到远程仓库

### 二、组件工程文件／代码存放
1、**远程**：在代码托管平台上新建一个空项目（YsModuleA），用来存放组件工程

2、**本地**：使用命令在本地创建Xcode工程

```
pod lib create YsModuleA
```

![img](/assets/images/2017/module-config-1.png)

3、将要组件化的文件添加到Class文件夹下

4、修改YsModuleA.podspec中的版本、远程仓库首页／地址等配置

5、使用命令对组件进行验证 

```
pod lib lint
```

### 三、提交到远程仓库
1、使用命令提交组件工程文件／代码到远程仓库

```
git add .  
git commit -m "xxx" 
git push
```

如果是首次提交，需要关联远程仓库，参照代码托管平台创建的空项目即可

![img](/assets/images/2017/module-config-2.png)

2、使用命令提交组件的.podspec到远程仓库

```
git tag -a [版本] -m "xxx"
git push --tags
//将组件的.podspec文件放入本地repos，并上传（push）到远程仓库
pod repo push [本地仓库名] YsModuleA.podspec  
```

### 四、使用组件

1、在Podfile中，添加远程仓库地址，source '[远程仓库地址]'

![img](/assets/images/2017/module-config-3.png)

2、使用命令导入组件

```
pod install
```

### 五、附加：图片／css等资源的配置

1、图片：在Assets文件夹中创建Images.xcassets用来放置图片

![img](/assets/images/2017/module-config-4.png)

2、其它资源文件：直接放在Assets文件夹下

3、将组件资源打包到bundle中

![img](/assets/images/2017/module-config-5.png)

4、通过访问组件的bundle来获取图片等资源

![img](/assets/images/2017/module-config-6.png)