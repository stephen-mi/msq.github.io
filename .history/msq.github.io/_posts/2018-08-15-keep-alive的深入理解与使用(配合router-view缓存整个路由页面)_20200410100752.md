---
layout:     post
title:      keep-alive的深入理解与使用（配合router-view缓存整个路由页面）
subtitle:
date:       2018-08-15
author:     BY
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - vue
    - vue-router
    - javascript
---

> keep-alive的深入理解与使用

在搭建 vue 项目时，有某些组件没必要多次渲染，所以需要将组件在内存中进行‘持久化’，此时 <keep-alive> 便可以派上用场了。 <keep-alive> 可以使被包含的组件状态维持不变，即便是组件切换了，其内的状态依旧维持在内存之中。在下一次显示时，也不会重现渲染。

    PS：<keep-alive> 与 <transition>相似，只是一个抽象组件，它不会在DOM树中渲染(真实或者虚拟都不会)，也不在父组件链中存在，比如：你永远在 this.$parent 中找不到 keep-alive 。

## 1.keep-alive的基础使用

最基础的一般是结合动态组件去使用：
```
<keep-alive>
    <component :is="currentView"></component>
</keep-alive>

new Vue({
    el: '#app',
    data(){
        return {
            currentView: Test //Test为引入的子组件
        }
    }
})
```
不过此种方式并无太大的实用意义。

## 2.生命周期钩子

被包含在 <keep-alive> 中创建的组件，会多出两个生命周期的钩子: activated 与 deactivated

- activated

在组件被激活时调用，在组件第一次渲染时也会被调用，之后每次keep-alive激活时被调用。

- deactivated

在组件被停用时调用。

    注意：只有组件被 keep-alive 包裹时，这两个生命周期才会被调用，如果作为正常组件使用，是不会被调用，以及在 2.1.0 版本之后，使用 exclude 排除之后，就算被包裹在 keep-alive 中，这两个钩子依然不会被调用！另外在服务端渲染时此钩子也不会被调用的。

## 3.配合router-view使用

有些时候可能需要将整个路由页面一切缓存下来，也就是将 <router-view> 进行缓存。这种使用场景还是蛮多的。

在vue 2.1.0 之前，大部分是这样实现的：
```
<!-- template -->
<keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>

//router配置
new Router({
    routes: [
        {
            name: 'a',
            path: '/a',
            component: A,
            meta: {
                keepAlive: true
            }
        },
        {
            name: 'b',
            path: '/b',
            component: B
        }
    ]
})
```
这样配置路由的路由元信息之后，a路由的 $route.meta.keepAlive 便为 true ，而b路由则为 false 。
所以为 true 的将被包裹在 keep-alive 中，为 false 的则在外层。这样a路由便达到了被缓存的效果，如果还有想要缓存的路由，只需要在路由元中加入 keepAlive: true 即可。

## 4.在2.1.0版本之后

在vue 2.1.0 版本之后，keep-alive 新加入了两个属性: include(包含的组件缓存生效) 与 exclude(排除的组件不缓存，优先级大于include) 。

include 和 exclude 属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示。
当使用正则或者是数组时，一定要使用 v-bind !
```
<!-- 逗号分隔字符串，只有组件a与b被缓存。这样使用场景变得更有意义了 -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (需要使用 v-bind，符合匹配规则的都会被缓存) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- Array (需要使用 v-bind，被包含的都会被缓存) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>
```
有了include之后，再与 router-view 一起使用时便方便许多了:
```
<!-- 一个include解决了，不需要多写一个标签，也不需要在路由元中添加keepAlive了 -->
<keep-alive include='a'>
    <router-view></router-view>
</keeo-alive>
```

## 5.需要注意的地方

1.<keep-alive> 先匹配被包含组件的 name 字段，如果 name 不可用，则匹配当前组件 componetns 配置中的注册名称。
2.<keep-alive> 不会在函数式组件中正常工作，因为它们没有缓存实例。
3.当匹配条件同时在 include 与 exclude 存在时，以 exclude 优先级最高(当前vue 2.4.2 version)。比如：包含于排除同时匹配到了组件A，那组件A不会被缓存。
4.包含在 <keep-alive> 中，但符合 exclude ，不会调用activated 和 deactivated。

## 以上，致那颗骚动的心