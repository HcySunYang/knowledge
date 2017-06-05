## 异步处理（待续......）

#### 案例简介

主流的异步处理方案主要有：回调函数(CallBack)、Promise、Generator函数、async/await。这一小节，我们通过一个小例子，对比这几种异步处理方案的不同。

###### 回调函数(CallBack)

假设我们有一个 `getData` 方法，用于异步获取数据，第一个参数为请求的 `url` 地址，第二个参数是回调函数，如下：

```js
function getData (url, callBack) {
    // 模拟发送网络请求
    setTimeout(() => {
        // 假设 res 就是返回的数据
        var res = {
            url: url,
            data: Math.random()
        }
        // 执行回调，将数据作为参数传递
        callBack(res)
    }, 1000)
}
```

我们预先设定一个场景，假设我们要请求三次服务器，每一次的请求依赖上一次请求的结果，如下：

```js
getData('/page/1?param=123', (res1) => {
    console.log(res1)
    getData(`/page/2?param=${res1.data}`, (res2) => {
        console.log(res2)
        getData(`/page/3?param=${res2.data}`, (res3) => {
            console.log(res3)
        })
    })
})
```

通过上面的代码可以看出，第一次请求的 `url` 地址为：`/page/1?param=123`，请求结果为 `res1`。

第二个请求的 `url` 地址为：`/page/2?param=${res1.data}`，依赖第一次请求的 `res1.data`，返回结果为 `res2`。

第三次请求的 `url` 地址为：`/page/3?param=${res2.data}`，依赖第二次请求的 `res2.data`，返回结果为 `res3`。

由于后续请求依赖前一个请求的结果，所以我们只能把下一次请求写到上一次请求的回调函数内部，这样就形成了常说的：回调地狱。

###### 使用 Promise

`Promise` 就是为了解决回调地狱的问题，为异步编程提供统一接口而提出的，最早有社区实现，由于ES6的原因，现在 `Promise` 已经是语言基础的一部分了。

现在我们使用 `Promise` 重新实现上面的案例，首先，我们要把异步请求数据的方法封装成 `Promise` ：

```js
function getDataAsync (url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            var res = {
                url: url,
                data: Math.random()
            }
            resolve(res)
        }, 1000)
    })
}
```

那么请求的代码应该这样写：

```js
getDataAsync('/page/1?param=123')
    .then(res1 => {
        console.log(res1)
        return getDataAsync(`/page/2?param=${res1.data}`)
    })
    .then(res2 => {
        console.log(res2)
        return getDataAsync(`/page/3?param=${res2.data}`)
    })
    .then(res3 => {
        console.log(res3)
    })
```

`then` 方法返回一个新的 `Promise` 对象，`then` 方法的链式调用避免了 `CallBack` 回调地狱。但也并不是完美，比如我们要添加很多 `then` 语句，
每一个 `then` 还是要写一个回调。如果场景再复杂一点，比如后边的每一个请求依赖前面所有请求的结果，而不仅仅依赖上一次请求的结果，那会更复杂。
为了做的更好，`async/await` 就应运而生了，来看看使用 `async/await` 要如何实现。

###### async/await

`getDataAsync` 方法不变，如下：

```js
function getDataAsync (url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            var res = {
                url: url,
                data: Math.random()
            }
            resolve(res)
        }, 1000)
    })
}
```

业务代码如下：

```js
async function getData () {
    var res1 = await getDataAsync('/page/1?param=123')
    console.log(res1)
    var res2 = await getDataAsync(`/page/2?param=${res1.data}`)
    console.log(res2)
    var res3 = await getDataAsync(`/page/2?param=${res2.data}`)
    console.log(res3)
}
```

对比 `Promise` 感觉怎么样？是不是非常清晰，但是 `async/await` 是基于 `Promise` 的，因为使用 `async` 修饰的方法最终返回一个 `Promise`
