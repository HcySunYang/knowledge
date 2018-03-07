## Vue 奇技淫巧

#### 使用 hook:* 事件监听子组件相应生命周期钩子的触发

一图胜千言：

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-06-code%E7%9A%84%E5%89%AF%E6%9C%AC.png)

#### 如何让所有的后代组件实例都能够访问由根组件注入的选项

比如 `Vuex` 注入 `store` 的选项，在所有后代组件中都可以通过 `this.$store` 访问，其实很简单：

```js
Vue.mixin({
  beforeCreate() {
    this.$store = this.$options.store || (this.$parent && this.$parent.$store)
  }
})
```

首先对于跟组件来讲，传递给 `Vue` 构造函数的所有选项(包括 `Vue` 提供的选项和自定义选项)，都是可以通过 `this.$options.xxx` 访问的，所以如果根实例对象想要访问这个选项，只需要将该选项添加到根实例的某个属性上即可：

```js
this.$xxx = this.$options.xxx
```

但是对于子组件，访问他们自身的 `this.$options.xxx` 是访问不到从根组件注入的选项的，但是可以通过访问之前在父组件实例上定义的属性简介访问：

```js
this.$xxx && this.$parent.xxx
```

这样，就使得根组件以及其所有后代组件，都会在他们的实例对象上添加对选项的引用，使得它们全部能够访问由根组件注入进来的选项。
