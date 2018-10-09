---
title: Vue 学习笔记(2) - Vue-cli脚手架
date: 2018-09-07 10:30:00
tags:
  - code
  - vue
  - note
---

> Vue 学习笔记和代码记录。

## 定义

> Vue 官方提供的`CLI`，为单页面应用 (SPA) 快速搭建繁杂的脚手架。

## 特点

- 脚手架是通过`webpack`搭建的开发环境；
- 使用`ES6`语法；
- 打包和压缩为一个`js`文件；
- 项目文件在环境中编译，而不是浏览器；
- 实现页面自动刷新。

## 安装

```js
index.html -> main.js -> App.vue
```

## 组件传值

### 父传子：`props`

- 传值：`Number`、`String`、`Boolean`
- 传引用：`Array`、`Object`

如何传引用值？

### 子传父：`$emit`

引用多个子组件，相同的方法相互影响如何避免？

## 生命周期

### 生命周期的作用

- 了解过程，方便找错误；
- 了解需求，来确定实现过程定义在哪个环节；

### 生命周期的过程

- `beforCreate`: 组件实例化之前执行的函数，预加载的执行；
- `created`: 组件实例化完毕但页面还未显示，常用作请求数据；结束`beforeCreate`的动作；
- `beforeMount`：组件挂载前状态，页面仍未展示，但虚拟 Dom 已配置，挂载模版之前的操作；
- `mounted`：组件挂载完毕，页面 dom 生成，加载完毕；
- `beforeUpdate`：组件更新前，页面仍未更新，需要调用的钩子函数
- `updated`：组件更新后，此方法一旦执行，页面就显示。需要发生变化而调用的钩子函数；
- `beforeDestory`：组件销毁前执行的钩子函数方法；
- `destoryed`：组件销毁后的钩子函数方法；

## 路由参数

### 路由参数的作用

> 使用同一套模版，根据传过来的不同 ID 展示不同内容。

### 路由的实例化

过程：

- 安装路由：`npm install vue-router --save`；
- 引入路由：`import VueRouter form 'vue-router'`；
- 使用路由：`Vue.use(VueRouter)`
- 路由实例化：路由实例化有两种方式。

第一种：直接实例化：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      name: 'routerName',
      component: RouterName
    }
  ],
  mode: 'history'
});
```

第二种：引入实例化：

```js
const routes = [
  {
    path: '/',
    name: 'routerName',
    component: RouterName
  }
];

const router = new VueRouter({
  routes: routes,
  mode: 'history'
});
```

- 路由挂载：

```js
new Vue({
  el: '#app',
  router,
  render: h => h(App)
});
```

- 路由使用：路由使用可以有不同种方法：

第一种：使用`path`属性。

```html
const router = new VueRouter({
  routes : [
    {
      path: "/",
      name: "routerName",
      component: RouterName
    }
  ],
  mode: "history"
})

<router-link to="/routerName">菜单</router-link>
```

第二种：使用`path`属性结合动态属性绑定。

```html
const router = new VueRouter({
  routes : [
    {
      path: "/",
      name: "routerName",
      component: RouterName
    }
  ],
  mode: "history"
})

export default {
  data () {
    return {
      routerLink : "/routerName"
    }
  }
}


<router-link :to="{ routerLink }">菜单</router-link>
```

第三种：使用`name`属性，需要结合属性绑定来使用；

```html
const router = new VueRouter({
  routes : [
    {
      path: "/",
      name: "routerName",
      component: RouterName
    }
  ],
  mode: "history"
})

<router-link :to="{ name: 'routerName' }">菜单</router-link>
```

第四种：动态改变路由跳转。

```html
<template>
  <div class="home">
    <h1>Home</h1>
    <button class="btn btn-success" @click="goToRouter">Bingo</button>
  </div>
</template>

<script>
const router = new VueRouter({
  routes : [
    {
      path: "/",
      name: "routerName",
      component: RouterName
    }
  ],
  mode: "history"
})

export default {
  methods : {
    goToRouter : function () {
      // 跳转到上一次访问链接
      //this.$router.go(-1);

      // 跳转到指定的地址
      //this.$router.replace("/admin");

      // 跳转到指定的名称
      // this.$router.replace({name: "menu"});

      // 通过push进行跳转
      // this.$router.push({name: "menu"});
      //this.$router.push("/admin");
    }
  }
}
</script>
```

## 导航守卫

> `vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

以上内容来自[官网](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E5%AE%88%E5%8D%AB)。

### 全局守卫

可以使用 `router.beforeEach` 注册一个全局前置守卫。

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

参数说明：

- `to`：值为`router`，即将要进入的路由；
- `from`：值为`router`，当前导航正要离开的路由；
- `next`：值为`function`，一个回调函数

```js
router.beforeEach((to, from, next) => {
  // 判断当前页面是否是注册或者登录页面
  if (to.path == '/login' || to.path == '/register' || to.path == '/') {
    next();
  } else {
    alert('请先登录！');
    next('/login');
  }
});
```

### 全局后置钩子

> 和守卫不同的是，这些钩子不会接受 next 函数也不会改变导航本身。

```js
//后置钩子
router.afterEach((to, from) => {
  alert('after each!');
});
```

### 路由独享守卫

> 在路由配置上直接定义 `beforeEnter` 守卫。

```js
{
  path: "/admin",
  name: "admin",
  component : Admin
  beforeEnter: (to, from, next) => {
    //路由独享守卫
    alert("非登录状态无法登录，请先登录！");
    next("/login");
  }
}
```

### 组件内守卫

> 在路由组件内直接定义以下路由导航守卫。

共有三种模式：

第一种模式： `beforeRouteEnter`

```js
export default {
  data() {
    return {
      name: 'tabliu'
    };
  },
  beforeRouteEnter(to, from, next) {
    next(vm => {
      alert('Hello ' + vm.name + '!');
    });
  }
};
```

说明：

- 在渲染该组件的对应路由被 `confirm` 前调用；
- 不！能！获取组件实例 `this`；
- 因为当守卫执行前，组件实例还没被创建。

第二种模式：`beforeRouteLeave`

```js
export default {
  data() {
    return {
      name: 'tabliu'
    };
  },
  beforeRouteLeave(to, from, next) {
    if (confirm('确定要离开吗？') == true) {
      next();
    } else {
      next(false);
    }
  }
};
```

说明：

- 导航离开该组件的对应路由时调用；
- 可以访问组件实例 `this`。

第三种模式：`beforeRouteUpdate` (2.2 新增)

```js
export default {
  beforeRouteUpdate(to, from, next) {
    if (confirm('确定要改变吗？') == true) {
      next();
    } else {
      next(false);
    }
  }
};
```

说明：

- 在当前路由改变，但是该组件被复用时调用；
- 举例来说，对于一个带有动态参数的路径 `/foo/:id`，在 `/foo/1` 和 `/foo/2` 之间跳转的时候，由于会渲染同样的 `Foo` 组件，因此组件实例会被复用，而这个钩子就会在这个情况下被调用；
- 可以访问组件实例 `this`。

### 完整的导航解析流程

- 导航被触发；
- 在失活的组件里调用离开守卫；
- 调用全局的 `beforeEach` 守卫；
- 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)；
- 在路由配置里调用 `beforeEnter；`
- 解析异步路由组件；
- 在被激活的组件里调用 `beforeRouteEnter；`
- 调用全局的 `beforeResolve` 守卫 (2.5+)；
- 导航被确认；
- 调用全局的 `afterEach` 钩子；
- 触发 `DOM` 更新；
- 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。
