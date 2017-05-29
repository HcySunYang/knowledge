## 栈

#### 概念

栈是一种特殊的列表，栈内的元素只能通过列表的一端访问，这一端称为栈顶，栈是一种后入先出(last-in-first-out)的数据结构。
对栈的操作有以下几种：

* 1、压入栈（将一个元素压入栈顶）
* 2、弹出栈（弹出栈顶的元素）
* 3、预览栈顶（获取但不弹出栈顶的元素）
* 4、清空栈（清空栈内所有元素）
* 5、获取栈内元素的个数

#### JavaScript实现

我们知道，JavaScript数组原生提供了栈方法，如：`push`、`pop`、`unshift`、`shift` 等等，所以我们既可以用数组来模拟栈，又可以用数组来模拟队列，但正因为这样，数组既不能严格的作为栈使用，也不能严格的作为队列使用，所以我们有必要手动封装严格的栈的定义。

###### 栈的方法和属性

根据对栈的操作定义，我们可以总结栈所拥有的属性和方法

* 属性
    * 无
    * 或者你自定义需要的属性
* 方法
    * push(element)
    * pop()
    * peek()
    * clear()
    * length()

###### 代码实现

```js
function Stack () {
    this.store = []
}
Stack.prototype = {
    constructor: Stack,
    push: function (element) {
        return this.store.push(element)
    },
    pop: function () {
        return this.store.pop()
    },
    peek: function () {
        return this.store[this.length() - 1]
    },
    clear: function () {
        this.store.length = 0
    },
    length: function () {
        return this.store.length
    }
}
```

上面的代码是一段极简的实现，栈的底层数据结构是使用的数组：`this.store`，我们可以对上面的代码进行如下测试：

```js
var stack = new Stack()

console.log(stack.peek())   // undefined

stack.push(1)
stack.push(2)
stack.push(3)

console.log(stack.peek())   // 3

stack.pop()

console.log(stack.peek())   // 2
console.log(stack.length()) // 2

stack.clear()

console.log(stack.length()) // 0
```

#### 栈的应用

###### 回文问题

“回文” 简单的说就是正着读和反着读是一样的，比如 `abcba`，`12321` 等等。那么问题来了，如何确定一个字符串是不是回文呢？

有的同学可能已经想到了，js数组有一个方法 `reverse` ，用来反转一个数组的顺序，于是我们可以借助数组以及数组的 `reverse` 方法来判断：

```js
var str = '12321'
var reStr = str.split('').reverse().join('')
if (str === reStr) {
    // 是回文
}
```

那么问题来了，如果js在语言层面没有给我们提供 `reverse` 方法，需要我们手动封装怎么办呢？这，就用到了栈的知识，接下来我们手动封装 `reverse` 方法，用来对数组进行反转：

```js
function reverse (arr) {
    var stack = new Stack()
    for (var i = 0; i < arr.length; i++) {
        stack.push(arr[i])
    }
    var reArr = []
    while (stack.length() > 0) {
        reArr.push(stack.pop())
    }

    return reArr
}
```

然后，我们可以这样使用我们的 `reverse` 方法：

```js
var str = '12321'
var reStr = reverse(str.split('')).join('')
if (str === reStr) {
    // 是回文
}
```

原理其实很简单，我们把每个元素按照原来的顺序依次入栈，然后在依次将元素从栈顶弹出，那么弹出后元素的顺序应该是原来元素顺序的反转。

除了判断回文，数制转换也可以应用栈，判断表达式中的括号是否匹配等等，都可以应用到栈，其原理无非是利用了 `入栈` 与 `出栈` 后的顺序变化，以及栈内元素的数量来作出相应的判断。另外有些问题需要多个栈配合来解决，有兴趣的可以多在网上搜一搜用栈解决的一些问题。
