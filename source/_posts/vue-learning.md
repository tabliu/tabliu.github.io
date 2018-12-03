---
title: Vue 学习笔记
date: 2018-03-12 11:12:58
tags:
  - code
  - vue
  - note
---

> Vue学习笔记和代码记录。

## 定义

> Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

以上是Vue的官网定义。

## 安装

### 直接引入链接

> 建议初学者使用

常用两种方式：

* 开发环境

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
 ```

* 生产环境

```html
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### 通过node.js的NPM安装Vue

### 通过node.js的NPM安装Vue-cli脚手架（推荐安装方式）

## 数据类型

* 字符串： `title : "hello world!"`；
* 数字： `num : 123`；
* 数组： `arrow : ["apple", "banana","orange"]`；
* 对象： `name : { firstName : "ming", lastName : "Li" }`；
* 布尔值： `flag ： true`；

## 实例化对象

```html
<body>
  <!-- app是根容器 -->
  <div id="app">
    <h1>{{ name }}</h1>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</body>
```

```js
new Vue({
  el : "#app",
  data: {
    name : "tabliu"
  }
})
```

说明：

* `el`：`element`需要获取的元素，一定是html中的根容器元素；
* `data`：用于数据的存储。

## 模版数据绑定

### 模版解析

使用 &#123;&#123; 数据 &#125;&#125; 对数据进行解析。

```html
<div id="app">
  {{ msg }}
</div>
<script>
    var app = new Vue({
        el : "#app",
        data : {
            msg : "Hello World!!"
        }
    })
</script>
```

数据是js的简单表达式，可以输出`data`中的数据，也可以是一个方法。

```html
<div id="app">
  {{ getFullName() }}
</div>
<script>
    var app = new Vue({
        el : "#app",
        data : {
            msg : "Hello World!!",
            age : 18,
            firstName : "tab",
            lastName : "liu"
        },
        methods : {
            getFullName : function() {
                return this.firstName + " " + this.lastName;
            }
        }
    })
</script>
```

`methods`：用户存储定义的方法。数据的方法可以直接使用`data`中的数据，也可以使用参数来调用。

```html
<div id="app">
  {{ getFullName(firstName, lastName) }}
</div>
<script>
    var app = new Vue({
        el : "#app",
        data : {
            msg : "Hello World!!",
            age : 18,
            firstName : "tab",
            lastName : "liu"
        },
        methods : {
            getFullName : function(firstName, lastName) {
                return firstName + " " + lastName;
            }
        }
    })
</script>
```

需要注意的是，如果使用ES6的语法，在`methods`中尽量避免使用箭头函数来写方法，因为箭头函数中`this`默认指向的是`window`，而不是`app`这个实例。

```html
<div id="app">
  {{ getFullName(firstName, lastName) }}
</div>
<script>
    var app = new Vue({
        el : "#app",
        data : {
            msg : "Hello World!!",
            age : 18,
            firstName : "tab",
            lastName : "liu"
        },
        methods : {
            getFullName : (firstName, lastName) => {
                console.log(this); // window
                return firstName + " " + lastName;
            }
        }
    })
</script>
```

### v-text：文本绑定

### v-html：标签绑定

> 双大括号会将数据解释为普通文本，而非 HTML 代码。

```html
<div id="app">
  <p v-html="site"></p>
</div>
<script>
    var app = new Vue({
        el : "#app",
        data : {
            msg : "Hello World!!",
            site : "<a href='http://iqianduan.com/'",
        }
    })
</script>
```

### v-model：双向数据绑定

* `v-model.lazy`：延迟对数据进行更新；
* `v-model.number`：对输入的数据字符串转为数字；
* `v-model.trim`：对数据进行裁剪，去除空格等

## 表单数据绑定

### checkbox：储存的数据类型是数组

### radio：储存的数据类型是字符串

### select：存储的数据类型是字符串

## 标签属性

### v-bind标签绑定属性

> 标签属性绑定，属于动态绑定，可以简写为：。
> 组成形式：`v-指令名称 + ":" + 指令参数 = 指令表达式`。
> 绑定后的指令表达式可以是字符串、数组或者是对象。

