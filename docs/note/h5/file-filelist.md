## File 和 FileList

#### 简介

`File` API 顾名思义，是与文件相关的API，它提供了文件信息，并允许JavaScript访问文件的内容。

`File` API 继承了 `Blob`，如下证明：

```js
// Chrome 中
File.__proto__  // Blob
```

##### File 对象出现在哪里

###### FileList

通过 `<input type="file" />` 元素选择文件时的 `FileList` 对象访问，`FileList` 对象中的每个元素都是一个 `File` 对象实例。

```html
<input type="file" id="fileinput" />

<script>
var input = document.querySelector('#fileinput')
input.addEventListener('change', function () {
    this.files[0]   // 这就是一个 File 对象实例
}, false)
</script>

```

###### DataTransfer

拖放事件对象的 `dataTransfer.files` 属性中的每一个元素都是 `FIle` 对象实例。

#### File 构造函数

`File` 是一个构造函数，使用方式如下：

```js
new Blob(bits, name[, options])
```

参数：
* `{array} bits` bits是一个数组，可以包含 `ArrayBuffer`、`ArrayBufferView`、`Blob` 或者 `DOMString`。
* `{string} name` 文件的名字或者路径
* `{object} options` 用来设置文件属性的选项，有两个值可以设置：`type` 和 `lastModified`
    * `{string} type` 内容的 `MIME type`。
    * `{number} lastModified` 最后的修改时间，毫秒数。

示例：
```js
// 验证 File 继承 Blob
var file = new File(["foo"], "foo.txt", {
  type: "text/plain",
});

file instanceof Blob    // true
```

##### File 对象的属性

`File` 继承了 `Blob`，所以 `File` 示例对象的属性中包含继承自 `Blob` 的属性，比如：`size` 和 `type`。

###### lastModified

* `{Number} lastModified` 文件的最后修改时间，毫秒数

###### lastModifiedDate

* `{Date} lastModifiedDate` 文件的最后修改日期

###### name

* `{string} name` 文件的名称

###### size

* `{number} size` 文件的尺寸

###### type

* `{string} type` 文件的 `MIME type`

###### webkitRelativePath

* `{string} webkitRelativePath` 文件的相对路径

##### File 对象的方法

`File` 实例有三个方法，不过都不是标准的(Non-standard)，且已经不推荐使用了。推荐使用 `FileReader` 实例对象的对用方法替代：

###### getAsBinary()

使用 `FileReader` 对象的 `readAsBinaryString()` 代替

###### getAsDataURL()

使用 `FileReader` 对象的 `readAsDataURL()` 代替

###### getAsText()

使用 `FileReader` 对象的 `readAsText()` 代替

## FileList

`FileList` 对象是一个类数组对象，拥有 `length` 属性，对象的每个元素都是一个 `File` 对象实例，比如：

```html
<input type="file" id="fileinput" />

<script>
var input = document.querySelector('#fileinput')
input.addEventListener('change', function () {
    this.files      // 这就是一个 FileList 对象实例
    this.files[0]   // 这就是一个 File 对象实例
}, false)
</script>
```

其中 `this.files` 就是一个 FileList 对象实例，可以想数组一样访问其元素 `this.files[0]`，代表一个 `File` 对象实例。

也可以通过 `this.files.item(0)` 来访问其元素。

