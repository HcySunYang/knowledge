## 跨域

#### 同源策略

###### 定义

同源策略指三个相同：协议相同、域名相同、端口相同，有一个不同即非同源。

<p class="tip">主域与子域、域名与域名对应的IP。都是非同源的</p>

###### 意义

同源策略可以算是web安全的基石，没有同源策略就么有安全可言。

###### 非同源的限制

* 无法共享 cookie/Storage/indexDB
* 无法操作彼此的 DOM
* 无法发送 ajax 请求
* 无法通过 flash 发送 http 请求

#### 跨域请求的方法

###### JSONP

* 原理：JSONP 的原理是利用 `<script>` 标签的 `src` 属性加载资源不受同源策略影响，其本质是向服务端请求一段 `js` 代码。

* 实现：

```js
let scriptTag = document.createElement('script')
// 其中cb是回调函数参数的名字，cbname是回调函数的名字，这两个名字要与服务端沟通定义
scriptTag.src = 'http://hcysun.me/xxx?cb=cbname'
document.body.appendChild(scriptTag)
```

* 优点
    * 1、可跨域
    * 2、兼容性好，基本全部兼容

* 缺点
    * 只支持 `GET` 请求
    * 确定JSONP请求是否失败并不容易，一般根据超时时间来判断

###### CORS

全称是：Cross-Origin Resource Sharing（跨域资源共享）

* 原理：

    * 1、当浏览器发现我们的XHR请求不符合同源策略时，会在请求头添加 `Origin` 字段，代表请求的来源
    * 2、服务端需要处理请求头部的 `Origin` 字段，根据情况在响应头中添加
        * `Access-Control-Allow-Origin`
        * `Access-Control-Allow-Methods`
        * `Access-Control-Allow-Headers`
        等头部信息

* 缺点
    * 兼容性问题，他是现代浏览器支持的一种跨域资源请求的一种方式

#### iframe中跨域请求的方法

###### 跨文档通信API（postMessage）

```js
// Page Foo
iframe.contentWindow.postMessage('Hello from foo', '/path/to/bar')

// Page Bar
window.parent.addEventListener('message', function (e) {
    console.log(e.source)    // 发送消息的窗口
    console.log(e.origin)  // 消息发向的网址
    console.log(e.data)    // 消息内容
})
```

###### 片段标识符（hash值）

父级页面虽然不能操作 `iframe` 的 `window` 和 DOM，但是可以修改 `iframe` 的 `URL`，通过修改 `hash` 值（即：`location.hash`），监听 `hashchange` 事件进行跨文档通信

###### window.name
