## Vue 和 React 使用方式的对标（待续...）

这部分内容主要是在使用上对标一下 `Vue` 和 `React`，比如 `State`、`Props`、`生命周期(Lifecycle Hooks)`、`渲染方式`、`组件的管理方式` 等方面。

#### 组件的创建方式

先看看两者如何创建组件：

首先对于 `Vue`，你只需要像引入 `jQuery` 一样引入 `Vue` 就行了，然后我们可以使用 `Vue.component` 注册全局组件：

```js
Vue.component('list', {
    // 组件选项
})
```

也可以使用 `Vue` 构造函数的 `components` 选项注册局部组件：

```js
new Vue({
    template: '<list></list>',
    components: {
        'list': {
            // 组件选项
        }
    }
})
```

但是你知道吗少年，你想像引入 `jQuery` 一样直接使用 `React`，不介入任何构建系统，让浏览器直接运行，可能有点麻烦，首先，你得先把下面这两个文件引入：

```js
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>
```

然后你是不是要创建组件啊少年？那你就老老实实再引入一个 `create-react-class.js`
然后你就可以这样去创建一个组件了：

```js
var MyComponent = createReactClass({
    render: function() {
        return <h1>Hello, {this.props.name}</h1>
    }
})
```

这还没完，为了能让浏览器直接运行代码，你就不能使用 `jsx` 语法，`React` 官方文档也告诉你了，反正 `jsx` 语法最终都被转换成函数，类似下面这样：

```html
<div>Hello {this.props.toWhat}</div>
// 上面的jsx转换为
React.createElement(Hello, {toWhat: 'World'}, null)
```

既然如此，那你直接写 `React.createElement(Hello, {toWhat: 'World'}, null)` 不就可以了.......也是日了狗了，要是都用这种语法去写一个视图，那程序员的寿命估计是要减半了。即使你可以通过引入其他垫片库来使你可以使用 `jsx` 语法，那你想象一下这么做了之后会不会出现性能问题？无论是引入文件的体积还是代码运行的性能。

所以没人傻到不让构建系统参与的情况下使用 `React`，但你完全可以无构建系统的情况下使用 `Vue`，所以如果你要做以下小东西，没必要搭建构建系统，那 `Vue` 是首选。

如果允许使用构建系统的话，那么两者都可以换个玩法，比如 `Vue` 可以使用单文件组件：

```js
<template>
这里是模板
</template>
<script>
export default {
    // 组件选项
}
</script>
<style scoped>
组件化的样式
</style>
```

上面的 `.vue` 单文件组件，在 `webpack` 配合 `vue-loader` 的情况下你真的只需要专心写你的视图，写你组件的逻辑就可以了，什么都不用关心。

`React` 则这样：

```js
import React from 'react'

export default class SomeComponent extends React.Component {
    // 组件的逻辑
}
```

在上面的对比中大家要注意一点，在 `Vue` 中，无论你使用什么方式创建组件，组件的选项都是一模一样的，但是 `React` 则不然，比如 `React` 在使用 `ES6` 语法创建组件的时候，初始化状态要这么写：

```js
export default class SomeComponent extends React.Component {
    state = {
        a: 1
    }
}
```

但是如果你不使用 `ES6`，你就要使用 `getInitialState` 方法：

```js
var MyComponent = createReactClass({
    getInitialState: function () {
        return {
            a: 1
        }
    }
})
```

除此之外，在 `React` 中使用 `ES6` 和不使用 `ES6` 还有很多地方是不一样的，比如 `声明默认 Props` 的方式也不同，给事件绑定函数时要注意 `this` 的问题等等，具体看[官方文档吧](https://facebook.github.io/react/docs/react-without-es6.html)

然后再强调一下：在 `Vue` 中，无论你怎么写，写法都只有一种：`Only one`。

另外还应该强调一点的是：`Vue` 单文件组件还对 `CSS` 进行了组件化管理，使用 `scoped`。同时单文件组件允许你使用其他模板语言来写模板，允许你使用css预处理器来写样式。

吐槽一下，你看看 `React` 设计的，写个组件还得继承一下 `React.Component`，继承你妹啊继承，能不能设计的清爽一点，越看越不顺眼。

#### State

再来看看两者的数据状态。

在 `React` 中，`State` 是组件自身的数据状态，其实在 `Vue` 中也是，两者的写法略有不同：

`React` 如下：

```js
export default class Xxxx extends React.Component{
    state = {
        a: 1
    }
}
```

或者：

```js
export default class Xxxx extends React.Component{
    constructor (props) {
        super(props)
        this.state = {
            a: 1
        }
    }
}
```

当然，非 `ES6` 语法的这里就不写了。。。。

对于 `Vue`，很简单：

```js
export default {
    data () {
        return {
            a: 1
        }
    }
}
```

在 `data` 方法中返回一个对象就可以了，这就是数据。之前我们也谈过，无论采用何种方式使用 `Vue`，你就都这么写就完了。

我们知道，数据变化最终会反应到视图上，我们的应用基本都在和数据打交道，所以很多场景下，我们都要改变数据，进而改变视图，在 `React` 中，改变数据你要使用 `setState()` 方法：

```js
this.setState({
    a: 3
})
```

当我们的对象很复杂的时候，比如：

```js
{
    obj: {
        a: {
            b: 1
        }
    }
}
```

那你需要这样 `setState`：

```js
this.setState({
    obj: {
        a: {
            b: 3
        }
    }
})
```

想象一下，比上面复杂的数据多了去了，你这么 `setState` 的话，会死人的。所以 `React` 让你使用这玩意：[immutability-helper](https://github.com/kolodny/immutability-helper)。行了，去学吧，学完了赶紧去用。

在 `Vue` 中呢？你只需要这样：

```js
obj.a.b = 3
```

对，就这么简单。不过导致两者在设置状态方便会有这么大差异的原因有两个：

第一：两者触发视图更新的方式不一样，`Vue` 底层做了数据劫持，当你改变数据之后，`Vue` 会重新 `re-render` 然后通过 `Virtual dom` 的 `diff` 算法来跟新视图。也就是说你什么都不用做，只管改变数据就好了。而 `React` 是函数式的思想，状态与视图是一种函数关系：

```js
view = f(state)
```

所以你要是显示的调用 `setState` 并将新的数据传递给该函数，然后 `React` 使用 `Virtual dom` 的 `diff` 算法来跟新视图。

那这到底是好还是不好呢？网上有很多喷 `setState` 的文章，其中更是有业界小有名气的人。我也与周围的开发者朋友讨论过，只能说：口碑不好。

那我说一下我个人的看法：看见就想吐。