练习：

```html
<div id="app">
  <p>电影名称： {{ filmName }}</p>
  <p>上映日期： {{ filmDate }}</p>
  <p>该电影上映年份是{{ filmDate.split("-")[0] }}年，是一部{{ getDate() }}电影</p>
  <button v-bind:title="filmName">Hover Me</button>
</div>
<script>
    var app = new Vue({
        el : "#app",
        data : {
            filmName : "红海行动",
            filmDate : "2018-05-12",
        },
        methods : {
          getDate : function() {
            return this.filmDate.split("-")[0] > 2000 ? "新" : "老";
          }
        }
    })
</script>
```

### 事件绑定

> 通过用`v-on`指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。
> 事件处理方法可以是 JavaScript 表达式，也可以是定义好的方法。通过`v-on:event="eventName"`进行绑定，可简写为`@:event="eventName"`。
> 方法通过在`methods`里进行方法定义。

#### 事件绑定的方法

```html
<div id="app">
  <button v-on:click="msg += msg;">click me!!</button>
  <p>show msg: {{ msg }}</p>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    }
  })
</script>
```

```html
<div id="app">
  <button v-on:click="showMe">click me!!</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    },
    methods: {
      showMe: function() {
        alert(this.msg);
      }
    }
  })
</script>
```

说明：

* 如果是在模版标签中解析事件的方法，需要在方法名后加`()`来进行执行；
* 如果是在事件属性中可以省略，如果有传参曾需要添加。

#### 事件绑定的内联方法

绑定中添加的事件名可以是事件函数，也可以是事件函数的执行形式。如果添加的是函数执行模式，有形参的情况下也必须添加对应的实参。

```html
<div id="app">
  <button v-on:click="showMe('Hello World!!')">click me!!</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    methods: {
      showMe: function(msg) {
        alert(msg);
      }
    }
  })
</script>
```

我们经常会用到事件对象来进行操作，上例中添加了形参`event`，在实参中必须添加一个Vue中固定形式的`$event`来接收。

```html
<div id="app">
  <button v-on:click="showMe('Hello World!!', $event)">click me!!</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    methods: {
      showMe: function(msg, event) {
        console.log(event); //window
        alert(msg);
      }
    }
  })
</script>
```

#### 事件绑定修饰符

> 在`v-on:event.midiflyer`添加修改器，形式为`v-on:event.midiflyer="eventName"`。

在`v-on:event="eventName"`中添加`stop`可阻止事件冒泡。

```html
<div id="app">
  <div v-on:click="showMe($event)">
    <button v-on:click="showMe">click me!!</button>
  </div>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    },
    methods: {
      showMe: function(event) {
        console.log(event);
        alert(this.msg);
      }
    }
  })
</script>
```

在`v-on:event="eventName"`中添加`prevent`可阻止默认事件。

```html
<div id="app">
    <a href="http://www.baidu.com" v-on:click.prevent="showMe">click me!!</a>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    },
    methods: {
      showMe: function(event) {
        console.log(event);
        alert(this.msg);
      }
    }
  })
</script>
```

在`v-on:event="eventName"`中添加`capture`元素自身触发的事件先在此处理，然后才交由内部元素进行处理。

```html
<div id="app">
    <a href="http://www.baidu.com" v-on:click.capture="showMe">click me!!</a>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    },
    methods: {
      showMe: function(event) {
        console.log(event);
        alert(this.msg);
      }
    }
  })
</script>
```

在`v-on:event="eventName"`中添加`self`只当在`event.target` 是当前元素自身时触发处理函数。

```html
<div id="app">
    <a href="http://www.baidu.com" v-on:click.self="showMe">click me!!</a>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    },
    methods: {
      showMe: function(event) {
        console.log(event);
        alert(this.msg);
      }
    }
  })
</script>
```

自定义事件：v-on:diyEvent="eventName"，通过$emit来触发自定义事件。`methods: {my-function () {this.$emit("diyEvent"), 参数}}`；

#### 键盘按键修饰符

> 在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为`v-on`在监听键盘事件时添加按键修饰符。

