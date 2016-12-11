---
layout: post
title: 【openshift-3】添加Flask等第三方库+部署自己的应用
date: 2016-09-10 20:00:00
tags: openshift
---

**前提：请先学习git和flask的简单使用**

1、我们的操作只需要基础的git知识，如何使用git：[Pro Git（中文版）](http://git.oschina.net/progit/)

最低目标：将线上的项目`clone`到本地；将本地修改后的项目`push`到线上

2、Flask是一个使用Python编写的轻量级Web应用框架，详细请参考：[ Flask 文档（中文版）](http://www.pythondoc.com/flask/index.html)

最低目标：使用Flask实现`WSGI`接口



### 一、添加Flask依赖



#### 1、将线上的项目clone到本地

进入openshift中我们创建好的应用

![img](/assets/images/2016/openshift-guide-3-1.png)

右侧红框处，提示开发者使用`git clone`将线上项目拷贝至本地



#### 2、在哪添加第三方依赖库？

依赖关系可以被添加到Python应用程序下的 **requirements.txt** 或 **setup.py** 文件中（注意用词为 `或`）

如在requirements.txt添加，代码为：

```
Flask==0.10.1
```

如在setup.py添加，代码为：

```
install_requires=['Flask>=0.10.1']
```

*本教程在setup.py中添加第三方依赖库，在下面的步骤会有具体的演示操作*



### 二、修改主页内容



#### 1、项目目录和结构

可以看到我`git clone`下来的文件名为`python`（你在openshift中创建的项目名称），其中目录结构如下：

![img](/assets/images/2016/openshift-guide-3-2.png)

**.openshift**：这个文件夹及其中内容不需要操作

**requirements.txt**：可添加需要的依赖库（默认为空）

**setup.py**：添加一些配置信息（也可以添加依赖库）

**wsgi.py：python应用默认的入口文件**

我们可以通过修改`wsgi.py`里面的代码，来修改主页内容



#### 2、使用Flask修改主页内容

因为这里只讲解如何操作及思路，所以下面以最简单的方式来实现

* 在`setup.py`中添加依赖库Flask（添加红框处的代码）：

  ![img](/assets/images/2016/openshift-guide-3-3.png)

* 创建一个`helloflask.py`文件（代码如下）：

  ![img](/assets/images/2016/openshift-guide-3-4.png)

* 修改`wsgi.py`中的代码（代码如下）：

  ![img](/assets/images/2016/openshift-guide-3-5.png)

* 此时，项目结构如下（我们只新增了一个`helloflask.py`文件）：

  ![img](/assets/images/2016/openshift-guide-3-6.png)




### 三、部署到OpenShift

将本地修改后的项目`push`到线上

```
git add -A
git commit -m "add flask application"
git push
```

访问或刷新你的项目域名吧，看看是否显示`helloflask.py`中输出的内容



