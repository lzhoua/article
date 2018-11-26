### vue-router 中 name 的用法
> 我们常用vue.js和vue-router来创建单页应用，vue-router能很方便的管理所有的单页组件。我们在定义每个路由的时候会有一个name的属性（如下面代码），通常我们不定义这个属性发现也没有任何问题，那么这个name到底有什么用呢？

```javascript
export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'Hello',
      component: Hello
    }, {
      path: '/text',
      name: 'text',
      component: text
    }, {
      path: '/text/:id',
      component: param
    }
  ]
})
```
-------
- 第一种用法：

> 通过name属性，为一个页面中不同的router-view渲染不同的组件,如：将上面代码的Hello渲染在 name为Hello的router-view中，将text渲染在name为text的router-view中。不设置name的将为默认的渲染组件。

```javascript
<template>
  <div id="app">
     <router-view></router-view>
     <router-view  name="Hello"></router-view> //将渲染Hello组件
     <router-view  name="text"></router-view>   //将渲染text组件
  </div>
</template>
```
- 第二种用法：

>  使用$router.name获取组件name值

```javascript
 <template>
  <div id="app">
    <p>{{ $route.name }}</p> //可以获取到渲染进来的组件的name值
    <router-view></router-view>
  </div>
</template>
```
- 第三种用法：

> 页面渲染时传递参数

```javascript
<template>
  <div id="app">
    //向name为hello的组件传参数id，值为12
    <router-link ：to="{name:'hello', params:{id: '12'}}">hello</router-link> 
    <router-view></router-view>
  </div>
</template>
      