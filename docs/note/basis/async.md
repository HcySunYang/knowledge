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

#### Promise

###### Promise.prototype.then

* 作用：为 `Promise` 实例添加状态改变的回调函数

* 参数：

第一个参数：状态从 `pending` -> `fulfilled` 时的回调函数

第二个参数：状态从 `pending` -> `rejected` 时的回调函数

* 返回值：新的 `Promise` 实例（**注意不是原来的 `Promise` 实例**）

* 特点

由于 `then` 方法返回一个新的 `Promise` 实例，所以 `then` 方法是可以链式调用的，链式调用的 `then` 方法有两个特点：

第一：后一个 `then` 方法的回调函数的参数是前一个 `then` 方法的返回值

第二：如果前一个 `then` 方法的返回值是一个 `Promise` 实例，那么后一个 `then` 方法的回调函数会等待该 `Promise` 实例的状态改变后再执行

###### Promise.prototype.catch

* 说明：`Promise.prototype.catch` 方法是 `.then(null, rejection)` 的别名

* 特点

`Promise` 有一点特点，如下代码：

```js
const p1 = new Promise(function (resolve, reject) {
    setTimeout(() => {
        reject('err')
    }, 1000)
})

p1.then(
    res => console.log('s1'),
    err => console.log('e1')
).then(
    res => console.log('s2')
).catch(
    err => console.log('e2')
)
```

上面代码的输出结果是：`e1`、`s2`，可以发现，在第一个 `then` 方法执行的错误处理函数中捕获到了错误，所以输出了 `e1`，那么这个错误已经被捕获到了，也就不需要 `catch` 再次捕获了，所以没有输出 `e2`，这是正常的，但问题是竟然输出了 `s2`。。。。

为了避免上面的问题，你应该总是使用 `catch` 进行错误处理，而尽量避免在 `then` 方法中添加错误处理函数，上面的代码修改后如下：

```js
p1.then(
    res => console.log('s1')
).then(
    res => console.log('s2')
).catch(
    err => console.log('e2')
)
```

这样，就只会输出 `e2` 了。

###### Promise.prototype.finally

有的时候，我们希望无论 `Promise` 实例的结果最终变成 `fulfilled` 还是 `rejected`，我们都要执行某个动作，我们可以这样做：

```js
p1.then(
    res => console.log('s1')
).catch(
    err => console.log('e1')
).then(
    () => console.log('end'),
    () => console.log('end')
)
```

在最后使用一个 `then` 方法作为结尾，并且成功和失败的回调函数做了相同的事情，这样就能保证无论 `Promise` 实例的状态如何变化，最后总是执行某个动作，但问题是我们写了两端相同的代码，并且 `then` 方法也不够语义化，为了解决这个问题，我们就可以采用 `.finally`：

```js
p1.then(
    res => console.log('s1')
).catch(
    err => console.log('e1')
).finally(
    () => console.log('end')
)
```

很容易能够发现，`.finally` 只不过是一个成功与失败的回调函数相同的 `.then` 而已。

###### Promise.all

* 作用：将多个 `Promise` 实例包装成一个 `Promise` 实例

* 参数：一个参数，该参数应该具有 `Iterator` 接口，通常是一个数组，数组的每个元素都是一个 `Promise` 实例，如果不是那么会自动调用 `Promise.resolve` 方法将其转为 `Promise` 实例。

* 特点：

只有当所有 `Promise` 实例的状态都变为 `fulfilled`，那么 `Promise.all` 生成的实例才会 `fulfilled`。

只要有一个 `Promise` 实例的状态变成 `rejected`，那么 `Promise.all` 生成的实例就会 `rejected`。

* 例子：

```js
const p = Promise.all([promise1, promise2, promise3])

p.then(
    (res) => {
        // res 是结果数组
    }
)
```

###### Promise.race

* 作用：与 `Promise.all` 类似，也是将多个 `Promise` 实例包装成一个 `Promise` 实例。

* 参数：与 `Promise.all` 相同

* 特点：

`Promise.race` 方法生成的 `Promise` 实例的状态取决于其所包装的所有 `Promise` 实例中状态最先改变的那个 `Promise` 实例的状态。

* 例子：请求超时

```js
const p = Promise.race([
    getData('/path/data'),
    new Promise((resolve, reject) => {
        setTimeout(() => { reject('timeout') }, 10000)
    })
])

p.then(res => console.log(res))
p.catch(msg => console.log(msg))
```

###### Promise.resolve

* 作用：将现有对象(或者原始值)转为 `Promise` 对象。

* 参数：参数可以是任意类型，不同的参数其行为不同
    * 如果参数是一个 `Promise` 对象，则原封不动返回
    * 如果参数是一个 `thenable` 对象(即带有 `then` 方法的对象)，则 `Promise.resolve` 会将其转为 `Promise` 对象并立即执行 `then` 方法
    * 如果参数是一个普通对象或原始值，则 `Promise.resolve` 会将其包装成 `Promise` 对象，状态为 `fulfilled`
    * 不带参数，则直接返回一个状态为 `fulfilled` 的 `Promise` 对象

###### Promise.reject

* 作用：返回一个状态为 `rejected` 的 `Promise` 对象

* 参数：任意参数，该参数将作为失败的理由：

```js
Promise.reject('err')

// 等价于
new Promise(function (resolve, reject) {
    reject('err')
})
```

###### 统一使用 Promise

无论对于同步操作，还是异步操作，我们都可以将其作为异步操作统一处理，这样就能统一使用 `Promise` 的API了，假设函数 `fn` 是一个同步函数：

```js
function fn () {
    console.log('fn 执行了')
}
```

将其包装为异步函数：

```js
const p = new Promise((resolve, reject) => {
    resolve(fn())
})
```

或者点击这里：[https://github.com/tc39/proposal-promise-try](https://github.com/tc39/proposal-promise-try)

