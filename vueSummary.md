## vue知识点总结

### vuex的核心原理

一张图先来介绍vuex的工程流程

![vuex](../imgs/08123fdd96274e23a203ae7a1c67f798.jpeg)

先来理解vue.use。
相信很多人在使用第三方组件的时候会用到Vue.use()。这里先给出官方解释：
>安装Vue.js插件，如果插件是一个对象，必须提供install方法，如果插件是一个函数，它会被作为install方法。install方法调用时，会将Vue作为参数传入。

所以我们在理解vuex时可以先关注vuex的install方法实现，其核心代码如下：
```
Vue.mixin({beforeCreate:vueInit})
```

vueInit的核心代码
```
this.$store = typeof options.store === 'function'
? options.store()
: options.store
```

显然，vuex利用了Vue的mixin机制，混合beforeCreate钩子将 store 注入至 Vue 组件实例上，并注册了 Vuex store 的引用属性 $store。

Vuex 的 state 是借助 Vue 的响应式 data 实现的。 getter 是借助 Vue 的计算属性 computed 特性实现的。 