---
layout: post
title: OC函数式、链式、响应式编程
date: 2016-07-10 00:00:00
tags: code
---

## 目的

1. 理解函数式调用
2. 理解链式调用
3. 函数式和链式调用区别
4. 理解响应式编程

[DEMO点我](https://github.com/YunsChou/ExercisProject/tree/master/链式函数式响应式编程)

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
> 2. 当`成员方法`的`参数列表为空`时，可以使用`点语法`调用这类函数
> 3. 当需要`连续调用`函数式或链式时，成员函数的`返回值`必须为`当前对象`
>
> 第1点说明了函数式和链式调用的展现形式，第2点说明了使用点语法的前提条件
>
> 你可能会产生疑惑：在OC的平常使用中，`[]`是用来调用`方法`的，`.`是用来调用`属性`的，现在`.`也能调用方法了？
>
> 在这里，我们需要深刻认识到：**OC点语法的本质是方法调用**。当我们用@property声明属性时，默认是生成了`一个成员变量和setter/getter方法`，OC中`点语法`不是访问成员变量，而是隐式调用getter/setter方法。同理，用`[]`调用属性时，其实是调用了该属性的getter方法
>

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
    //使用KVO监听属性userName变化
    [model addObserver:self forKeyPath:@"userName" options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:nil];
    self.model = model;
}

