```
layout: post
title: OC函数式、链式、响应式编程
date: 2016-07-10 00:00:00
tags: code
```

## 目的

1. 理解函数式调用
2. 理解链式调用
3. 函数式和链式调用区别
4. 理解响应式编程

## 建议

理解`链式调用`前，先理解`函数式调用`

## 函数式到链式

```
//声明
@interface Animal : NSObject

- (Animal *)run;

- (Animal *)jump;

- (Animal *)say:(NSString *)words;

@end

//实现
@implementation Animal

- (Animal *)run
{
    NSLog(@"run -- ");
    return self;
}

- (Animal *)jump
{
    NSLog(@"jump -- ");
    return self;
}

- (Animal *)say:(NSString *)words
{
    NSLog(@"say -- %@", words);
    return self;
}

@end
```

```
//调用
Animal *a = [[Animal alloc] init];
    
//函数式调用
[[a run] jump];
//链式调用（Xcode⚠️提示“调用结果未使用”）
a.run.jump;

//带参数的方法，不能使用点语法调用
[[[a run] say:@"hello"] jump];
//❌以下为错误写法
//a.run.say:@"hello";
```

> 小结：能使用`.`的地方肯定能够使用`[]`，反之则不然
>
> 1. 函数式调用使用`[]`，链式调用使用`.`
> 2. 当`成员函数`的`参数列表为空`时，可以使用`点语法`调用这类函数
> 3. 当需要`连续调用`函数式或链式时，成员函数的`返回值`必须为`当前对象`
>
> 第一点说明了函数式和链式调用的展现形式，第二点说明了使用点语法的前提条件。
>
> 你可能会产生疑惑：在OC的平常使用中，`[]`是用来调用`方法`的，`.`是用来调用`属性`的，现在`.`也能调用方法了？
>
> 在这里，我们需要深刻认识到：**OC点语法的本质是方法调用**
>
> 当我们用@property声明属性时，默认是生成了`一个成员变量和setter/getter方法`，OC中`点语法`不是访问成员变量，而是隐式调用getter/setter方法，同理，用`[]`调用属性时，其实是调用了该属性的getter方法

## 响应式编程

```
//声明
@interface ResponseModel : NSObject

@property (nonatomic, copy) NSString *userName;

@end

//实现
@implementation ResponseModel

- (instancetype)init
{
    self = [super init];
    if (self) {
        _userName = @"隔壁";
    }
    return self;
}

@end
```

```
//调用
- (void)responseMethod
{
    ResponseModel *model = [[ResponseModel alloc] init];
    //使用KVO监听model的userName属性变化
    [model addObserver:self forKeyPath:@"userName" options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:nil];
    self.model = model;
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context
{
    //如果userName属性发生改变，1秒后改变按钮上的内容
    if ([keyPath isEqualToString:@"userName"]) {
        NSString *oldName = change[@"old"];
        NSString *newName = change[@"new"];
        [self.userNameButton setTitle:oldName forState:UIControlStateNormal];
        //设置延时1秒
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
            [self.userNameButton setTitle:newName forState:UIControlStateNormal];
        });
    }
}

- (IBAction)userNameButtonClick:(id)sender {
    self.model.userName = @"老王";
}
```

> 小结：
>
> 

---

学习以下知识时，先参考这篇文章 [block和C语言函数对比](http://yunschou.github.io/2016/07/code-block/)

## 结合block的函数式调用





## 结合block的链式调用







