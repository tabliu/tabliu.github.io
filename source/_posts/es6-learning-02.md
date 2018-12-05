---
title: ES6学习笔记（2）- 使用Let和Const
date: 2018-12-05 11:22:00
tags:
  - code
  - javascript
  - note
  - ES6
---

> JS中一般定义变量或常量都使用`var`来定义，在`ES6`中引入了`let`和`const`，这两个定义方法和之前的`var`有什么区别呢，我们来了解下。

## `let`变量定义

`let`和`var`的主要区别体现在***作用域***上。通过`var`定义的变量，作用域是整个封闭函数，为函数作用域，是全域的 ；而`let`定义的变量作用域是在块级或是子块中，被规定为块作用域，块作用域要比函数作用范围要小一些。但是如果两者既没在函数中，也没在块作用域中定义，那么两者都属于全局作用域。

```js
// var
{
  var a = 1;
}
console.log(a); // 1

// let
{
  let b = 1;
}
console.log(b); // Uncaught ReferenceError: b is not defined

```

在了解他们区别之前，我们先要了解一下函数预编译的几个概念：

预编译原则：

* 函数声明：***整体提升***；
* 变量： ***声明提升***；

预编译过程：

1. 创建`AO`对象：`Activation Object`（执行期上下文，作用域）；
2. 找形参和变量声明，将形参名和变量作为`AO`属性名，值为`undefined`；
3. 将实参值和形参统一，把实参的值赋给形参；
4. 在函数体里面找函数声明，值赋予函数体。

实例说明：

```js
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

test(1);
// 预编译：发生在函数执行的前一刻
// 找AO
AO {
  a: undefined, //第3步：1；第4步：function a() {}。执行后第1次：function a() {}；第2次：123；第3次：123
  b: undefined, //执行后function () {}
  d: function d() {} //执行后function d() {}
}

// 最后的执行结果：
function test(a) {
  console.log(a); // function a() {}
  var a = 123;
  console.log(a); //123
  function a() {};
  console.log(a); //123
  var b = function () {};
  console.log(b); // function () {}
  function d() {};
  console.log(d); // function d() {}
}

```

如果使用let定义呢，我们试试：

```js
function test(a) {
  console.log(a);
  let a = 123;
  console.log(a);
  function a() {};
  console.log(a);
  let b = function () {};
  console.log(b);
  function d() {};
  console.log(d);
}

test(1);
```

页面执行结果：报错：`Uncaught SyntaxError: Identifier 'a' has already been declared`，至于是什么原因，看完下面的内容您自然明白。

### 全局作用域

在全局作用域下定义变量，使用`var`和`let`比较相似。但`var`定义的变量可以当作全局`window`的一个属性来用，`let`定义的变量则不可以。

```js
var a = 10;
let b = 20;

console.log(window.a); // 10
console.log(window.b); // undefined
```

### 函数作用域

在函数作用域下定义变量，使用`var`和`let`是相同的，在该函数整体内都可以使用。

```js

function test() {
  var a = 10;
  let b = 20;

  console.log(a); // 10
  console.log(b); // 20
}
test();
```

### 块级作用域

在块级作用域下定义变量，使用`var`不仅可以在该块级内使用，还可以在该块关联的上下级使用；
而`let`只可以在该块级内使用，超出使用会报错。

```js
for (var i = 0; i < 10; i ++) {
  ...
}

console.log(i); // 10

for (let j = 0; j < 10; j ++) {
  ...
}

console.log(j); // `Uncaught ReferenceError: j is not defined`
```

### 变量重新声明

使用`let`声明的变量，在同一作用域内不可以再进行同名变量定义；
而使用`var`没有限制。

```js
var a = 10;
console.log(a); // 10
var a = [1, 2, 3];
console.log(a); // [1, 2, 3]

let b = 20;
console.log(b); // 20
var b = [1, 2, 3];
console.log(b); // `Uncaught SyntaxError: Identifier 'b' has already been declared`
```

## `const`常量定义

### 使用范围

和`let`一样，`const`定义的常量也只能在块级作用域内使用，且不可以再进行同名变量定义。

### 声明赋值

`let`定义变量，可以先声明不赋值，在使用的时候再进行赋值；
`const`则不可以，必须在定义的时候进行声明和赋值。

```js
let a;
a = 123;
console.log(a); // 123

const pi;
pi = 3.14;
console.log(pi); // `Uncaught SyntaxError: Missing initializer in const declaration`
```

### 重新赋值

`let`声明的变量，可以重新赋值，而且对变量类型没有要求；
`const`定义的常量不可以改变类型，也不能直接进行重新赋值操作，只能通过相应类型提供的方法进行修改。

```js
let a = 123;
a = [1, 2, 3]
console.log(a); // [1, 2, 3]

const b = 'abc';
b = ['a', 'b', 'c']
console.log(b); // `Uncaught TypeError: Assignment to constant variable.`

const c = [1, 2, 3];
c.push(4);
console.log(c); // [1, 2, 3, 4]
```

综合：相对`var`，使用`let`和`const`比较严谨，可以避免一些小错误的发生，而且可以更快定位错误产生的原因。通过`let`和`const`的使用，可以有效规范代码，提高团队合作中代码的可读性，值得推荐使用。

本节完。