//当属性的值发生变化时，自动调用此方法
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context
{
    //如果userName值发生改变，改变按钮上的内容
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

//点击按钮改变userName的值
- (IBAction)userNameButtonClick:(id)sender {
	//userName的值由初始化 '隔壁' 变为 '老王'
    self.model.userName = @"老王";
}
```

> 小结：OC中可以使用KVO或NSNotification实现响应式编程
>
> 以下引用 [Objective-C之KVC、KVO](http://www.cnblogs.com/kenshincui/p/3871178.html#kvo) ：
>
> 在ObjC中使用KVO操作常用的方法如下：
>
> * 注册指定Key路径的监听器： addObserver: forKeyPath: options:  context:
> * 删除指定Key路径的监听器： removeObserver: forKeyPath、removeObserver: forKeyPath: context:
> * 回调监听： observeValueForKeyPath: ofObject: change: context:
>
> KVO的使用步骤也比较简单：
>
> * 通过addObserver: forKeyPath: options: context:为被监听对象（它通常是数据模型）注册监听器
> * 重写监听器的observeValueForKeyPath: ofObject: change: context:方法

---

> 通过上面简单的函数式和链式调用，我们已经了解到：
>
> 1. 要实现函数式和链式调用，返回值必须为该对象
> 2. 且链式调用，还须满足成员方法的参数列表为空
>
> 同时，对于这样最基本的调用，也存在一些局限性，如：
>
> 1. 函数式调用，返回的是对象本身，获取方法的返回值不够直接（内部可以通过属性接收处理后的值，外部再通过对象的属性获取值）
> 2. 链式调用，无法传入参数
>
> 所以，在实际使用中，我们可以通过结合block的特性，来解决这些局限性问题

学习以下知识时，先参考这篇文章 [block和C语言函数对比](http://yunschou.github.io/2016/07/code-block/)

## 结合block的函数式调用

> 为了实现函数式调用，必须保证方法的返回值是该对象
>
> 为了获取方法的返回值，可以在方法的参数列表中动手脚

```
typedef int (^valueBlock)(int value);

typedef BOOL (^boolBlock)(int value);

//声明
@interface Calculator : NSObject

@property (nonatomic, assign) int result;

@property (nonatomic, assign) BOOL isEqual;

- (Calculator *)calculator:(valueBlock)block;

- (Calculator *)equal:(boolBlock)block;

@end
```

```
//实现
@implementation Calculator

- (Calculator *)calculator:(valueBlock)block
{
    NSLog(@"result -- %d", _result);
    _result = block(_result);
    NSLog(@"result -- %d", _result);
    return self;
}

- (Calculator *)equal:(boolBlock)block
{
    _isEqual = block(_result);
    return self;
}

@end
```

```
//调用
Calculator *c = [[Calculator alloc] init];
    
BOOL isEqual = [[[c calculator:^int(int value) {
    //返回计算后的值
    value += 2;
    value *= 5;
    return value;
}] equal:^BOOL(int value) {
    //判断计算后的值是否等于10
    return value == 10;
}] isEqual];//获取是否等于的BOOL值
NSLog(@"函数式编程isEqual -- %d", isEqual);
```

## 结合block的链式调用

> 为了实现链式调用，必须保证方法的参数列表为空
>
> 为了能够传入参数，必须在方法的返回值里做文章

```
@class Person;

typedef Person * (^nameBlock)(NSString *name);

typedef Person * (^workBlock)();

//声明
@interface Person : NSObject

- (nameBlock)who;

- (workBlock)work;

@end
```

```
//实现
@implementation Person

- (nameBlock)who
{
    return ^Person * (NSString *name){
        NSLog(@"%@", name);
        return self;
    };
}

- (workBlock)work
{
    NSLog(@"调用work方法");
    return ^Person * (){
        NSLog(@"敲代码");
        return self;
    };
}

@end
```

### 链式调用的等价函数式分析

调用

```
Person *p = [[Person alloc] init];
//使用链式调用输出：小明程序员
p.who(@"小明").work();
```

分析

```
//只使用"."和"[]"调用work方法，是等价的，但此时还无法触发内部block
workBlock block1 = [p work];
workBlock block2 = p.work;
//只有在block之后调用"()"才能触发block代码块
block1();
block2();
//所以下面两个方法调用结果是一样的，只是两种不同的写法
[p work]();
p.work();
```

等价

```
//经过上面的分析，以下代码和p.who(@"小明").work();是等价的，只是用点语法调用更加直观
[[p who](@"小明") work]();
```

## 函数式和链式调用

只要成员方法的`最终返回值`为当前对象，函数式和链式可以混搭调用

```

@class Robot;

typedef int (^moneyBlock) (int money);

typedef Robot * (^netBlock) ();

typedef Robot * (^learnBlock) (NSString *learn);

//声明
@interface Robot : NSObject

@property (nonatomic, assign) int money;

- (Robot *)money:(moneyBlock)block;

- (netBlock)netWork;

- (learnBlock)learnWithTime:(int)time;

- (Robot *)shutdown;
@end
```

```
//实现
@implementation Robot

- (Robot *)money:(moneyBlock)block
{
    _money = block(_money);
    NSLog(@"机器人价值 %d rmb", _money);
    return self;
}

- (netBlock)netWork
{
    return ^Robot *(){
        NSLog(@"连上网");
        return self;
    };
}

- (learnBlock)learnWithTime:(int)time
{
    return ^Robot * (NSString *learn){
        NSLog(@"大师帮它训练 %@ ，用了 %d 个小时", learn, time);
        return self;
    };
}

- (Robot *)shutdown
{
    NSLog(@"关机");
    return self;
}

@end
```

```
//调用
Robot *r = [[Robot alloc] init];
//打印：机器人价值150rmb 连上网 大师帮它训练围棋，用了3个小时 关机 机器人价值180rmb
[[[r money:^int(int money) {
    //机器价格100，额外交税50%
    money += 100;
    money *= 1.5;
    return money;
}].netWork() learnWithTime:3](@"围棋").shutdown money:^int(int money){
    //增值了20%
    money *= 1.2;
    return money;
}];
```

## 总结

1. 函数式和链式调用的成员方法都是返回一个对象
2. 如果要实现带参数的链式调用，成员方法返回带参数的block对象
3. 成员方法返回的block对象，通过`()`来触发这个block，获取block对象的返回值
4. 当最终返回值（成员方法的返回值或成员方法返回的block对象的返回值）为nil或其它对象时（如NSString），则终止函数式或链式的连续调用

