---
title: Vue 学习笔记(5) - 理解vue-router中的router、routes和$route
date: 2018-11-27 09:30:00
tags:
  - code
  - vue
  - note
---

> 用`Vue.js + Vue Router`创建单页应用，是非常简单的。在使用`Vue Router`过程中，我们经常会遇到`router`、`routes`和`$route`这些名称，对于初学者来讲很容易混淆，接下来，通过官方文档和实例一起学习下。

## `vue-router`的安装

> 可以通过引用或者`npm`下载的方式来安装

```bash
// 引入cdn地址
<script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>

// 通过npm安装
npm install vue-router
```

安装后需要引入和使用：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

## `vue-router`的使用

### 基本路由

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<div id="app">
  <h1>Hello, vue-router!</h1>
  <div class="router">
    <router-link to="/foo">Foo</router-link>
    <router-link to="/bar">Bar</router-link>
  </div>
  <router-view></router-view>
</div>

<script>
  // 定义或导入路由组件
  const Foo = { template: `<div class="foo">Foo</div>` }
  const Bar = { template: `<div class="bar">Bar</div>` }

  // 定义路由
  const routes = [
    {
      path: '/foo',
      component: Foo
    },
    {
      path: '/Bar',
      component: Bar
    }
  ]

  // 定义路由器，创建`router`实例，传`routes`配置
  const router = new VueRouter({
    routes: routes
  })

  // 创建跟实例并挂载路由器，通过`router`装载路由器
  const app = new Vue({
    router,
  }).$mount('#app')
</script>
```

### 动态路由

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<div id="app">
  <h1>Hello, vue-router!</h1>
  <div class="router">
    <router-link to="/user/lilei" exact>lilei</router-link>
    <router-link to="/user/lucii">lucii</router-link>
  </div>
  <router-view></router-view>
</div>

<script>
  // 定义或导入路由组件
  const User = { template: `<div class="user">{{ $route.params.username }}</div>` }

  // 定义路由
  const routes = [
    {
      path: '/user/:username',
      component: User
    },
    // 'router-link-active': 'active'
  ]

  // 定义路由器，创建`router`实例，然后传`routes`配置
  const router = new VueRouter({
    routes: routes
  })

  // 创建跟实例并挂载路由器，通过`router`装载路由器
  const app = new Vue({
    router,
  }).$mount('#app')
</script>
```

## `vue-router`中的几个概念

### `router`

`router`是`VueRouter`的实例。其本质是`Vue`实例下的一个对象属性，通过挂载在`Vue`使创建的路由在`Vue`使用。

常用`router`定义：

```js
const router = new VueRouter({
  mode: <String>, // 配置路由模式，可选值："hash" | "history" | "abstract"，默认值："hash" (浏览器环境) | "abstract" (Node.js 环境)
  base: <String>, // 应用的基路径，默认值: "/"
  routes: <Array>, // 路由定义
  linkActiveClass: <String>, // 全局配置 <router-link> 的默认“激活`class`类名”，默认值: "router-link-active"
  linkExactActiveClass: <String>, // 全局配置 <router-link> 精确激活的默认的`class`，默认值: "router-link-exact-active"
  scrollBehavior: <Function>, // 设置路由跳转的滚动行为
  ...
})
```

### `$route`

`$route`是单个的路由对象，表示当前激活的路由的状态信息，包含了当前`URL`解析得到的信息，还有`URL`匹配到的路由记录(`route records`)。

路由对象是不可变 (`immutable`) 的，每次成功的导航后都会产生一个新的对象。

路由对象出现在多个地方:

* 在组件内，即`this.$route`;

* 在`$route`观察者回调内

* `router.match(location)`的返回值

* 导航守卫的参数：

```js
router.beforeEach((to, from, next) => {
  // `to` 和 `from` 都是路由对象
})
```

* `scrollBehavior`方法的参数:

```js
const router = new VueRouter({
  scrollBehavior (to, from, savedPosition) {
    // `to` 和 `from` 都是路由对象
  }
})
```

`$route`包含的属性：

```js
$route = {
  path: <String>, // 对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"
  params: <Object>, // 一个`key/value`对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象
  query: <Object>, // 一个`key/value`对象，表示`URL`查询参数
  hash: <String>, // 当前路由的`hash 值 (带 #)`，如果没有`hash`值，则为空字符串
  fullPath: <String>, // 完成解析后的`URL`，包含查询参数和`hash`的完整路径
  matched: <Array>, // 包含当前路由的所有嵌套路径片段的路由记录
  name: <String>, // 当前路由的名称，如果有的话
  redirectedFrom: <String>, // 如果存在重定向，即为重定向来源的路由的名字。
}
```

## 参考文档

* [Router 构建选项](https://router.vuejs.org/zh/api/#router-%E6%9E%84%E5%BB%BA%E9%80%89%E9%A1%B9)
