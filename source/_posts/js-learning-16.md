---
title: JavaScript学习笔记（16）- 对象的应用
date: 2018-08-03 08:58:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 属性的表示方法

### obj.prop

```javascript
var obj = {
  name : 'abc'
}

console.log(obj.name); //'abc'
```

### obj[prop]

这里的`prop`是属性名，是字符串形式，使用时要用`''`括起来。

```javascript
var obj = {
  name : 'abc'
}

console.log(obj['name']); //'abc'
```

使用`obj.prop`和`obj[prop]`都可以进行属性表示，而且其实是隐式进行了转换：

```javascript
//obj.prop --> obj['prop']
```

所以其实直接使用`obj[prop]`运行起来更快，不需要隐式转化。

使用`obj[prop]`还有一个很重要的作用就是属性拼接。

```javascript
var deng = {
  wife1 : 'xiaozhang',
  wife2 : 'xiaowang',
  wife3 : 'xiaoli',
  wife4 : 'xiaozhao',
  sayWife : function(num) {
    return this['wife' + num];
  }
}

console.log(deng.sayWife(1)); //'xiaozhang'
```

## 对象的枚举

### 枚举的定义

> 在数学和计算机科学理论中，一个集的枚举是列出某些有穷序列集的所有成员的程序，或者是一种特定类型对象的计数。这两种类型经常（但不总是）重叠。 [1]  是一个被命名的整型常数的集合，枚举在日常生活中很常见，例如表示星期的SUNDAY、MONDAY、TUESDAY、WEDNESDAY、THURSDAY、FRIDAY、SATURDAY就是一个枚举。

以上是百科里对枚举的定义，比较枯燥难懂。通俗的讲枚举就是遍历，对一组特定的数据集里的数据进行逐个访问或者其他操作的过程。

我们看一个简单的例子：

```javascript
var arr = [1,4,4,55,66];
for (var i = 0; i < arr.length; i++) {
  console.log(arr[i]); //1,4,4,55,66
}
```

我们对定义的一个数组逐个访问数组内部的值的过程就是枚举。

那如果要遍历一个对象，该如何操作呢，接下来我们继续。

### 对象的枚举方法(for...in...)

```javascript
var xiaowang = {
  name : 'abc',
  age : 23,
  sex : 'male',
  height : 172,
  weight: 72
}

for(var prop in xiaowang) {
  console.log(xiaowang.prop);
}

```

我们看下打印结果：

![对象属性](/demo/demo_12.png)

输出全部都是`undefined`，为什么呢？

我们刚才上面讲了一个知识点：使用`obj.prop`和`obj[prop]`都可以进行属性表示，而且其实是隐式进行了转换：

```javascript
//obj.prop --> obj['prop']
```

再回到本例中，我们看下隐式转换关系：

```javascript
//xiaowang.prop --> xiaowang['prop']
```

其实是执行了5次`console.log(xiaowang['prop']);`，因为`prop`没有值，所以返回`undefined`。换成`console.log(xiaowang[prop]);`就可以正常执行了。

```javascript
var xiaowang = {
  name : 'abc',
  age : 23,
  sex : 'male',
  height : 172,
  weight: 72
}

for(var prop in xiaowang) {
  console.log(xiaowang[prop]);
}

```

这才是我们期望的结果：

![对象属性](/demo/demo_13.png)

总结：

* 使用for...in...循环，其实就是遍历，通过对象的属性个数来控制循环圈数，有多少个属性就循环多少圈；
* `var prop`是变量定义，可以放在外面，同时`prop`可以其他名称来命名；
* `for(prop in obj)`在循环每一圈时，他会把对象的属性放在`prop`里，想遍历那个对象就是`in`谁，这里是固定的；
* 建议输出变量属性时用`obj[prop]`这种形式来写，避免不必要的错误。

## 判断对象的属性(in)

判断一个属性是否在对象中，可以使用`in`方法来操作，返回一个布尔值。

```javascript
console.log('prop' in obj); // true or false

```

```javascript
var xiaowang = {
  name : 'abc',
  age : 23,
  sex : 'male',
  height : 172,
  weight: 72
}

console.log('name' in xiaowang); //true
console.log('lastName' in xiaowang); //false
```

通过`in`方法可判断这个对象`obj`身上所有的属性。这个方法使用没有什么实际意义，所以很少应用。

## 判断对象的自身属性(hasOwnProperty)

```javascript
var xiaowang = {
  name : 'abc',
  age : 23,
  sex : 'male',
  height : 172,
  weight: 72,
  __proto__: {
    lastName : 'Liu'
  }
}

for(var prop in xiaowang) {
  console.log(xiaowang[prop]);
}

```

使用for...in...循环有一个潜在的问题，`prop`会把这个对象`obj`身上所有自设的方法都进行输出，包括自己的和继承原型和原型链上的，但是不会把系统的属性进行打印。

如果我们不希望把原型上的属性输出，只希望输出自身的属性，就要用到`hasOwnProperty`这个方法。

```javascript
var xiaowang = {
  name : 'abc',
  age : 23,
  sex : 'male',
  height : 172,
  weight: 72,
  __proto__: {
    lastName : 'Liu'
  }
}

for(var prop in xiaowang) {
  if(xiaowang.hasOwnProperty(prop)) {
    console.log(xiaowang[prop]);
  }
}

```

总结：
* `hasOwnProperty`方法可以判断属性是自身的还是继承来在原型，任何一个对象里都有这个方法可以使用。
* 使用`hasOwnProperty`方法需要传一个字符串形式的参数进来，返回布尔值。

## 判断对象的方法(instanceof)

`instanceof`的操作用法和`in`类似，但作用完全不同。他是判断一个对象是否输出给定的构造对象并返回一个布尔值。

相当于`A instanceof B`。意思是：A对象是不是B构造函数构造出来的。

判断方法：***看对象的原型链上有没有目标构造函数的原型***。

```javascript
function Obj() {

}

function Person() {

}

var person = new Person();
console.log(person instanceof Obj); //false
console.log(person instanceof Person); //ture
```

如果给定一个变量，其值可能是数组或者对象，如何进行判断？

第一种方法：使用`constructor`返回构造函数来看。

```javascript
var arr = [];
var obj = {};
console.log(arr.constructor); //Array 数组
console.log(obj.constructor); //Object 对象
```

第二种方法：使用`instanceof Array`看返回结果。

```javascript
var arr = [];
var obj = {};
console.log(arr instanceof Array); //true 数组
console.log(obj instanceof Array); //false 对象
```

第三种方法：使用`Object.prototype.toString()`方法来调用。

```javascript
var arr = [];
var obj = {};
arr.toString(); //""
obj.toString(); //"Object"

//相当于
Object.prototype.toString = function() {
  //第一步：识别this指向
  //第二步：返回相应的结果
}
```

我知道`toString()`方法针对不同类型的对象会返回不同结果，方法定义如下：

```javascript
//相当于
Object.prototype.toString = function() {
  //第一步：识别this指向
  //第二步：返回相应的结果
}
```

我们可以利用`call()`方法来改变`this`的指向，这样就达到了判别不同类型数据的类型了。

```javascript
Object.prototype.toString.call([]); //Array
Object.prototype.toString.call({}); //Object
Object.prototype.toString.call(123); //Number
```

本节完。
