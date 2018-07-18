---
title: JavaScript学习笔记（11）- 原型
date: 2018-07-18 09:30:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/webcourse/index.html#course_id=231577&term_id=100273169&taid=1464734172022937&vid=a14198i8y2h)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。


## 原型的定义

> 原型是`function`对象的一个属性，它定义了构造函数构造出的对象的公共祖先。
> 通过该构造函数产生的对象，可以继承该原型的属性和方法。
> 原型也是对象。

```javascript
Person.prototype.lastName = 'Liu'; //原型是祖先
function Person () {}

var person1 = new Person();
var person2 = new Person();
console.log(person1.lastName); //Liu
console.log(person2.lastName); //Liu
```

原型描述的是一种继承关系，出生的时候就已经定义好了。

## 原型的特点

### 利用原型的概念和特点，可以提取对象的公有属性，统一定义。

```javascript
function Car (color) {
  this.carName = 'BMW';
  this.height = 1400;
  this.lang = 5200;
  this.color = color;
}

var car1 = new Car('red');
var car2 = new Car('green');
```

我们知道，通过构造函数构造对象时，可以把一些公用属性预先进行设置。

如果构造多个对象，那每次都要执行一次，这样显得比较冗余。这时把这些公共属性通过`prototype`这个原型对象来进行定义，这样就方便许多。

```javascript
Car.prototype.carName = 'BMW';
Car.prototype.height = 1400;
Car.prototype.lang = 5200;
function Car (color) {
  this.color = color;
}

var car1 = new Car('red');
var car2 = new Car('green');
```

更近一步，我们知道`prototype`是一个对象，那我们可以把对象属性统一通过`{}`来进行定义。

```javascript
Car.prototype = {
  carName = 'BMW',
  height = 1400,
  lang = 5200
}
function Car (color) {

}
```

### 通过构造对象无法修改原型的属性。

对象有增、删、改、查的功能，原型也同样具有。
但是不能通过构造函数构造的对象来修改原型的属性值。

```javascript
Person.prototype.lastName = 'Liu';
function Person (name) {
  this.name = name;
}

var person = new Person('laoda');
console.log(person.lastName); //Liu

person.lastName = 'Zhang';
console.log(person.lastName); //Zhang
console.log(Person.prototype.lastName); //Liu
Person.prototype.lastName = 'Wang';
console.log(person.lastName); //Zhang
console.log(Person.prototype.lastName); //Wang
```

我们可以看出：***如果构造函数的原型和构造对象相同属性都有值，则返回对象属性值。即使原型属性值发生变化，该对象属性值也保持不变。***

### 对象如何查看原型： 隐式属性`__proto__`

一个函数通过`new`申明成为一个构造函数，不管是否预先定义原型`prototype`的属性，`prototype`对象都已产生，可以通过隐式属性`__proto__`来进行查看。

![构造函数隐式属性](../../../../demo/demo_04.png)

### 如何查看对象的构造函数：构造器`constructor`

查看一个对象是由哪个构造函数构造的，可以通过原型属性`constructor`来进行查看。

```javascript
Person.prototype.lastName = 'Liu';
function Person () {}

var person1 = new Person();
var person2 = new Person();
console.log(person1.constructor); //function Person () {}
```

如果修改原型`prototype`的`constructor`属性值，会发生什么事情呢？
```javascript
Person.prototype.lastName = 'Liu';
function Person () {}

var person1 = new Person();
var person2 = new Person();
console.log(person1.constructor); //function Person () {}

function Car () {}
Person.prototype.constructor = Car;
console.log(person2.constructor); //function Car () {}
```

这样会导致构造器发生混乱，我们可不希望这样的事情发生。