```html
<div id="app">
    <input type="text" v-on:keyup.enter="showMe">
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg: "Hello World!!"
    },
    methods: {
      showMe: function(event) {
        alert(this.msg);
      }
    }
  })
</script>
```

由于键盘的键值不太方便记忆和使用，Vue为常用的键值提供了别名。

* `enter`
* `tab`
* `delete`
* `esc`
* `space`
* `up`
* `down`
* `left`
* `right`

可以通过全局 `config.keyCodes` 对象自定义按键修饰符别名：

```html
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

#### 系统修饰符

> 可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

* `ctrl`
* `alt`
* `shift`
* `meta`

> 注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。

### v-if/v-if-else-if/v-else：条件渲染，如果成立则执行，不成立则注销；

```html
<div id="app">
  <button v-on:click="error = !error">Toggle Error</dbuttoniv>

  <button v-on:click="succeed = !succeed">Toggle Succeed</button>
  <p v-if="error">网络连接错误：404</p>
  <p v-else-if="succeed">网络连接成功：200</p>
</div>
<script type="text/javascript">
  var app = new Vue({
    el: '#app',
    data: {
      error: false,
      succeed: false
    }
  });
</script>
```

### v-show：同样是条件渲染，不同的是不成立是隐藏而不是注销；

```html
<div id="app">
  <h1>v-if条件语句</h1>
  <button v-on:click="error = !error">Toggle Error</dbuttoniv>

  <button v-on:click="succeed = !succeed">Toggle Succeed</button>
  <p v-show="error">网络连接错误：404</p>
  <p v-show="succeed">网络连接成功：200</p>
</div>
<script type="text/javascript">
  var app = new Vue({
    el: '#app',
    data: {
      error: false,
      succeed: false
    }
  });
</script>
```

## 计算属性和数据监听

### methods事件属性

> 所有事件都在这里进行定义使用。

```html
<div id="app">
  <div class="num">{{ num }}</div>
  <div class="name">{{ getFullName() }}</div>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      num: 0,
      lastName : "liu",
      firstName : "tab"
    },
    methods: {
      getFullName : function() {
        return this.firstName + this.lastName;
      }
    }
  })
</script>
```

在模版用调用方法需要加上`()`来进行执行方法。

需要注意的是：很多情况下在执行事件时需要取消默认事件：`function(e){e.preventDefault()}`。

### computed属性

> 使用`computed`计算属性创建的是一个属性，模版调用的时候当作属性来调用。

我们将上述例子做一些调整：在执行`getFullName`方法时增加一个显示执行操作的提示。并且在模版中增加一个按钮，每次点击按钮，让`num`+1。

```html
<div id="app">
  <div class="num">{{ num }}</div>
  <div class="name">{{ getFullName() }}</div>
  <button v-on:click="num ++">add</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      num: 0,
      lastName : "liu",
      firstName : "tab"
    },
    methods: {
      getFullName : function() {
        alert("I do！")
        return this.firstName + this.lastName;
      }
    }
  })
</script>
```

我们发现：每点击一次按钮，`getFullName`方法就会被执行一次。这是因为按钮点击会触发模版渲染，在渲染的时候，Vue无法判断`getFullName`方法执行的内容，所以又会被执行一次避免有遗漏。

然后这样的方法并不是我们希望的，这时候就需要用到`computed`计算属性。

```html
<div id="app">
  <div class="num">{{ num }}</div>
  <div class="name">{{ fullName }}</div>
  <button v-on:click="num ++">add</button>
  <button v-on:click="changeName">change</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      num: 0,
      lastName : "liu",
      firstName : "tab"
    },
    computed : {
      fullName : function() {
        alert("I do！")
        return this.firstName + this.lastName;
      }
    },
    methods : {
      changeName : function() {
        this.firstName = "taigong";
        this.lastName = "yuchi"
      }
    }
  })
