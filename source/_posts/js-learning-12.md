---
title: JavaScript学习笔记（12）- 原型链
date: 2018-07-27 09:00:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 原型链的定义

```javascript
Grand.prototype.lastName = 'Liu';
function Grand () {}

var grand = new Grand();

Father.prototype = grand;
function Father () {
  this.name = 'laoda';
}

var father = new Father();

Son.prototype = father;
function Son () {
  this.ability = function shot(){}
}

var son = new Son();
```

访问`son`相关属性会打印出如下内容：

![原型链属性](/demo/demo_05.png)

我们通过打印对象`son`的属性可以发现：`son`不仅具有自身的对象属性，还继承其父级`father`的原型对象属性，同时也继承了其父级`father`的父级`Grand`的原型对象属性。
这种链式的继承关系，我们称之为原型链。

原型链的终端是`Object.prototype`。

## 原型链的增删改查

```javascript
Grand.prototype.lastName = 'Liu';
function Grand () {}

var grand = new Grand();

Father.prototype = grand;
function Father () {
  this.name = 'laoda';
  this.fortune = {
    card1 = 'visa'
  }
}

var father = new Father();

Son.prototype = father;
function Son () {
  this.ability = function shot(){}
}

var son = new Son();
```

通过不同的打印和赋值，查看结果：

![原型链操作](/demo/demo_06.png)

`son.fortune.card2 = 'Master'`这种修改，是引用值自己的修改，相当于自己修改自己的属性，这不是赋值修改，是一种调用修改。这种也仅限于引用值的修改，原始值是不可以的。

`son.fortune = 200`这种修改是赋值修改，相当于给自己加了属性。对原型不受影响。

```javascript
Person.prototype = {
  height : 100
}

function Person() {
  eat : function () {
    this.height ++;
  }
}

var person = new Person();
person.eat();

console.log(person.height); //101
console.log(Person.prototype.height); //100
```

通过上例我们可以了解：原型链的增删改查基本和原型的增删改查规则一致。

```javascript
Person.prototype = {
  name: 'a',
  sayName : function() {
    console.log(this.name);
  }
}

function Person() {
}

var person = new Person();
```

以上代码我们在控制台输出`person.sayName()`打印结果是什么呢？答案是`a`，很好理解。

我们对构造函数`Person`增加一个属性：

```javascript
Person.prototype = {
  name: 'a',
  sayName : function() {
    console.log(this.name);// 如果直接写成name会报错，没有这个变量
  }
}

function Person() {
  this.name = 'b';
}

var person = new Person();
```

现在控制台输出`person.sayName()`打印结果是什么呢？答案是`b`，如何理解。这里关键点在于`this`的指向是谁？

`this`指向规则： ***谁调用这个方法，this就指向谁***。

## 原型的另一种创建方法：`Object.create(原型)`

我们一般构建对象有两种方法：`var obj = {}` 或者 `var obj1 = new Object()`。这两种构建方法基本是一样的，我们推荐使用第一种自面量式的创建方法。而且这两种对象都有原型，其`__proto__`指向的是`Object`对象的终端。

通过`Object.create(原型)`也可以创建对象，但是必须要基于某个对象原型。

```javascript
Person.prototype.name = 'sunny';
function Person() {

}

var person = new Person();

var person1 = Object.create(Person.prototype);
```

上面`person`和`person1`两个对象的原型都是`Person.prototype`。

绝大多数对象最终都会继承自`Object.prototype`，但是，也存在一种例外。

我们通过`Object.create()`方法来创建一个对象，如果不添加原型会是什么情况？

如下图：

![原型创建对象](/demo/demo_07.png)

报错内容提示：对象原型必须是一个对象或者`null`。

所以通过`Object.create()`方法来创建对象时，也可以使用`null`来创建一个没有原型的对象。但这个对象不具有`Object.prototype`的任何属性和方法。

![原型创建对象](/demo/demo_08.png)

即使手动添加了`__proto__`对象，也和系统隐式的内置属性不是同一个，不具有`Object.prototype`的属性和方法。

## 补充知识点：`toString()`方法

`toString()`是原型的一个方法，通过调用返回字符串。

### `undefined`和`null`不能调用`toString()`方法

`undefined`和`null`不是对象就没有原型，所以无法使用`toString()`方法。

![toString()方法](/demo/demo_09.png)

### 数字调用`toString()`方法

先看一个例子

```javascript
123.toString();
```

我们发现会报错。

![toString()方法](/demo/demo_10.png)

为什么会这样呢？

因为`123.toString()`首先会被识别成浮点型，所以后面加`toString()`无法识别。

要使用`toString()`方法需要先定义一个变量，通过变量来调用。

```javascript
var num = 123;
num.toString(); //'123'
```

通过把`123`赋值给变量`num`，然后调用`toString()`方法输出字符串`'123'`。

我们知道原始值是无法直接调用原型`toString()`方法，这里其实是经过了一次包装类的转换。

```javascript
var num = 123;
// num.toString(); --> new Number(num).toString();
Number.prototype.toString = function() {}
Number.prototype.__proto__ = Object.prototype
```

`new Number()` 的原型是`Number.prototype`，原型`Number.prototype`有一个`toString()`方法；`Number.prototype`也有原型`Number.prototype.__proto__`对应的是`Object.prototype`，`Object.prototype`也具有`toString()`方法。

原型上有方法，自身又重新写了一个和原型同名功能不同的方法，叫做重写。

同名函数，重新方式不同。

调用时，如果是重写方法，会优先调用自身重写发方法。

所以上面`num.toString()`方法是调用了`Number.prototype.toString()`方法，如果调用`object.prototype.toString()`方法返回的结果就会不同。

类似的还有：
```javascript
Object.prototype.toString();
Number.prototype.toString();
Array.prototype.toString();
Boolean.prototype.toString();
String.prototype.toString();
```

再看一个好玩的例子：

```javascript
var obj = 123
document.write(obj); //123

obj = {};
document.write(obj); //[object Object]

obj = Object.create(null);
obj.toString = function() {
  return '老邓身体好！';
}
document.write(obj); //老邓身体好！
```

上面例子表面：通过`document.write(obj)`的打印结果，其实是`toString()`方法的返回结果，这也证明了`document.write()`调用的是`toString()`方法。

本节完。
