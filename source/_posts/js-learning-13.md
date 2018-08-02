---
title: JavaScript学习笔记（13）- call和apply
date: 2018-07-30 18:18:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## Call

### Call()方法的定义

官方定义：

> 调用一个对象的一个方法，以另一个对象替换当前对象。

这么看非常难懂，怎么理解`call()`的作用呢？

先看一个小例子：

```javascript
function test() {

}

test()
```

上例中，`test()`其实的执行相当于`test.call()`。也就是说，`test()`的执行，其实是调用了该函数的`call()`方法。

任何一个方法都可以执行`call()`，`call()`才是方法执行的真实面目。

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

var person = new Person('deng', 100);
var obj = {}
Person.call();
```

这里执行`Person.call()`和`Person()`没有区别，但`Person.call()`括号里可以传东西。

如果`Person.call(obj)`，里面的`call`让`person`所有的`this`都指向变成了`obj`。但如果不执行`new`的话，`this`指向还是默认的`window`。

`call`的使用必须要`new`，`call`的第一个参数用于改变`this`的指向，第二位参数（对应第一个形参）及后面的参数都当作正常的参数，传到形参里面去。

### Call()方法的作用

通俗点说，`Call()`方法的作用：***用于改变this的指向***。

最主要的一个应用就是：***借用别人的方法，实现自己的功能***。

```javascript
function Person(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}

function Student(name, age, sex, tel, grade) {
  this.name = name;
  this.age = age;
  this.sex = sex;
  this.tel = tel;
  this.grade = grade;
}

var studend = new Student('zhangsan', 18, male, 188, 201801);
```

上例中有两个构造函数，可以分别用于构造`Person`和`Student`对象。但我们发现`Person`里其实是包含了`Student`中的一些属性，那`Student`是否可以借用`Person`来构造自己的对象呢？

我们可以使用`call`来改变`this`的指向。

```javascript
function Person(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}

function Student(name, age, sex, tel, grade) {
  // this.name = name;
  // this.age = age;
  // this.sex = sex;
  //var this = {name : '', age : '', sex : ''}
  Person.call(this, name, age, sex);
  this.tel = tel;
  this.grade = grade;
}

var studend = new Student('zhangsan', 18, male, 188, 201801);
```

`Person.call(this, name, age, sex)`里面的`this`现在是`new`了以后的`var this = {name : '', age : '', sex : ''}`。利用`Person`方法，实现了`Student`自己的封装。

但要注意：这里只能是你的需求完全涵盖别人的时候才可用，如果不想要其中某个参数，就不太适合使用这种方法。

### Apply()方法的定义和作用

`Apply()`和`Call()`一样，都是函数非继承而来的方法。其作用也相同：***用于改变`this`的指向***。

一般来说，`this`总是指向调用某个方法的对象，但是使用`call()`和`apply()`方法时，就会改变this的指向。

### Call()和Apply()方法的不同点

用一句话概括：***接收参数的方式不同***。

`Call()`方法：第一个参数是函数运行的作用域(`this`)，后面是传递给函数的参数全部列出来。

语法：`call(thisObject, arg1, arg2, ..., argn);`

`apply()`方法：接收两个参数，一个是函数运行的作用域（this），另一个必须是一个参数数组（arguments）。

语法：`apply(thisObject, [arg1, arg2, ..., argn]);`

看一个实例：

```javascript
function Model(height, width, length) {
  this.height = height;
  this.width = width;
  this.length = length;
}

function Sit(sitColor, sitStyle) {
  this.sitColor = sitColor;
  this.sitStyle = sitStyle;
}

function Wheel(wheelSize, wheelStyle) {
  this.wheelSize = wheelSize;
  this.wheelStyle = wheelStyle;
}

function Car(height, width, length, sitColor, sitStyle, wheelSize, wheelStyle) {
  // call()
  Model.call(this, height, width, length);
  Sit.call(this, sitColor, sitStyle);
  Wheel.call(this, wheelSize, wheelStyle);

  //apply()
  Model.apply(this, [height, width, length]);
  Sit.apply(this, [sitColor, sitStyle]);
  Wheel.apply(this, [wheelSize, wheelStyle]);
}

var car = new Car(1800, 2100, 4900, 'red', '真皮座椅', '18寸', '运动型');
```

总结：

`Call()`方法：需要把实参按照形参的个数传入；

`apply()`方法：需要传一个arguments数组。

本节完。
