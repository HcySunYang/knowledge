## get 请求和 post 请求的区别

#### 传递数据的方式

get 通过 URL 或 cookie 传参

post 将数据放到请求体(BODY) 中

#### 传递数据的大小

get 的URL长度有限制

post 则可以传递大量的数据

#### 安全性

get 传递的参数会暴露在 URL 中

post 则不会

另外注意 GET 请求的参数会保存在浏览器的历史记录中