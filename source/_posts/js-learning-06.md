---
title: JavaScript学习笔记（6）- 函数
date: 2018-07-03 10:10:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。


## 函数定义
> 函数具有***高内聚、弱偶合***的特点。

* 函数声明：function 函数名() { 函数体 }
* 函数表达式：var 变量名 = function 函数名() { 函数体 }

***表达式会忽略函数名，变成一个匿名函数，可通过变量名.name打印出函数名。***

## 函数组成形式

### 函数名称

* 同变量名规则
* 小驼峰形式

### 参数

* 形式参数：arguments[i]
* 实际参数：函数名[i]

***形参和实参有强对应关系，一个改变，另一个也会跟着变，但他们是两个不同的地址；但如果不对应实参，就会不对应映射关系。***

### 返回值（结束体）

* 直接返回：return
* 返回值：return 返回值

### 递归
* 更好找规律：return公式；
* 必须要有出口：结束死循环。

***递归可以让代码更简洁，但实现最不高效，先执行的最后被执行完。***

## 函数执行三要素

* 执行边界
* 开始边界
* 结束边界


## 函数预编译

### 原则

* 函数声明：***整体提升***；
* 变量： ***声明提升***；

### 预编译前奏

* imply global暗示全局变量：任何变量，如果变量未经声明就赋值，此变量就为全局对象window所有。

```javascript
//直接赋值
a = 10;
相当于：
window.a = 10;
console.log(a) ===> console.log(window.a)

//连续赋值
function test() {
  var a = b = 123;
}
test();
console.log(window.a)  //undefined
console.log(window.b)  //123
```

* 一切声明的全局变量，都是归window所有，是window的属性。

```javascript
var a = 123;
相当于：
window {
  a = 123
}
```

* ***window 就是全局***

### 预编译过程：发生在函数执行的前一刻。预编译过程：

* 创建AO对象：Activation Object（执行期上下文，作用域）；
* 找形参和变量声明，将形参名和变量作为AO属性名，值为undefined；
* 将实参值和形参统一，把实参的值赋给形参；
* 在函数体里面找函数声明，值赋予函数体。
  示例如下：

```javascript
function test(a) {
  console.log(a);
  var a = 123;
  console.log(a);
  function a() {};
  console.log(a);
  var b = function () {};
  console.log(b);
  function d() {};
  console.log(d);
}
// 预编译：发生在函数执行的前一刻
// 找AO
AO {
  a: undefined, //第3步：1；第4步：function a() {}。执行后第1次：function a() {}；第2次：123；第3次：123
  b: undefined, //执行后function () {}
  d: function d() {} //执行后function d() {}
}
```

***敲黑板***
* 打印的变量后有同名的函数声明，打印的一定是function；

```javascript
function bar() {
  return foo;
  foo = 10;
  function foo() {}
  var foo = 11;
}
console.log(bar());
```

* 打印的变量前有变量声明并赋值，打印的一定是最后的赋值；

```javascript
console.log(bar());
function bar() {
  foo = 10;
  function foo() {}
  var foo = 11;
  return foo;
}
```

进阶练习：

```javascript
a = 100;
function demo(e) {
  function e() {};
  arguments[0] = 2;
  console.log(e); //2
  if (a) {
  var b = 123;
  function c() {
  //这里是个bug，最新浏览器里规定if里不允许包含function
  }
}
var c;
a = 10;
var a;
console.log(b); //undefined
f = 123;
console.log(c); //function/undifined
console.log(a); //10
}
var a;
demo(1);
console.log(a); //100
console.log(f); // 123

/*
GO {
  a: undefined, // 100
  demo: undefined, // function
  f: 123
}
*/
/*
AO {
  e: undefined, //1,function, 2
  b: undefined,
  c: undefined, //function
  a: undefined
}
*/
```

本节完。
