---
title: JavaScript学习笔记（20）- 错误捕捉
date: 2018-10-09 09:00:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## try...catch...

> 有时候代码的错误无法把控，或者一些未知情况下会出现错误。在这些情况下，需要多代码进行错误捕捉。
> 使用try...catch...可以对错误进行捕捉并抛出。

```javascript
  try {
    console.log('a');
    console.log(b);
    console.log('c');
  } catch(e) {
    console.log(e);
  }
    console.log('d');
```

以上代码的执行结果是：

```javascript
a
ReferenceError: b is not defined
d
```

说明：

* 在`try`里的代码如果发生错误，不会再执行try里错误代码后面其他的代码；
* 如果`try`里的代码不发生错误，`catch`里的代码不会执行；如果`try`里代码产生错误，`try`里其他代码停止执行，直接执行`catch`里的代码。
* `catch`里的代码执行完毕后，`catch`后面的代码正常执行。
* `catch(e){...}`代码里的形参`e`表示一个`error`对象。包括两个属性：`error.name`和`error.message`。

## 常见的错误名称(`error.name`)

* `EvalError`：`eval()`的使用与定义不一致
* `RangeError`：数值越界
* `ReferenceError`：非法或不能识别的引用数值
* `SyntaxError`：发生语法解析错误
* `TypeError`：操作数类型错误
* `URIError`：URI处理函数使用不当

最常见的错误类型包括：`ReferenceError`、`SyntaxError`和`TypeError`。

本节完。
