---
title: JS作用域系列—闭包
date: 2018-09-28 14:57:04
tags: javascript
categories: 总结
---

<!-- more -->
## 1. 前言
原理只有一句话：
**函数可以记住并访问所在的词法作用域，即使函数是在当前词法作用域之外执行，这时就产生了闭包。《你不知道的javascript上》**


## 2. 举例
```javascript
function a() {
    var i=0;
    //函数b
    function b(){alert(++i);}
    return b;
}
//函数c
var c = a();
c();
```

### 2.1 代码特点
1. 函数b嵌套在函数a内部；
2. 函数a返回函数b。
代码中函数a的内部函数b，被函数a外面的一个变量c引用的时候，这就叫创建了一个闭包。有时候函数b也可以用一个匿名函数代替来返回，即return function(){};

### 2.2 优点
1. 保护函数内的变量安全,加强了封装性
2. 在内存中维持一个变量(用的太多就变成了缺点，占内存)
> 闭包之所以会占用资源是当函数a执行结束后, 变量i不会因为函数a的结束而销毁, 因为b的执行需要依赖a中的变量。
3. 逻辑连续，当闭包作为另一个函数调用的参数时，避免你脱离当前逻辑而单独编写额外逻辑。
4. 方便调用上下文的局部变量。

### 2.3 实践

闭包的典型框架应该就是jquery了。闭包是javascript语言的一大特点，**主要应用闭包场合主要是为了：设计私有的方法和变量。**这在做框架的时候体现更明显，有些方法和属性只是运算逻辑过程中的使用的，不想让外部修改这些属性，因此就可以设计一个闭包来只提供方法获取。不适合场景：返回闭包的函数是个非常大的函数。闭包的缺点就是常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。


## 3. 场景

闭包的常用场景有
1. 是函数作为返回值，
2. 是函数作为参数来传递。
3. 闭包会把函数中变量的值保存下来，供其他函数使用，这些变量会一直保存在内存当中，这样占用大量的内存，使用不当很可能造成内存泄漏，故要及时清除，清楚方法有两种，一是标记清除，二便是引用计数清除。


不适用于
1. 返回闭包的函数是个特别大的函数,很多高级应用都要依靠闭包实现.

> **使用闭包的好处是不会污染全局环境，方便进行模块化开发，减少形参个数，延长了形参的生命周期，坏处就是不恰当使用会造成内存泄漏**