</script>
```

`computed`计算属性的优点：

* `computed`计算属性会分析创建的属性中调用的`data`中属性值是否发生改变。如果改变了就会执行该方法，如果没有改变就不执行。
* `computed`计算属性具有缓存机制。计算属性会缓存所依赖的那个值，如果下次在使用该属性，则不会重新取值。

`computed`计算属性的`get`和`set`方法

我们可以把计算属性定义成一个对象，该对象具有两个方法：`get`和`set`方法。

* `get`方法：获取定义的该属性值；
* `set`方法：设置改属性的值，需要一个参数来接收值。

```html
<div id="app">
  <div class="num">{{ num }}</div>
  <div class="name">{{ fullName }}</div>
  <button v-on:click="num ++">add</button>
  <button v-on:click="changeName">change</button>
  <button v-on:click="changeFullName">changeFullName</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      num: 0,
      lastName : "liu",
      firstName : "tab"
    },
    computed : {
      fullName : {
        get : function() {
          alert("get it！")
          return this.firstName + " " + this.lastName;
        },
        set : function(newValue) {
          var arr = newValue.split(" ");
          this.firstName = arr[0];
          this.lastName = arr[1];
        }
      }
    },
    methods : {
      changeName : function() {
        this.firstName = "taigong";
        this.lastName = "yuchi"
      },
      changeFullName : function() {
        this.fullName = "xiao hong";
      }
    }
  })
</script>
```

`computed`计算属性的使用场景

在耗时、大量搜索的情况下使用，减少dom重复渲染的性能支出。

### watch监听器

> 用来监听指定属性的数据变化，通过一个参数来接收。

```html
<div id="app">
  <div class="search">
    <input type="text" v-model="searchInfo">
  </div>
  <div class="text" v-if="showSearching">正在输入中...</div>
  <ul class="result">
    <li v-for="item in result">{{ item }}</li>
  </ul>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      searchInfo : "",
      showSearching : false,
      result : []
    },
    watch : {
      searchInfo : function(query) {
        this.showSearching = true;
        var vm = this;
        setTimeout(function() {
          vm.result = ["html", "css", "javascript"];
          vm.showSearching = false;
        }, 500);
      }
    }
  })
</script>
```

`watch`监听器和`computed`计算属性的区别

* 相同点：作用一样，都是用来监听一个属性值的变化；
* 不同点：
  1. `computed`监听的属性需通过`return`返回一个值，而`watch`不需要返回；
  2. `computed`必须是同步操作，不能进行异步操作；而`watch`都可以。

实例：把给出的数据按照要求进行格式化并输出，并可以点击按钮添加新的字段。

```html
<div id="app">
  <div class="moive">
    <ul class="list">
      <li v-for="item in fullMovies">{{ item }}</li>
    </ul>
    <button v-on:click="addMovie">add movie</button>
  </div>
</div>
<script>
  var app = new Vue({
    el: "#app",
    data: {
      movies: [{
          name: "捉妖记2",
          year: 2018
        },
        {
          name: "红海行动",
          year: 2017
        },
        {
          name: "速度与激情8",
          year: 2017
        }
      ]
    },
    computed : {
      fullMovies : {
        get: function() {
          var arr = [];
          for(var i = 0; i < this.movies.length; i ++) {
            arr.push(this.movies[i].name + "(" + this.movies[i].year + ")");
          }
          return arr;
        }
      }
    },
    methods : {
      addMovie : function() {
        this.movies.push({
          name : "妖猫传3",
          year : 2019
        })
      }
    },
    watch : {
      movies : function(newValue) {
        alert("我添加了" + newValue[newValue.length - 1].name);
      }
    }
  })
</script>
```

通过上述实例，有效的实践了`methods`事件属性、`computed`计算属性和`watch`监视器的使用。

### filters过滤器

> 用来对数据属性进行过滤操作并返回。
> 调用方法： &#123;&#123; property | filtersMethed &#125;&#125;。方法有一个可接受属性值的参数是必须的，另外还可加条件判断的参数，可选。
> 单个属性可以调用多个方法，按照顺序执行。

实例应用：对给定的字符串进行如下操作：

* 根据输入的字符串输出为大写字母；
* 如果输出的字符串中有空格，这输出的字符串空格前后的字符串首字母大写；
* 如果输出的字符串中有空格，去掉空格进行输出。

```html
<div id="app">
  <input type="text" v-model="msg">
  <p v-bind:title=" msg | uppperFirstWord ">{{ msg | upperCase }}</p>
  <p>{{ msg | uppperFirstWord | toSpace }}</p>
