---
title: JavaScript学习笔记（14）- 继承发展史
date: 2018-07-31 10:00:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 继承的概念

> JavaScript中并没有真正意义上的继承，所谓的继承是利用不同方法来实现的。

## 继承的实现方法

### 传统形式：原型链

通过原型和原型链的方法来实现继承。

```javascript
Grand.prototype.lastName = 'Liu';
function Grand() {

}

var grand = new Grand();

Father.prototype = grand;

function Father() {

}
var father = new Father();

Son.prototype = father;
function Son() {

}

var son = new Son();
```

用原型链实现继承在使用这些方法时会产生一些额外的且不希望产生的影响，过多的继承了一些没用的属性。

这种方法很早就被废弃。

### 借用构造函数

```javascript
function Person(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}

function Student(name, age, sex, tel, grade) {
  Person.call(this, name, age, sex);
  this.tel = tel;
  this.grade = grade;
}

var studend = new Student('zhangsan', 18, male, 188, 201801);
```

这种方式是借用了`call()`或者`apply（）`方法来实现继承。这种方式不算标准的继承，因为无法继承借用构造函数的原型。

还有一个不太好的地方就是每次执行构造函数的时候还要多执行一个函数，造成效率浪费。

### 公有原型

```javascript
Father.prototype.lastName = 'Liu';
function Father() {

}

function Son() {

}

var father = new Father();
Son.prototype = Father.prototype;
var son = new Son();
```

打印`son.lastName`输出`Liu`，很好的实现了继承。

我们可以把上面的继承方法封装成一个方法，方便不同构造函数的调用。

```javascript
function inherit(Target, Origin) {
  Target.prototype = Origin.prototype;
}

```

通过原型共享实现继承，但对于继承的还是被继承的构造函数，不可以随便修改自己的原型，否则继承或者被继承的原型也会发生改变。

```javascript
Father.prototype.lastName = 'Liu';
function Father() {

}

function Son() {

}

function inherit(Target, Origin) {
  Target.prototype = Origin.prototype;
}
inherit(Son, Father);
var father = new Father();
var son = new Son();

Son.prototype.sex = 'male';

console.log(son.sex); //male
console.log(father.sex); //male
```

我们可以看到打印`father.sex`也会输出`male`，究其原因是因为`Father.prototype`受到`Son.prototype`的修改而发生变化。


### 圣杯模式

上例可看出：在共有原型中，对象试图改变自己原型的一些属性时将会影响到其他对象，这是我们不希望看到的情况。

我们希望：对象修改自己原型的一些属性，又保证不影响其他对象的原型。

这里就可以采用圣杯模式的方法来实现继承：

+ 通过创建一个构造函数`Temp`来充当中间层并共用原始构造函数`Origin`的原型；
+ 让目标构造函数`Target`的原型通过中间层构造函数原型链`new Temp()`来产生继承；

```javascript
function inherit(Target, Origin) {
  function Temp() {}
  Temp.prototype = Origin.prototype;
  Target.prototype = new Temp();
}
```

回到上例中：

```javascript
Father.prototype.lastName = 'Liu';
function Father() {

}

function Son() {

}

function inherit(Target, Origin) {
  function Temp() {}
  Temp.prototype = Origin.prototype;
  Target.prototype = new Temp();
}

inherit(Son, Father);
var father = new Father();
var son = new Son();

Son.prototype.sex = 'male';

console.log(son.sex); //male
console.log(father.sex); //undefined
```

`Son`修改原型`Son.prototype.sex = 'male'`并不会影响到`Father`的原型，从而达到我们期望的效果。

我们知道对象都有一个属性`constructor`来指向对象构造函数，我们看上例中`son`的`constructor`是谁呢？

![constructor属性](../../../../demo/demo_11.png)

```javascript
son.__proto__ --> new Temp().__proto__ --> Father.prototype
```

通过输出和分析我们得知：`son`的`constructor`是`function Father() {}`，这样在实际使用有些混乱，我们可以把这个构造器进行归位，同时记录其原始构造器以备后用：

```javascript
function inherit(Target, Origin) {
  function Temp() {}
  Temp.prototype = Origin.prototype;
  Target.prototype = new Temp();
  Target.prototype.constructor = Target;
  Target.prototype.owner = Father.prototype;
}
```

这样就完成了一个完整的圣杯模式的方法。

再看下`YUI`中圣杯模式的写法：

```javascript
var inherit = (function() {
  var Temp = function() {}
  return function(Target, Origin) {
    Temp.prototype = Origin.prototype;
    Target.prototype = new Temp();
    Target.prototype.constructor = Target;
    Target.prototype.owner = Father.prototype;
  }
}())

inherit(Son, Father); //调用
```

这里用到了闭包的一个作用：***可以实现封装，属性私有化***。

用立即执行函数和返回匿名函数来形成闭包，导致函数体中的`Temp`形成一个私有化变量。

通过圣杯模式很好的解决了对象的继承和避免原型相互依赖和影响的问题。

本节完。
