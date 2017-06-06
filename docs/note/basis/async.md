## 异步处理

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

通过上面的代码可以看出，第一次请求的 `url` 地址为：`/page/1?param=123`，返回结果为 `res1`。

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

对比 `Promise` 感觉怎么样？是不是非常清晰，但是 `async/await` 是基于 `Promise` 的，因为使用 `async` 修饰的方法最终返回一个 `Promise`，
实际上，`async/await` 可以看做是使用 `Generator` 函数处理异步的语法糖，我们来看看如何使用 `Generator` 函数处理异步。

###### Generator

首先异步函数依然是：

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

使用 `Generator` 函数可以这样写：

```js
function * getData () {
    var res1 = yield getDataAsync('/page/1?param=123')
    console.log(res1)
    var res2 = yield getDataAsync(`/page/2?param=${res1.data}`)
    console.log(res2)
    var res3 = yield getDataAsync(`/page/2?param=${res2.data}`)
    console.log(res3))
}
```

然后我们这样逐步执行：

```js
var g = getData()
g.next().value.then(res1 => {
    g.next(res1).value.then(res2 => {
        g.next(res2).value.then(() => {
            g.next()
        })
    })
})
```

上面的代码，我们逐步调用遍历器的 `next()` 方法，由于每一个 `next()` 方法返回值的 `value` 属性为一个 `Promise` 对象，所以我们为其添加 `then` 方法，
在 `then` 方法里面接着运行 `next` 方法挪移遍历器指针，直到 `Generator` 函数运行完成，实际上，这个过程我们不必手动完成，可以封装成一个简单的执行器：

```js
function run (gen) {
    var g = gen()

    function next (data) {
        var res = g.next(data)
        if (res.done) return res.value
        res.value.then((data) => {
            next(data)
        })
    }

    next()
    
}
```

`run` 方法用来自动运行异步的 `Generator` 函数，其实就是一个递归的过程调用的过程。这样我们就不必手动执行 `Generator` 函数了。
有了 `run` 方法，我们只需要这样运行 `getData` 方法：

```js
run(getData)
```

这样，我们就可以把异步操作封装到 `Generator` 函数内部，使用 `run` 方法作为 `Generator` 函数的自执行器，来处理异步。其实我们不难发现，
`async/await` 方法相比于 `Generator` 处理异步的方式，有很多相似的地方，只不过 `async/await` 在语义化方面更加明显，同时 `async/await`
不需要我们手写执行器，其内部已经帮我们封装好了，这就是为什么说 `async/await` 是 `Generator` 函数处理异步的语法糖了。

