---
title: JavaScript学习笔记（9）- 立即执行函数
date: 2018-07-11 9:15:10
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

### 立即执行函数的定义

> 此类函数没有声明，在一次执行后即释放。
> 适合做初始化工作。针对初始化功能的函数，形式上这种函数执行后即被释放。

```javascript
(function test() {
  var a = 123,
      b = 234;
  console.log(a + b); //357
}())

console.log(abc); //abc is not defined
```

## 立即执行函数的书写形式

* (function () {}()) // W3C推荐写法
* (function () {})()

## 立即执行函数的执行

* 只有表达式才能被执行符号执行。

```javascript
function test() {
  var a = 123,
      b = 234;
  console.log(a + b); //357
}
test();
```

* 能被执行符号执行的函数名称会被自动忽略，被执行符号执行的表达式就会变成立即执行函数。

```javascript
var test = function () {
  var a = 123,
      b = 234;
  console.log(a + b); //357
}()
console.log(test); //undefined
```

* 变异的函数执行：通过在正常函数声明的前面添加`+`、`-`或`!`，后面添加`()`可变成立即执行函数。

```javascript
[+/-/!] function test() {
  var a = 123,
      b = 234;
  console.log(a + b); //357
}();
```

原理其实就是先把函数隐式转化为表达式并进行了执行。

示例：阿里的考题

```javascript
function test(a, b, c, d) {
  console.log(a + b + c + d);
}(1, 2, 3, 4);
```

上面的代码是否会报错？是否会执行？返回结果是什么？


再看下面一个例子：输出0到9的数字。

```javascript
function test() {
  var arr = [];
  for(var i = 0; i < 10; i ++) {
    arr[i] = function () {
      document.write(i + " ");
    }
  }

  return arr;
}

var myArr = test();
for(var j = 0; j < 10; j ++) {
  myArr[j]();
}
```

我们在浏览器查看结果，发现输出的是`10 10 10 10 10 10 10 10 10 10`。

到这是不是有点懵？为什么会是这样的结果呢？



答案就是因为这里产生了闭包。for循环会产生10个函数体并返回到数组arr里，但函数只有在下面的for循环输出的时候才会被依次执行。此时，函数体里`document.write(i + " ");`的`i`其实是公用testAO作用域链里的值10，所以每次输出的都是10。

那如何能够实现我们的要求呢？这里就要用到立即执行函数。

```javascript
function test(){
  var arr = [];
  for(var i = 0; i < 10; i ++) {
    (function (j){
      arr[j] = function () {
        document.write(j);
      }
    }(i))
  }

  return arr;
}

var myArr = test();
for(var j = 0; j < 10; j ++) {
  myArr[j]();
}
```

在for循环里添加一个带参数的立即执行函数，通过`i`的实参传给函数里的形参来执行。

这是一个很好的用立即执行参数来解救闭包的问题，关键点在于：

* 理解视觉上的误差：输出语句不是立即能够输出结果的，而是只有被执行后才能输出。
* 理解闭包产生的原因；
* 能够想到使用立即执行函数；
* 通过传参来解决数字在定义的时候能够保存并变现的问题。

接下来看一个实例：

```html
<ul>
  <li>a</li>
  <li>b</li>
  <li>c</li>
  <li>d</li>
</ul>
```

在上述列表中通过点击事件，打印出对应列的索引值。

基本思路：

* 把所有`li`标签进行遍历存储；
* 通过点击事件来创建一个函数来输出索引；
* 利用`for`循环来循环输出。

具体代码如下：

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>Page Title</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body,
    ul,
    li {
      padding: 0;
      margin: 0;
    }

    ul {
      list-style: none;
    }

    li {
      background-color: green;
      text-indent: 2em;
    }

    li:nth-of-type(2n) {
      background-color: purple;
    }
  </style>
</head>

<body>
  <ul>
    <li>a</li>
    <li>b</li>
    <li>c</li>
    <li>d</li>
  </ul>

  <script>
    var lis = document.getElementsByTagName('li');
    for (var i = 0; i < lis.length; i++) {
      lis[i].onclick = function () {
        console.log(i);
      }
    }
  </script>
</body>

</html>
```

输入结果如下：

![返回结果](../../../../demo/demo_01.png)

这并不是我们希望看到的，没有正确返回结果的原因和上一示例类似，都是因为循环体里产生了闭包函数，导致把循环体最后执行的结果进行了输出。

这里我们就要用到立即执行函数来进行改进。

```javascript
var lis = document.getElementsByTagName('li');
for (var i = 0; i < lis.length; i++) {
  (function (j) {
    lis[j].onclick = function () {
      console.log(j);
    }
  }(i))
}
```

这时再看返回结果，正式我们希望看到的！

![返回结果](../../../../demo/demo_02.png)

收工。
