## React技术栈相关经验总结

#### 1、react-redux 与 react-router 配合的问题

我项目中使用的 `react` 版本是 `15.5.10`，`react-redux()` 的版本是 `3.7.2`，`react-router` 的版本是 `4.1.2`。

有用过 `redux` 的同学应该清楚，`redux` 和 `react` 没有一毛钱关系，所以就有了 `react-redux`，它是 `redux` 针对 `react` 的绑定库，更多的时候我们使用的都是 `react-redux`，其实我们是完全可以不用这个绑定库而直接使用 `redux` 的，我们使用 `store.subscribe()` 监听数据状态的变化，然后通过 `store.getState()` 获取最新的数据状态，然后使用最新的数据状态渲染组件，说上去简单，但要注意这涉及几个问题：

1、你要手动把 `state` 树以 `props` 的方式传递给根组件，根组件再层层传递给子组件。

2、你要还要把 `dispatch` 和 `Action Creators` 以 `props` 的形式传递给根组件，根组件再层层传递给子组件。

对于第一个问题其实还好，我们只需要这样：

```js
const state = store.getState()
<App {...state} />
```

上面的代码我们把数据状态以 `props` 的形式从根组件注入了进去，对于第二个问题我们可以类似于处理第一个问题一样：

```js
import action from './actions'
const state = store.getState()
<App {...state, ...action, dispatch: store.dispatch} />
```

上面的代码中，我们把 `Action Creators` 以及 `dispatch` 方法手动的注入给根组件，然后在 `App` 组件或者其子组件中可以手动触发：

```js
// 其中 changeList() 为一个 Action Creators
this.props.dispatch(this.props.changeList())
```

虽然能够实现需求，但这样做是有问题的，首先子组件感知到了 `redux` 的存在，因为你使用了 `dispatch`，这违背了 [容器组件和展示组件相分离](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) 的思想，使得 `react` 与 `redux` 强耦合，当这些组件拿到其他地方的时候未必可用，这就是我们为什么要用 `react-redux`，它能够让子组件感知不到 `redux` 的存在，并且提供了一些好用的方法来辅助你做 `state=>props`，`Action Creators=>props`。

使用 `react-redux` 之后，对根组件进行处理：

```js
const mapStateToProps = state => ({...state});

function mapDispatchToProps(dispatch) {
  return bindActionCreators({ ...action }, dispatch);
}

// 使用 connect 装饰 App 组件
const Root = connect(mapStateToProps, mapDispatchToProps)(App);
```

这样在 `App` 组件中，我们就可以直接调用 `Action Creators` 从而触发 `Action`：

```js
this.props.changeList()
```

上面这句代码，在 `react-redux` 环境中，它是在触发 `Action`，如果不在 `react-redux` 环境中，那就是在调用一个方法，所以上面的代码是可移植的，对于 `App` 本身来讲，对 `redux` 是无感知的。

的确，看上去很好，但是当我们的相中同时使用 `react-redux` 和 `react-router` 的时候，你要注意，你可能遇到这个问题：*路由变化了，但是路由匹配的组件并没有被渲染*。

这是因为 `react-redux` 实现了 `shouldComponentUpdate` 方法为你做了一些优化，所以当你URL改变的时候，对于子组件来讲，他们并没有接收到新的 `props`，所以并不会渲染，解决的方案就是采用 `withRouter()` 包装一下由 `content()` 装饰的容器组件即可：

```js
const Root = withRouter(connect(mapStateToProps, mapDispatchToProps)(App));
```

其原理简单来讲，就是当你的路由变化的时候，组件将被强制 `re-render`，可以在这里看到解释：[https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/redux.md](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/redux.md)

#### 2、react-router 4 实现按需加载 (待续......)