</div>
<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg : "hello world!!"
    },
    filters : {
      upperCase : function(val, isFirstWord) {
        var val = val.toString();
        if(isFirstWord) {
          return val.charAt(0).toUpperCase() + val.slice(1);
        } else {
          return val.toUpperCase();
        }
      },
      uppperFirstWord : function(val) {
        var val = val.toString();
        return val.split(" ").map(function(value) {
          return value.charAt(0).toUpperCase() + value.slice(1);
        }).join(" ");
      },
      toSpace : function(val) {
        var val = val.toString();
        return val.split(" ").join("");
      }
    }
  })
</script>
```

* 传值属性：`props: ["xx","xx"]`。父子组件如果需要传值，必须要在props里进行定义；
* 创建属性：`created:function(){}`。方法不需要手动调用，直接执行。

### 应用

#### 动态绑定style和class

> 用来操作元素的`class`列表和内联样式`style`。
> 通过`v-bind:style=""`或`v-bind:class=""`来进行属性绑定。
> 数据绑定的表达式可以是字符串、对象或者数组。

通过隐藏或者显示`class`来实现动态绑定。

```html
<div id="app">
  <div class="shape" v-bind:class="{c : isC}"></div>
  <div class="shape" v-for="shape in shapes" v-bind:class="{c : shape.flag, t : !shape.flag}"></div>
  <button v-on:click="changeClass">change it</button>
</div>
<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg : "hello world!!",
      isC : true,
      shapes : [
        {
          flag : true
        },
        {
          flag : false
        }
      ]
    },
    methods : {
      changeClass : function () {
        this.isC = !this.isC;
      }
    }
  })
</script>
```

通过改变`class`的值来动态绑定。

```html
<div id="app">
  <div class="shape" v-for="shape in shapes" v-bind:class="[shape.shape]"></div>
  <button v-on:click="changeClass">change it</button>
</div>
<script>
  var app = new Vue({
    el: "#app",
    data: {
      msg : "hello world!!",
      isC : true,
      shapes : [
        {
          shape : "c"
        },
        {
          shape : "t"
        }
      ]
    },
    methods : {
      changeClass : function () {
        this.isC = !this.isC;
      }
    }
  })
</script>
```

## 组件

### 组件定义

> 组件是可复用的Vue实例。
> 组成形式：`Vue.component(name, object)`。

```html
<div id="app">
  <my-component></my-component>
</div>
<script>
  Vue.component("my-component", {
    template: `<div>
      <h1>联系方式</h1>
      <p>联系电话：{{ tel }}</p>
    </div>`,
    data: function () {
      return {
        tel: "0123456789"
      }
    },
  })

  var app = new Vue({
    el: "#app"
  })
