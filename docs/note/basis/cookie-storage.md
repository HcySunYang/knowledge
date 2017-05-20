## cookie 与 sessionStorage 和 localStorage 的区别

#### 区别

###### 数据的生命周期

cookie 可以设置失效时间，默认是关闭浏览器后失效

sessionStorage 仅在当前回话下有效，关闭标签或浏览器后失效

localStorage 除非手动清除，否则永久保存

###### 存储数据的大小

cookie 在 4kb 左右

sessionStorage 和 localStorage 在 5MB 左右，因浏览器而异

###### 与服务器端通讯

cookie 会被携带在HTTP的请求头中，所以用cookie存储太多数据会带有性能问题

sessionStorage 和 localStorage 仅存在客户端，不参与服务器通讯

正因为 cookie 会参与服务器通讯，所以 storage 是不能取代 cookie 的，因为cookie是HTTP协议的一部分

###### 易用性

cookie 的原生接口不友好，需要手动封装读写操作

storage 的原生接口还是可以接受的

#### 注意
storage在存储数据的时候，都是以字符串的形式进行保存的，所以无论你存储的是数字类型还是对象类型，当你读取的时候，你得到的将是该数据的字符串表示

#### 封装

###### cookie 的增删改查

document.cookie

###### storage 的增删改查

曾：

```js
localStorage.age = 24
// 等价于
localStorage.setItem('age', 24)
```

查：

```js
localStorage.age
// 等价于
localStorage.getItem('age')
```

改：

修改类似于增加，在原有的 `key` 上重新设置值就可以了

```js
localStorage.age = 24
// 修改
localStorage.age = 30
```

删：

删除全部存储的内容

```js
localStorage.clear()
```

删除某个键值

```js
localStorage.removeItem('age')
```

获取对应索引的键值：

```js
localStorage.key(index)
```
