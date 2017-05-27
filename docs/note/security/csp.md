## CSP (内容安全策略)

<p class="tip">CSP [Content Security Policy] 译为：内容安全策略</p>

#### CSP的目的

XSS(Cross Site Scripting) 跨站脚本攻击是最常见也是危害最大的攻击手段，我们前端能够做一些能力范围内的处理，比如最简单的将表单内容脚本序列化为HTML实体，以防止恶意脚本的执行，但除此之外，还有很多跨站脚本攻击的方式，如下：

```html
<a href="javascript:alert(1)"></a>
<iframe src="javascript:alert(1)"></iframe>
<img src="x" onerror="alert(1)" style="">
<video src="x" onerror="alert(1)"></video>
<div onclick="alert(1)" onmouseover="alert(2)"><div></div></div>
```

利用 `javascript:..` 以及 内联事件进行攻击。

为了阻止这些攻击，我们前端也要做不少相应的工作，于是很多人提出能不能从根本上解决问题，让浏览器帮我们做这些事情，这就是 CSP 提出的原因和要解决的问题。

#### CSP 的原理以及开启方式

###### 原理

原理其实就是白名单机制，开发者明确告诉客户端(浏览器)哪些资源可以加载并执行，我们只需要提供配置，其他的工作由客户端(浏览器)来完成。

###### 开启CSP的方式

一、通过 `<meta>` 标签开启

```html
<meta http-equiv="Content-Security-Policy" content="配置项" >
```

二、通过添加 `Content-Security-Policy` 响应头字段

<img src="../../asset/img/csp.png" width="500" />

#### 可配置的选项

```
default-src：用来设置每个选项的默认值

script-src：外部脚本
style-src：样式表
img-src：图像
media-src：媒体文件（音频和视频）
font-src：字体文件
object-src：插件（比如 Flash）
child-src：框架
frame-ancestors：嵌入的外部资源（比如<frame>、<iframe>、<embed>和<applet>）
connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource等）
worker-src：worker脚本
manifest-src：manifest 文件

block-all-mixed-content：HTTPS 网页不得加载 HTTP 资源（浏览器已经默认开启）
upgrade-insecure-requests：自动将网页上所有加载外部资源的 HTTP 链接换成 HTTPS 协议
plugin-types：限制可以使用的插件格式
sandbox：浏览器行为的限制，比如不能有弹出窗口等。

report-uri：有时，我们不仅希望浏览器帮我们防止XSS的攻击，还希望将该行为上报到给定的网址，该选项用来配置上报的地址
```

#### 选项的值

每个限制选项可以设置以下几种值

* 主机名：`example.org`，`https://example.com:443`
* 路径名：`example.org/resources/js/`
* 通配符：`*.example.org`，`*://*.example.com:*`（表示任意协议、任意子域名、任意端口）
* 协议名：`https:`、`data:`
* 关键字'self'：当前域名，需要加引号
* 关键字'none'：禁止加载任何外部资源，需要加引号

###### 例子

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:">
```

上面代码中，CSP 做了如下配置：

* 脚本：只信任当前域名
* `<object>` 标签：不信任任何URL，即不加载任何资源
* 样式表：只信任 `cdn.example.org` 和 `third-party.org`
* 框架（frame）：必须使用HTTPS协议加载
* 其他资源：没有限制