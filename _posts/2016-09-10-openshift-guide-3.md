---
layout: post
title: 【openshift-3】添加flask等第三方库+部署自己的应用
date: 2016-09-10 19:00:00
tags: openshift
---

### 一、添加flask依赖

#### 1、准备git基本知识

进入openshift中我们创建好的应用



![img](/assets/images/2016/openshift-guide-3-1.png)



右侧红框处，提示开发者使用`git clone`将线上项目拷贝至本地

我们的操作只需要基础的git知识，如何使用git，请学习：[Pro Git（中文版）](http://git.oschina.net/progit/)


#### 2、什么是flask

Flask是一个使用Python编写的轻量级Web应用框架，详细请参考：[ Flask 文档（中文版）](http://www.pythondoc.com/flask/index.html)

#### 3、为应用添加第三方依赖库

依赖关系可以被添加到Python应用程序下的requirements.txt或setup.py中

如在requirements.txt添加代码为：

```
Flask==0.10.1
```

在setup.py添加代码为：

```
install_requires=['Flask>=0.10.1']
```

在下面的步骤会有具体操作

### 二、修改主页内容

#### 1、项目结构和目录

可以看到我们`git clone`下来的文件名为`python`（openshift中项目名称），其中目录结构如下：

![img](/assets/images/2016/openshift-guide-3-2.png)

*.openshift*：这个文件夹及其中内容我不需要操作

*requirements.txt*：可添加需要的依赖库（默认是空的）

*setup.py*：添加一些配置信息（也可以添加依赖库）

*wsgi.py*：python应用默认的入口文件

我们可以通过修改wsgi.py里面的代码，来修改主页内容

#### 2、简单的操作方法

因为这里只讲解如何操作，所以我们以最简单的方式来实现（截图）

* 在`setup.py`中添加依赖库flask（添加红框处的代码）

  ​

  ![img](/assets/images/2016/openshift-guide-3-3.png)

  ​

* 创建一个`helloflask.py`文件（代码如下）

  ​

  ![img](/assets/images/2016/openshift-guide-3-4.png)

  ​

* 修改`wsgi.py`中的代码（代码如下）

  ​

  ![img](/assets/images/2016/openshift-guide-3-5.png)

  ​

* 项目结构如下

  ​

  ![img](/assets/images/2016/openshift-guide-3-6.png)

  ​

### 三、部署到OpenShift

```
git add -A
git commit -m "add flask application"
git push
```

访问或刷新你的项目域名吧，看看是否显示`helloflask.py`中输出的内容



