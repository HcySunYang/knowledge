## 队列

#### 概念

队列是一种先进先出(first-in-first-out)的数据结构，只允许在一段插入数据，在另一端读取数据。对队列的操作有如下几种：

* 入队（向队列尾部插入新元素）
* 出队（删除队列头部的元素）
* 读取队头的元素（获取但不删除队列头部的元素）
* 清空队列
* 获取队列的元素个数
* 等...

#### JavaScript实现

我们知道，JavaScript数组原生提供了队列方法，如：`push`、`pop`、`unshift`、`shift` 等等，所以我们既可以用数组来模拟栈，又可以用数组来模拟队列，但正因为这样，数组既不能严格的作为栈使用，也不能严格的作为队列使用，所以我们有必要手动封装严格的队列的定义

###### 队列的方法和属性

* 属性
    * 无
    * 或者自定义需要的属性
* 方法
    * enqueue(element)
    * dequeue()
    * peek()
    * clear()
    * length()

###### 代码实现

```js
function Queue () {
    this.store = []
}
Queue.prototype = {
    constructor: Queue,
    enqueue: function (element) {
        return this.store.push(element)
    },
    dequeue: function () {
        return this.store.shift()
    },
    peek: function () {
        return this.store[0]
    },
    clear: function () {
        this.store.length = 0
    },
    length: function () {
        return this.store.length
    }
}
```

上面的代码是一个极简的队列实现，我们可以写如下测试代码：

```js
var qu = new Queue()

qu.enqueue('1')
qu.enqueue('2')
qu.enqueue('3')

console.log(qu.peek())  // 1

qu.dequeue()

console.log(qu.peek())  // 2

qu.clear()

console.log(qu.peek())  // undefined
```

#### 优先队列

一般情况下，队列始终保持着先进先出的原则，但有些业务场景，并不一定要求先进来的要先出去，在出队的时候，要考虑队列中所有元素权重因子，优先级最高的元素最先出队，这种队列叫做`优先队列`。
