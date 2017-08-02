## MIME type

#### 简介

`MIME` 和 `MIME type` 并不是一个东西，`MIME` 的全称是 `Multipurpose Internet Mail Extensions`，它扩展了电子邮件标准，比如使得邮件支持非文本格式附件（二进制、声音、图像等）。后来 `HTTP` 协议也是用了 `MIME` 框架，使得 `MIME` 被扩展为 `互联网媒体类型(Internet media type)`，也就是 `MIME type`，或者叫 `MIME 类型`。

#### 语法

语法如下：

```
type/subtype
```

其中 `type` 是主要类型，可以是离散类型(`discrete type`) 或者 多部分类型(`multipart type`)。

`discrete type` 包含有：`text`、`image`、`audio`、`video`、`application`。

`subtype` 可以理解为每个主要类型(`type`)的子类型，或者具体类型。

举例：

```
text/plain
text/html
image/jpeg
image/png
audio/mpeg
audio/ogg
audio/*
video/mp4
application/octet-stream

multipart/form-data
multipart/byteranges
```

所有的 `MIME type` 可以看这里 [MIME type 列表](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types)

#### MIME type 的作用

文件的扩展名在网络中是没有意义的，决定一个文件类型的就是 `MIME type`，在 `HTTP` 中就是响应头中的 `Content-Type`，比如：

```
Content-Type: text/HTML
```

其中 `text/HTML` 就是一种 `MIME type`，表示内容是 `text/HTML` 类型，也就是超文本文件。

在使用表单空间选择文件时，我们常常需要限制用户所能够选择的文件，比如只允许用户选择 `Excel` 表格文件，我们可以这样写：

```html
<input type="file" accept="application/vnd.ms-excel, application/x-excel">
```

其中 `application/vnd.ms-excel` 和 `application/x-excel` 就是两个 `MIME type`。