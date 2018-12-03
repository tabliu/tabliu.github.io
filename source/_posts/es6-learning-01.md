---
title: ES6学习笔记（1）- 数组方法
date: 2018-11-30 10:40:00
tags:
  - code
  - javascript
  - note
  - ES6
---

## ES6中的数组方法

### `forEach()`

> 对数组进行遍历，不改变原数组；
> 无返回值。

应用场景：对给定的数组进行遍历输出

```js
var colors = ['red', 'blue', 'green']

// ES5 for循环遍历数组
for(i = 0; i < colors.length; i ++) {
  console.log(colors[i]);
}

// ES6 forEach方法
colors.forEach(function(color) {
  console.log(color);
})
```

说明：

* `forEach()`方法`()`中需要放一个迭代器函数，并需要有一个接收数据的参数；

* 迭代器函数可以直接在`forEach()`方法中定义，也可以外部定义后引用使用。

练习：遍历数组中的值并将值进行相加。

```js
var numbers = [1, 2, 3, 4, 5];
var sum = 0;

// ES5 for循环遍历数组
for (let i = 0; i < numbers.length; i++) {
  sum += numbers[i];
}

console.log(sum);

// ES6 forEach方法
numbers.forEach(function(num) {
  sum += num;
})

console.log(sum);
```

### `map()`

> 对给定数组进行处理并返回一个新数组；
> 不改变原数组值。

应用场景1：对给定数组中的值进行乘以2并返回一个新数组

```js
var numbers = [1, 2, 3];

// ES5 for循环遍历数组
var newNumbers = [];
for (let i = 0; i < numbers.length; i++) {
  newNumbers.push(numbers[i] * 2);
}

console.log(newNumbers);

// ES6 map方法
var esNumbers = numbers.map(function(num) {
  return num * 2;
})

console.log(esNumbers);
```

![map方法](/demo/es6_map.jpg)
(图片来源于米斯特吴的网络课程)

说明：

* `map()`方法`()`中同样需要放一个迭代器函数，并需要有一个接收数据的参数；

* `map()`方法必须使用`return`进行返回值；如果不给，返回的新数组中的值将是`undefined`。

应用场景2：对给定对象数组进行处理，将数组中指定的对象值存储到一个新数组中

```js
var cars = [
  {
    name: 'BMW',
    price: '60w'
  },
  {
    name: 'BENZ',
    price: '50w'
  },
  {
    name: 'JEEP',
    price: '40w'
  }
];
var carPrices = [];

// ES5 for循环遍历数组
for (let i = 0; i < cars.length; i++) {
  carPrices.push(cars[i].price);
}

console.log(carPrices);

// ES6 map方法
var esCarPrices = cars.map(function(car) {
  return car.price;
})

console.log(esCarPrices);
```

### `filter()`

> 通过对给定数组中的值进行过滤处理并将符合条件的返回到一个新数组；
> 不改变原数组值。

应用：给定一个数组，根据给定条件，将符合条件的值返回一个新数组

条件：1、类型是水果；2、有库存；3、价格不大于4；

```js
var products = [
  {
    name: 'apple',
    type: 'fruit',
    stock: 3,
    price: 5
  },
  {
    name: 'cucumber',
    type: 'vegetable',
    stock: 0,
    price: 15
  },
  {
    name: 'orange',
    type: 'fruit',
    stock: 13,
    price: 2
  },
  {
    name: 'celery',
    type: 'vegetable',
    stock: 3,
    price: 15
  }
];

// ES5 for循环遍历数组
var newProducts = [];
for (let i = 0; i < products.length; i++) {
  if (products[i].type === 'fruit' && products[i].stock > 0 && products[i].price <= 4) {
    newProducts.push(products[i]);
  }
}

console.log(newProducts);

//ES6 filter方法
var esNewProducts = products.filter(function(product) {
  return product.type === 'fruit' && product.stock > 0 && product.price <= 10;
})

console.log(esNewProducts);
```

![filter方法](/demo/es6_filter.jpg)
(图片来源于米斯特吴的网络课程)

说明：

* `filter()`方法`()`中同样需要放一个迭代器函数，并需要有一个接收数据的参数；

* `filter()`方法必须使用`return`进行返回值；如果不给，返回的新数组中的值将是`undefined`。

应用场景2：给定一个数组，将该数组中属性值满足另外一个数组或者对象的指定值的项返回一个新数组：

