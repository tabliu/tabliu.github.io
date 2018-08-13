---
title: JavaScript学习笔记（17）- JS中的this
date: 2018-08-03 09:40:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 一、this的使用

### 函数预编译过程中`this`指向`window`

```javascript
function test(c) {
  var a = 123;
  function b() {}
}

test(1);

//预编译环节
AO {
  arguments : [1],
  this : window,
  c : 1;
  a : undefined,
  b : function,
}

```

如果该函数是构造函数，在执行`new test()`过程中`this`会发生改变：

```javascript
//this = Object.create(test.prototype);
//相当于
//{
//  __proto__ : test.prototype
//}
```

```javascript
function test() {
  console.log(this);
}

test();

```

### 全局作用域中`this`指向`window`

### `call`和`apply`可以改变函数运行是的`this`指向

### `obj.func();`里`func()`里面的`this`指向`ojb`

```javascript
var obj = {
  name : 'abc',
  a :  function() {
    console.log(this.name);
    console.log(this);
  }
}
obj.a();
```

对象里函数中的`this`指向：***谁调用的这个方法，`this`就指向谁***。

```javascript
var name = '222';
var a = {
  name : '111',
  say : function() {
    console.log(this.name);
  }
}
var fun = a.say;
fun();
//function() {
//  console.log(this.name); //this指向window，结果'222'
//}
a.say();
//function() {
//  console.log(this.name); //this指向a，结果'111'
//}
var b = {
  name : '333',
  say : function(fun) {
    fun();
  }
}
b.say(a.say);
//function() {
//  console.log(this.name); //this指向window，结果'222'
//}
b.say = a.say;
b.say();
//function() {
//  console.log(this.name); //this指向b，结果'333'
//}
```

## 其他知识点

### arguments.callee

> 指向函数自身的引用。

举个栗子：求10的阶乘

```javascript
var num = (function(n){
  if(n == 1) {
    return 1;
  }
  return n * arguments.callee(n-1);
})(10)

console.log(num);
```

一般的做法是定义一个实名函数`fac(n)`，通过枚举的方法来循环调用`fac(n) * fac(n-1)`；但如果是通过立即执行函数来实现，就无法循环调用函数名，只能通过`argumengs.callee`来调用自身。

```javascript
function a() {
  console.log(arguments.callee);
  function b() {
    console.log(arguments.callee);
  }
  b();
}
a();
```

根据函数执行顺序从上到下执行，先执行`function a()`中`console.log(arguments.callee);`；再执行`function b()`中`console.log(arguments.callee);`。所以返回结果也是：`function a(){}`和`function b(){}`。

总结：`arguments.callee`返回的是***当前执行环境下的函数体***。

### func.caller

> 返回调用函数的函数体。

```javascript
function a() {
  console.log(a.caller); //null
  b();

}
function b() {
  console.log(b.caller); //function a() { b(); }
}
a();
```

`function a() {}`函数没有任何调用；`function b() {}`被`function a() {}`调用执行，所以返回结果：`null`和`function a() { b(); }`

总结：

* 这两个方法经常会拿来一起比较，但实际开发中很少用到；
* 在ES5语法中，这两个方法不可用。

### 三目运算符

> 条件判断 ? 是 : 否 并且会返回值

```javascript
var num = 1 > 0 ? ('10' > '9' ? 0 : 1) : 1 - 1;
// 字符串相比是比的主位ask码
console.log(num); //1
```

## 课后练习

### 求`console.log`打印结果？

```javascript
var foo = 123;

function test () {
  this.foo = 234;
  console.log(foo);
}

test();
```

打印结果是：`234`。


如果执行`new test();`的打印结果呢？

返回：`123`。

### 运行`test()`和`new  test()`的结果分别是什么？

```javascript
var a = 5;

function test () {
  a = 0;
  console.log(a);
  console.log(this.a);
  var a;
  console.log(a);
}

//test();
  // A0 {
  //   a : undefined;
  // }
  //a = 0;
  //console.log(a); //0
  //console.log(this.a); //5
  //var a;
  //console.log(a); //0

  //new test();
  // this == Object.create(test.prototype);
  // A0 {
  //   a : undefined;
  // }
  //a = 0;
  //console.log(a); //0
  //console.log(this.a); //undefined
  //var a;
  //console.log(a); //0

```

总结：

`this`考题涉及到的知识点：

* 函数预编译(AO)；
* `this`指向问题；
* 函数和构造函数的预编译差异。

## 课后作业

### 浅克隆

```javascript
var obj1 = {
  name : 'abc',
  age : 123,
  sex : 'male',
  card : ['visa', 'life',['card1', 'card2']]
}

var obj2 = {}

function clone(origin, target) {
  var target = target || {};
  for(var prop in origin) {
    target[prop] = origin[prop];
  }
  return target;
}

clone(obj1, obj2);
```

浅克隆的方法很好实现，但也存在一个问题。如果对象的属性值是非原始值，那目标对象和原始对象都会指向到相同的索引上，改变一方，另一方也会发生改变。

现实项目中是不希望发生的，这样就需要用到深克隆的方法来实现。

### 深克隆

```javascript
var obj1 = {
  name : 'abc',
  age : 123,
  sex : 'male',
  card : ['visa', 'life',['card1', 'card2']],
  wife : {
    name : 'bcd',
    son : {
      name : 'cdde'
    }
  }
}

var obj2 = {}
```

分析思路：

* 遍历对象：`for(var prop in obj)`
* 判断是否是原始值：`typeof(obj[prop]) == Object`
* 判断是否是数组还是对象： `constructor/instanceof/toString()`三种方法，推荐使用`toString()`
* 建立相应的数组或者对象：`[] or {}`
* 递归

实现：

```javascript
  var obj1 = {
    name: 'abc',
    age: 123,
    sex: 'male',
    card: ['visa', 'life', ['card1', 'card2']],
    wife: {
      name: 'bcd',
      son: {
        name: 'cdde'
      }
    }
  }

  var obj2 = {}

  function deepClone(origin, target) {
    var target = target || {},
      toStr = Object.prototype.toString,
      arrStr = '[object Array]';
    for (var prop in origin) {
      if (origin.hasOwnProperty(prop)) {
        if (origin[prop] !== 'null' && typeof (origin[prop]) == 'object') {
          if (toStr.call(origin[prop]) == arrStr) {
            target[prop] = [];
          } else {
            target[prop] = {}
          }
          deepClone(origin[prop], target[prop]);
        } else {
          target[prop] = origin[prop];
        }
      }
    }

    return target;
  }
```

使用三目运算符还看简化下代码：

```javascript
  function deepClone(origin, target) {
    var target = target || {},
      toStr = Object.prototype.toString,
      arrStr = '[object Array]';
    for (var prop in origin) {
      if (origin.hasOwnProperty(prop)) {
        if (origin[prop] !== 'null' && typeof (origin[prop]) == 'object') {
          // if (toStr.call(origin[prop]) == arrStr) {
          //   target[prop] = [];
          // } else {
          //   target[prop] = {}
          // }
          target[prop] = toStr.call(originp[prop] == arrStr) ? [] : {};
          deepClone(origin[prop], target[prop]);
        } else {
          target[prop] = origin[prop];
        }
      }
    }

    return target;
  }
```

总结：

在实现深克隆功能的过程中一定要注意一下几个问题：

1. `Object.prototype.toString`的使用；
2. `hasOwnProperty()`的使用；
3. `===`与`==`的区别使用；
4. `typeof` 的返回结果都是小写的字符串。

本节完。
