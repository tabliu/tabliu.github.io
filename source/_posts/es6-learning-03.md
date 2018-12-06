---
title: ES6学习笔记（3）- 箭头函数的使用
date: 2018-12-06 15:45:00
tags:
  - code
  - javascript
  - note
  - ES6
---

> ES6中提供了一种全新的函数写法叫箭头函数，接下来简单的介绍下关于箭头函数的使用。

## 箭头函数的定义

找了一圈也没有一个特别官方的定义，从字面意思就可以理解：使用箭头`=>`形式来进行函数使用。

## 箭头函数的使用

常规方式定义一个函数：

```js
let func1 = function() {
  console.log('Hello World!');
}
func1(); // Hello World!
```

使用箭头函数来定义：

```js
let func2 = () => {
  console.log('Hello World!');
}
func2(); // Hello World!
```

从形式上看，使用箭头函数的变化：

* 去掉了`function` 定义头；
* 在`()`和`{}`中间增加了箭头函数的定义符号`=>`。

## 箭头函数的作用

### 简化代码

使用箭头函数可以有效的简化代码，除了常规的可以省略`function`，在不同情况下有以下简化：

函数中有形参依然要放在`()`中，如果只有一个，`()`可以省略。

```js
/**
 * 多个形参
 *
 */

// 常规
let adder = function(num1, num2) {
  console.log(num1 + num2)
}
adder(1, 3); // 4

// ES6 arrow function
let adder = (num1, num2) => {
  console.log(num1 + num2)
}
adder(1, 3); // 4

/**
 * 一个形参
 *
 */

// 常规
let circularArea = function(r) {
  const pi = 3.14;
  console.log(pi * r * r);
}
circularArea(2); // 12.56

// ES6 arrow function
let circularArea = r => {
  const pi = 3.14;
  console.log(pi * r * r);
}
circularArea(2); // 12.56
```

如果函数中有多个表达式需要用`{}`括起来，如果只有一个，可以省略`{}`。

```js
/**
 * 只有一个表达式，省略括号
 *
 */

// 常规
let numbers = [1, 2, 3];
let newNumbers = numbers.map(function(num) {
  return num * 2;
})

console.log(newNumbers); //[2, 4, 6]

// ES6 arrow function
let numbers = [1, 2, 3];
let newNumbers = numbers.map(num => num * 2)

console.log(newNumbers); //[2, 4, 6]
```

### 不改变`this`指向

简单理解就是：箭头函数没有自己的`this`指向，它只是继承其父级作用域下的`this`指向，并且不会再被改变。

我们看一个实例：

```js
// 常规
let xiaoming = {
  age: 12,
  height: '160cm',
  info: function() {
    setTimeout(function() {
      console.log('我今年' + this.age + '岁了，身高是' + this.height);
    }, 1000);
  }
};

xiaoming.info(); // 我今年undefined岁了，身高是undefined
```

为什么上例中的结果会输出`undefined`呢？这是我们初学者经常搞不懂的地方，其原因是`setTimeout`方式是`window`上的方法，在使用时其作用域被指向了`window`全局，所以找不到在`xiaoming`这个对象里定义的`this.age`和`this.height`。

常规的解决方法是人为的把`this`的作用域进行保留指向：

```js
let xiaoming = {
  age: 12,
  height: '160cm',
  info: function() {
    let self = this;
    setTimeout(function() {
      console.log('我今年' + self.age + '岁了，身高是' + self.height);
    }, 1000);
  }
};
```

另外一种方法就是使用箭头函数。

```js
let xiaoming = {
  age: 12,
  height: '160cm',
  info: function() {
    setTimeout(() => {
      console.log('我今年' + this.age + '岁了，身高是' + this.height);
    }, 1000);
  }
};

xiaoming.info();
```

通过使用箭头函数，使`this`的很好的指向到了继承的父级对象中，避免`this`乱指向的问题。

## 箭头函数的缺陷

使用箭头函数带来了很多便利的地方，那有没有不好的地方呢？

还是回到上例中，细心的同学可能会问，为什么定义`info`方法还是用的`function`的方式，不换成箭头函数的形式呢？

其实我是换了的：

```js
let xiaoming = {
  age: 12,
  height: '160cm',
  info: () => {
    setTimeout(() => {
      console.log('我今年' + this.age + '岁了，身高是' + this.height);
    }, 1000);
  }
};

xiaoming.info();
```

这么执行后发现还是会输出`undefined`，百思不得其解。在网上冲浪中才了解到原来使用箭头换上定义方法后，对象属性的`()`无法封闭作用域，在找不到作用域情况下此时的`this`又指向到了全局。虽然对更深层的原因还不太理解，需要在未来的实践中继续探索，但一定要先记住：***不能用箭头函数定义对象的方法***。

还有其他场景下不适合使用箭头函数的，后继不断更新。

本节完。
