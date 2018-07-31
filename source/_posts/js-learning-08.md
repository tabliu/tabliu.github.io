---
title: JavaScript学习笔记（8）- 闭包
date: 2018-07-05 10:10:10
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 闭包的定义

> 当内部函数被保存到外部时，将会生成闭包。闭包会导致原有作用域链不释放，造成内存泄露。

```javascript
function a() {

  function b() {
    var bbb = 234;
    console.log(aaa);
  }

  var aaa = 123;
  return b;
}

var glob = 100;
var demo = a();
demo();
```

里面的函数被保存到了基础函数的外部就会形成闭包。当前可以通过`return`或者把内部函数赋值给变量的形式来跳到外面。

什么是内存泄漏？

> 内存空间被占用越多，剩余可用空间越来越少，形象的叫内存泄漏。

## 闭包的作用

### 实现公有变量

```javascript
function add() {
  var count = 0;
  function demo() {
    count++;
    console.log(count);
  }

  return demo;
}

var counter = add();
counter(); //1
counter(); //2
counter(); //3
```

这样做的好处既可以实现公有变量，又不会造成变量污染。

### 可以做缓存（存储结构）

```javascript
function test() {
  var abc = 100;
  function a() {
    abc++
    console.log(abc);
  }

  function b() {
    abc--
    console.log(abc);
  }

  return [a, b];
}

var myArr = test();
myArr[0](); //101
myArr[1](); //100
```

从上面的例子可以看出，`a()` 和 `b()` 有一个共用的作用域链 `testAO`，也包括一个公共的变量 `abc`。

接下来看另外一个例子：

```javascript
function eater() {
  var food = "";
  var obj = {
    eat: function () {
      console.log("I'am eating " + food + "!");
      food = "";
    },
    push: function (myFood) {
      food = myFood;
    }
  }

  return obj;
}

var eater = eater();
eater.push('banana');
eater.eat();
```

多个函数和一个函数形成闭包，他们直接的变量可以共用，类似于缓存的存储机制。

### 可以实现封装，属性私有化

### 模块化开发，防止污染全局变量

这两个作用后期讲到后进行补充。

本节完。
