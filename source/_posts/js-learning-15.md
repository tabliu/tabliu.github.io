---
title: JavaScript学习笔记（15）- 命名空间
date: 2018-08-01 09:00:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 命名空间的作用

> 管理变量，防止污染全局，适用于模块化开发。

## 命名空间的方法

### 传统方法：在变量前面加前缀

```javascript
var = jcName;
```

这种方法存在很多局限性，现在已基本抛弃使用。

### 通过对象属性来产生命名空间

```javascript
var org = {
  department1 : {
    zhangsan : {
      name : 'abc',
      age : 123
    },
    lisi : {

    }
  },
  department2 : {
    zhaowu : {

    },
    wangmazi : {

    }
  }
}

var zhangsan = org.department1.zhangsan;
zhangsan.name = 'bcd';
```

这种方法比较麻烦，不是最优的命名空间解决方法。

### 通过闭包来实现变量私有化

用闭包来解决，返回方法的调用。

`init`是入口函数，初始化，用来调用不同函数的执行。

```javascript
var init = (function(){
  var name = '123';

  function callName() {
    console.log(name);
  }

  return function() {
    callName();
  }
}())

init();
```

这样通过立即执行函数形成一个闭包，`init`用来执行`return`出来的方法，避免了变量冲突。

```javascript
var init = (function(){
  var name = 'aaa';

  function callName() {
    console.log(name);
  }

  return function() {
    callName();
  }
}())

var initDeng = (function(){
  var name = 'bbb';

  function callName() {
    console.log(name);
  }

  return function() {
    callName();
  }
}())

var name = 'ccc';

init(); //aaa
initDeng(); //bbb
console.log(name); //ccc
```

即使有全局变量或者其他函数的定义，也不会相互影响，很好的解决了变量污染的问题。

延伸阅读：[在JavaScript中创建命名空间的几种写法
](http://ourjs.com/detail/538d8d024929582e6200000c)

本节完。