</script>
```

需要注意几点：

* `template`属性里的内容必须是一个独立的标签来包裹，否则会报错；
* `data`属性必须是一个`function`，并返回一个对象，这样可以保证每个组件的数据都是独立的；
* 模版组件需要Vue实例后把组件放在实例结构中才可以使用。

### 组件命名

* 不强制要求按照W3C规则进行命名，但最好遵循，使用`-`的命名形式，例如：`my-component`;
* 不管组件是大驼峰还是小驼峰，在模版引用的时候一律要转为中横线的命名方式。例如：组件为`comName`，引用时为：`<com-name></com-name>`；在传递属性时名称也同样。

### 组件注册

* 全局注册：`Vue.component("my-template", {template: "..."});`  html：`<my-template></my-template>`
* 局部注册：只在使用的场景进行注册。`var myTemplate = {template: "..."};  new Vue({..., components: {"my-template: myTemplate"}})`

### 组件模版解析

* 特殊标签下的模版需要注意，比如table、ol、ul、select等标签，使用`is`进行挂载。例如:`<table><tr is="my-tr"></tr></table>`;
* 推荐使用字符串模版：
  * `<script type="text/x-template">`；
  * javascript内联模版字符串；
  * vue组件；

* 组件中的data必须是函数。

### 组件的传值

* 父传子：`props`传值。传值时要注意命名的选择和使用，使用props使用的驼峰式明显需要转变为对应的中横线式。`Vue.component("my-template", {props: ["myMessage"],template: "..."}); <my-template   my-message="hello"></my-template>`
* 子传父：`$emit`自定义事件。子组件通过自定义事件进行发送信息，子组件触发事件，父组件进行监听；
* 父传子的DOM节点：`slot`插槽。父组件向子组件插入template模板，父子之间通过slot属性和name属性进行对应`<p slot="header">我是header</p><span slot="footer">我是footer</span>`；

### 组件组合

* 字面量语法和动态语法；
* 动态组件：利用 `:is = ""` 进行组件的动态绑定，外层可以用内置组件`keep-alive`来进行缓存；

### 组件总结

* 使用组件树设计项目，配置文件链接各个组件-命名转换，动态组件；
* 父组件向内传递属性-动态属性；
* 子组件向外发布事件；
* `slot`插槽传递模版 - 具名`slot`；

## 高级用法

### 动画：使用`transition` 内置组件，有css过渡和js过渡两种方式。

#### css过渡实现原理：给动画的不同阶段加上不同的class名称。

* 四个阶段：`v-enter/v-enter-active/v-leave/v-leave-active`；
* 使用：`<transition name="fade"></transition>`：`.fade-enter/.fade-enter-active/.fade-leave/.fade-leave-active`；
* 能够接受动画的元素有：`v-show/v-if/`动态组件加载；
* 通过`mode="out-in"/"in-out"`实现动画顺序；
* 对于多元素模版，如果使用的是同标签名，需要使用`key`来进行区分；

#### js过渡实现原理：通过定义不同的方法来实现动画。

不同方法名：

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"
  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled">
</transition>
```

### 自定义指令

* 方法：主要有两种：局部定义和全局定义。
* 使用：`inserted`和`bind`是指令的两个配置属性，属性值是一个函数，所以用`es6`语法。讲`inserted`函数，然后然后回到组件，处理`el`表示使用了指令的元素对象，还有一个`binding`对象，其中`binding.value`表示的是使用了指令元素的指令的值，可以是`json`，然后借这个`json`（里面放着css相关信息）所包含的数据来修改`dom`样式。

### 插件

* `vue-resource`： 发送`http`请求
* `vue-router`：前端路由
* 引入步骤：
  1. 入口`main.js`文件 `import ...  from '...'`插件
  2. `Vue.use(插件)`，不过在模块环境中应当始终显式调用`Vue.use() `;

## 相关概念

* MVC
* MVP
* MVVM

## 相关知识点

* Node.js
* NPM
* Mustache
* ECMAscript
* Javascript
* Ajax

## Vue全家桶

* vue.js
* vue-cli
* vue-router
* vue-axios
* vue-lazyload

## 打包工具

* Webpack
* Gulp
* Parcel

## 安装依赖

* Css-loader
* Sass-loader
* Vue-style-loader
* Superagent

## 踩坑

* 在开发过程中，如果修改了配置文件，需要重新启动，否则报错；
* 手写输入的拼写错误问题，一般会提示出来；
* 样式文件中的拼写错误，包括属性、值、图片名称，如果找不到也会报错，一般很难找到，所以出现报错的时候一定先要解决！！
* 使用webpack要进行loader依赖的安装；
* proxyTable 反向代理设置；
* 在组件中template节点下必须只有一个子节点；
* 如果采用webpack进行打包管理，如果数据中有需要在js里引用图片地址，需要使用`require()`的方式进行引用，否则不会被打包到静态文件目录里；
* 在组件使用时候先进行数据绑定；
* 在使用属性的时候一定记得要添加作用域，比如this；
* 在组件或者模版中使用sass或less文件，一定要在style标签上声明lang，否则报错没商量；
* 在组件开发过程中，如果需要传参一定记得定义并且在引用的地方调用；

## 参考资料

* [Vue官网](https://cn.vuejs.org/)