```js
var auto = {
  name: 'auto',
  created: 2010
}

var cars = [
  {
    name: 'BMW',
    price: '60w',
    dealer: 'auto'
  },
  {
    name: 'BENZ',
    price: '50w',
    dealer: 'mofa'
  },
  {
    name: 'JEEP',
    price: '40w',
    dealer: 'auto'
  }
];
var newCars = [];

// ES6 filter方法
var newCars = cars.filter(function(car) {
  return car.dealer === auto.name;
})

console.log(newCars);
```

### `find()`

> 在给定数组中找到符合条件的值即返回
> 不改变原数组值。

应用：给定一个数组，根据给定条件，将符合条件的值返回

```js
/**
* 给定一个数组，根据给定条件，将符合条件的值返回
*/

var users = [
  {
    name: 'Alex',
    age: '12'
  },
  {
    name: 'Bill',
    age: '20'
  },
  {
    name: 'Alex',
    age: '1'
  }
];


// ES5 for循环遍历数组
var user;
for (let i = 0; i < users.length; i++) {
  if (users[i].name === 'Alex') {
    console.log(i);
    user = users[i];
    break; // 阻止继续查找

  }
}

console.log(user);

//ES6 find方法

var user = users.find(function(user) {
  user.name === 'Alex'
})

console.log(user);
```

![find方法](/demo/es6_find.jpg)
(图片来源于米斯特吴的网络课程)

`find()`方法和`filter()`方法的区别：

* 相同点：都是通过给定调用函数条件对指定数组进行查找并返回，找不到返回`undefined`；

* 不同点：find()`方法返回的是该数组中符合调用函数条件的第一个值或者对象，找到即停止；`filter()`方法返回的是所有符合条件的值或者对象组成的新数组。

### `every()和some()`

> every()方法：在数组中根据给定条件对数组进行判定，如果所有条件都符合则返回真(`true`)，否则返回假(`false`)，相当于逻辑运算中的`&&与`；
> some()方法：在数组中根据给定条件对数组进行判定，只要有条件符合则返回真(`true`)，全部没有才会返回假(`false`)，相当于逻辑运算中的`||或`；
> 必须有返回值。

```js
/**
* 给定一个包含数字的数组，判断每个数字是否都能被2整除
*/

var numbers = [8, 4, 32, 37];
var every = false;
var some = false;

// ES5 for循环遍历数组
for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] % 2 != 0) {
    every = false;
  } else {
    every = true;
    some = true;
  }
}

console.log(every); // false
console.log(some); // true

// ES6 every()方法
every = numbers.every(function(number) {
  return number % 2 === 0;
})

console.log(every); //false

some = numbers.some(function(number) {
  return number % 2 === 0;
})

console.log(some); //true
```

![every方法](/demo/es6_every.jpg)
(图片来源于米斯特吴的网络课程)

![some方法](/demo/es6_some.jpg)
(图片来源于米斯特吴的网络课程)

`every()`方法和`some()`方法的区别：

* `every()`方法：一假即假；

* `some()`方法：一真即真；

* 一旦`every()`和`some()`方法确认返回值就会停止遍历数组。

应用：对注册页面，判断所有的input内容都不为空

```js
function Field(value) {
  this.value = value;
}

Field.prototype.validate = function() {
  return this.value.length > 0;
}

var username = new Field('shanyang'),
    email = new Field('shanyang@hotmail.com'),
    telphone = new Field('18888888888');

// ES5
console.log(username.validate() && email.validate() && telphone.validate());

// ES6 every()方法

var inputs = [username, email, telphone];
var inputsValidate = inputs.every(function(input) {
  return input.validate();
});

console.log(inputsValidate);
```

### `reduce()`

> 根据指定的回调函数将数组元素进行组合并生成单个值返回；
> 必须有返回值。

```js
/**
* 数组中数字进行累加
*/

var numbers = [8, 4, 32, 37];

// ES5 for循环遍历数组
var sum = 0;
for (var i = 0; i < numbers.length; i++) {
  sum += numbers[i];
}

console.log(sum);

// ES6 reduce()方法
var total = numbers.reduce(function(prev, number) {
  return prev + number
})

console.log(total);
```

说明：

* `reduce()`方法的语法：`arr.reduce(function(prev, item, index, array) { return prev }, initValue)`；

* `reduce()`方法中必须有两个返回值：

  + `prev`：初始项，必须。每次运算后将`prev`和`item`的运算赋给自身，最后作为返回值返回；

  + `item`：当前项，必须；

  + `index`：当前项的索引值，可选；

  + `array`：当前数组，可选；

  + `initValue`：初始项的初始值，可选。如果初始值为空，`prev`默认接收该数组的第一项的值为初始值，`item`变为该数组的第二项的值。

本节完。
