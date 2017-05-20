## 一、节点概览

#### DOM文档对象模型

概念：DOM的目的是为使用JavaScript操作DOM提供编程接口

#### 节点类型

##### 全部的节点类型

Node构造函数的属性，也是节点属性 nodeType 的值

```
    ELEMENT_NODE----1
    ATTRIBUTE_NODE----2
    TEXT_NODE----3
    CDATA_SECTION_NODE----4
    ENTITY_REFERENCE_NODE----5
    ENTITY_NODE----6
    PROCESSING_INSTRUCTION_NODE----7
    COMMENT_NODE----8
    DOCUMENT_NODE----9
    DOCUMENT_TYPE_NODE----10
    DOCUMENT_FRAGMENT_NODE----11
    NOTATION_NODE----12
    DOCUMENT_POSITION_DISCONNECTED----1
    DOCUMENT_POSITION_PRECEDING----2
    DOCUMENT_POSITION_FOLLOWING----4
    DOCUMENT_POSITION_CONTAINS----8
    DOCUMENT_POSITION_CONTAINED_BY----16
    DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC----32
```

##### 常用节点类型
	
```
    DOCUMENT_NODE ---- 9 (window.document)
    ELEMENT_NODE ---- 1 (<body> <p> 等标签元素)
    ATTRIBUTE_NODE ---- 2 (class="test")
    TEXT_BODE ---- 3 (文本节点)
    DOCUMENT_FRAGMENT_NODE ---- 11 (document.createDocumentFragment())
    DOCUMENT_TYPE_NODE ---- 10 (<!DOCUMENT html>)
```

##### 节点继承链

所有节点类型都继承自Node，并且这个继承链可能更长

```
    Object -> EventTarget -> Node -> Element -> HTMLElement -> HTML*Element
    Object -> EventTarget -> Node -> Attr (DOM4 弃用)
    Object -> EventTarget -> Node -> CharacterData -> Text
    Object -> EventTarget -> Node -> Document -> HTMLDocument
    Object -> EventTarget -> Node -> DocumentFragment
```

继承链中的每一个环节都为最终的节点类型提供了大量的属性和方法

#### 节点属性

###### nodeType
* 读写特性：只读
* 描述：节点的类型，返回以上常量值

###### nodeName
* 读写特性：只读
* 描述：节点名字，该属性的返回值根据节点类型不同而异：
```
    元素节点返回标签名，与 tagName 值相同
    文本节点返回 #text
    Comment 节点返回 #comment
    Document 返回 #document
    DocumentFragment 返回 #document-fragment
```
###### nodeValue
* 读写特性：读写
* 描述：
    
    除了 Text 和 Comment 节点以及 Attr 节点，所有节点的 nodeValue 都返回 null
    nodeValue 的作用就是获取 Text 和 Comment 节点的实际文本字符串，也可以通过设置 nodeValue 的值修改 这两个节点内的文本内容

###### innerHTML
* 读写特性：读写
* 描述：

    读：读取元素节点的内容(标签和文本)作为JavaScript字符串

    写：设置元素节点的内容(标签将被解析为真正的HTML元素)。
<p class="tip">
    注意：因为 innerHTML 会调用一个沉重且高消耗的HTML解析器。所以慎用
</p>

###### outerHTML
* 读写特性：读写
* 描述：

    读：读取包含元素本身及其内容作为JavaScript字符串

    写：设置的内容将替换自身标签将被解析为真正的HTML元素()

###### textContent
* 读写特性：读写
* 描述：

    读：获取元素节点内所有文本节点的值，包括其子元素节点的文本节点

    写：将JavaScript字符串创建为文本节点作为该元素的子节点(原有的所有子节点将被删除)

<p class="tip">
    注意：在 文档节点(document) 或 文档类型节点(document.doctype) 调用 textContent 返回 null
        textContent 属性也能返回 `<script>` `<style>` 标签的内容
</p>

###### childNodes
* 读写特性：只读
* 描述：

    返回调用该方法的元素下所有“直属”子节点。包括元素节点，文本节点，注释节点等等所有节点。

    返回的集合是 NodeList

###### parentNode
* 读写特性：只读
* 描述：返回调用该方法的节点的父节点

###### firstChild
* 读写特性：只读
* 描述：返回调用该方法的节点的第一个子节点

###### lastChild
* 读写特性：只读
* 描述：返回调用该方法的节点的最后一个子节点

###### nextSibling
* 读写特性：只读
* 描述：返回调用该方法的节点的前一个兄弟节点

###### previousSibling
* 读写特性：只读
* 描述：返回调用该方法的节点的后一个兄弟节点

###### children
* 读写特性：只读
* 描述：返回调用该方法的所有元素子节点

###### parentElement
* 读写特性：只读
* 描述：返回调用该方法的节点的父元素节点

###### firstElementChild
* 读写特性：只读
* 描述：返回调用该方法的节点的第一个“元素”子节点

###### lastElementChuild
* 读写特性：只读
* 描述：返回调用该方法的节点的最后一个“元素”子节点

###### nextElementSibling
* 读写特性：只读
* 描述：返回调用该方法的节点的下一个兄弟“元素”节点

###### previousElementSibling
* 读写特性：只读
* 描述：返回调用该方法的节点的前一个兄弟“元素”节点

###### ownerDocument
* 读写特性：只读
* 描述：返回该节点所在的 document 对象，document.ownerDocument === null

#### 节点方法

###### insertAdjacentHTML(position, text)

* 描述：
	
    指定在 开标签前后 或者 闭标签前后 插入HTML文本。

* 参数：

    * `{String} position` 插入的位置：
        * `beforebegin` 开标签前
        * `afterbegin` 开标签后
        * `beforeend` 闭标签前
        * `afterend` 闭标签后
    * `{String} text` 插入的内容，字符串。与 innerHTML的行为相同

* 延伸：

    除火狐浏览器外，所有现代浏览器都可以使用下面两个方法：
    * insertAdjacentElement():
    * insertAdjacentText(): 
        

###### appendChild(element)

* 描述：将指定节点插入到调用该方法的子节点末尾

* 参数：
    * `{Element} element` 要插入的节点

* 返回值：
    * `{Element}` 被插入的节点
    
###### insertBefore(element, target)

* 描述：在调用该方法的元素的指定子节点之前插入所给节点，如果省略第二个参数，那么行为与appendChild相同

* 参数：
    * `{Element} element` 要插入的节点
    * `{Element} target` 想要在哪个节点之前插入的该节点的引用

* 返回值：
    * `{Element}` 被插入的节点

###### removeChild(element):

* 描述：移除调用该方法的元素的子节点

* 参数：
    * `{Element} element` 要移除的子节点

* 返回值：
    * `{Element}` 被移除节点的引用

###### replaceChild(element, target)

* 描述：使用新节点替换调用该方法的元素的指定子节点

* 参数：
    * `{Element} element` 新子节点
    * `{Element} target` 要替换的子节点

* 返回值：
    * `{Element}` 被替换节点的引用
    
###### cloneNode(deep)

* 描述：深/浅 复制调用该方法的节点

* 参数：
    * `{Boolean} deep` true表示深复制，false表示浅复制，默认为false

* 返回值：
    * `{Element}` 克隆的节点

<p class="tip">
注意：无论深复制还是浅复制，都只会复制节点的内联事件，任何通过 addEventListener 或 onxxx 添加的事件都不会被复制
</p>
				
###### contains(element)

* 描述：判断调用该方法的节点是否包含给定的节点

* 参数：
    * `{Element} element` 节点

* 返回值：
    * `{Boolean}` ture 包含，fals 不包含

###### compareDocumentPosition(element)

* 描述：对比传入节点和调用该方法的节点的位置。

* 参数：
    * `{Element} element` 节点

* 返回值：
    * `{Number}` 数字，口诀如下：该口诀中的位置是 传入节点 相对于 调用该方法节点 的
    `前2后4里20，同0外10不在1`

#### 节点集合

`NodeList` 或 `HTMLCollection`

* 特点：
    * 类数组对象
    * 拥有 length 属性
    * 实时节点树，每当文档结构发生变化时，他们都会得到更新
    * 集合的节点顺序与节点所在树中的顺序相同(深度优先)

```
    可以将节点集合(NodeList 或 HTMLCollection)转为数组，这样做的好处有两点：
        1、因为这两个集合是动态的，转为数组可以创建当前集合的快照。
        2、转为数组可以使用Array原型下的许多数组方法
        将NodeList或HTMLCollection转为数组的方法有：
            Array.prototype.clice.call(list) // 或者 Array.prototype.concat.call(list)
            Array.from(list)
```
		
## 二、文档节点

#### 概述

* 节点类型：DOCUMENT_NODE ---- 9
* 继承链： Object -> EventTarget -> Node -> Document -> HTMLDocument
* 示例：window.document

#### 文档节点属性
		
###### title

```js
// 获取文档标题
document.title
```

###### referrer

```js
// 获取提及者
document.referrer
```

###### URL

```js
// 获取文档url连接地址
document.URL
```

###### lastModified

```js
// 获取文档最后修改时间
document.lastModified
```

###### compatMode

```js
// 获取兼容模式 BackCompat: 怪异模式，CSS1Compat: 严格模式
document.compatMode
```

###### doctype

```js
// 取得 <!DOCTYPE html> 元素
document.doctype
```

###### documentElement

```js
// 取得 <html> 元素
document.documentElement
```

###### head

```js
// 取得 <head> 元素
document.head
```

###### links

```js
// 取得所有 <a> 元素
document.links
```

###### links

```js
// 取得文档的所有样式表
document.styleSheets
```

###### activeElement

```js
// 取得文档中聚焦(获得焦点)的元素
document.activeElement
```

###### defaultView

```js
// 获取顶部对象/全局对象，在浏览器中为 window，在其他JavaScript环境为该环境的顶部对象
document.defaultView
```

###### all

```js
// HTML 文档中所有元素组成的集合
document.all
```

###### forms

```js
// 文档中所有 <form> 元素组成的集合
document.forms
```

###### images

```js
// 文档中所有的 <img> 元素组成的集合
document.images
```

###### scripts

```js
// 文档中所有 <script> 元素组成的集合
document.scripts
```

#### 文档节点方法

###### document.createElement(tagName)

* 描述：创建元素节点

* 参数：
    * `{String} tagName` 要创建的元素名字

* 返回值：
    * `{Element}` 创建的新元素

###### document.createTextNode(text)

* 描述：创建文本节点

* 参数：
    * `{String} text` 文本内容，作为文本节点的 nodeValue 的值

* 返回值：
    * `{TEXT_NODE}` 创建的新文本节点

###### document.createComment(text)

* 描述：创建注释节点

* 参数：
    * `{String} text` 注释内容，作为注释节点的 nodeValue 的值

* 返回值：
    * `{COMMENT_NODE}` 创建的新注释节点

###### document.createDocumentFragment()

* 描述：创建文档碎片

* 返回值：
    * `{DOCUMENT_FRAGMENT_NODE}` 文档碎片

###### document.hasFocus()

* 描述：

    判断当前文档是否获得焦点（切换浏览器tab页到其他页面，该方法即返回false）

* 返回值：
    * `{Boolean}` true 代表文档获得焦点，false 代表没有获得焦点

###### document.getElementById(id)

* 描述：根据传入的id值匹配元素节点

* 参数：
    * `{String} id` id值

* 返回值：
    * `{Element | null}` 匹配则返回元素节点，否则返回 null

###### document.elementFromPoint(x, y)

* 描述：获取文档上某一点最顶层的元素

* 参数：
    * `{Number} x` 横坐标
    * `{Number} y` 纵坐标

* 返回值：
    * `{Element}` 该点所在的最顶层元素

###### document.implementation.createHTMLDocument()

* 描述：创建一个当前文档之外的HTML文档

* 返回值：
    * `{Document}` 文档引用

###### document.implementation.hasFeature(feature, version)

* 描述：探测浏览器是否支持指定版本的特性/模块

* 参数：
    * `{String} feature` 特性名字
    * `{Number} version` 版本

    可传参数如下：
```
    | feature       | version       |
    | ------------- | ------------- |
    | Core          | 1.0/2.0/3.0   |
    | XML           | 1.0/2.0/3.0   |
    | HTML          | 1.0/2.0       |
    | Views         | 2.0           |
    | StyleSheets   | 2.0           |
    | CSS           | 2.0           |
    | CSS2          | 2.0           |
    | Events        | 2.0/3.0       |
    | UIEvents      | 2.0/3.0       |
    | MouseEvents   | 2.0/3.0       |
    | MutationEvents| 2.0/3.0       |
    | HTMLEvents    | 2.0           |
    | Range         | 2.0           |
    | Traversal     | 2.0/3.0       |
    | LS            | 3.0           |
    | LS-Async      | 3.0           |
    | Validation    | 3.0           |
```
* 返回值：
    * `{Boolean}` 支持则返回 true，否则返回 false

## 三、元素节点

#### 概述

* 节点类型：ELEMENT_NODE ---- 1
* 继承链： Object -> EventTarget -> Node -> Element -> HTMLElement -> HTML*Element -> `<p>`
* 示例：document.querySelector('p')

#### 元素节点属性

###### tagName
* 读写特性：只读
* 描述：获取元素的标签名，与 `nodeName` 值相同

###### attributes
* 读写特性：只读
* 描述：获取元素上 属性 和 值 的集合（NamedNodeMap）

```
正因为该集合为 NamedNodeMap 所以可以使用一下方法操作集合
getNamedItem()
setNamedItem()
removeNamedItem()
不推荐这种方式操作属性
该属性的优势是所获取的集合是动态的，这样我们可以动态的知晓某元素上属性的数量
```

###### classList
* 读写特性：只读
* 描述：获取元素节点类属性和值的集合（类数组对象）

```
el.classList.add('b')	// 添加b类
el.classList.remove('b')	// 移除b类
el.classList.toggle('b')	// 切换b类
el.classList.contains('b')	// 判断el元素有没有b类
el.classList.value 		// 返回值与 el.className 相同（类名被空格分隔的字符串）
el.classList.length		// el元素拥有类的数量
注意：IE9不支持 classList
```

###### dataset
* 读写特性：只读
* 描述：返回一个对象，包含元素所有以 data-* 起始的属性

<p class="tip">
data-a-a 将要这样访问： el.dataset.aA (即转为驼峰)，可以使用 delete 语句删除一个data属性，另外IE9不支持该属性，可以使用 getAttribute/setAttribute/removeAttribute/hasAttribute 代替
</p>

#### 元素节点方法

###### getAttribute(attrName)

* 描述：获取元素节点上某一个属性的值

* 参数：
    * `{String} attrName` 属性名字

* 返回值：
    * `{String}` 属性值

###### setAttribute(attrName, attrValue)

* 描述：获取元素节点上某一个属性的值

* 参数：
    * `{String} attrName` 属性名字
    * `{String} attrValue` 属性值

###### removeAttribute(attrName)

* 描述：移除调用该方法的元素节点的某一属性

* 参数：
    * `{String} attrName` 属性名字

###### hasAttribute(attrName)

* 描述：判断元素是否有某一特定属性

* 参数：
    * `{String} attrName` 属性名字

* 返回值：
    * `{Boolean}` true 有，false 没有

<p class="tip">因为 hasAttribute() 方法可以为布尔值型属性取得布尔值反馈，所以可以用来判断单选框复选框是否被选中</p>

###### querySelector(selector)

* 描述：根据css选择器返回第一个匹配的节点

* 参数：
    * `{String} selector` css选择器，支持css3

* 返回值：
    * `{Element | Null}` 匹配的元素节点 或 null

<p class="tip">除了 document.querySelector() 外，还可以在指定元素下使用该方法 el.querySelector()</p>

###### querySelectorAll(selector)

* 描述：根据css选择器匹配并返回符合的节点集合

* 参数：
    * `{String} selector` css选择器，支持css3

* 返回值：
    * `{Array}` 匹配的元素节点 或 null

<p class="tip">
注意：querySelectorAll() 方法返回的节点集合是创建时文档的快照，并不是实时动态的。而 getElementsByTagName 和 getElementsByClassName 返回的节点集合则是动态的

加特技：除了 document.querySelectorAll() 外，还可以在指定元素下使用该方法 el.querySelectorAll()
</p>

###### getElementsByClassName(className)

* 描述：根据class值匹配并返回节点集合

* 参数：
    * `{String} className` class值

* 返回值：
    * `{节点集合}` 节点集合

<p class="tip">除了 document.getElementsByClassName() 外，还可以在指定元素下使用该方法 el.getElementsByClassName()</p>

###### getElementsByTagName(tagName)

* 描述：根据class值匹配并返回节点集合

* 参数：
    * `{String} tagName` 标签名

* 返回值：
    * `{节点集合}` 节点集合

<p class="tip">除了 document.getElementsByTagName() 外，还可以在指定元素下使用该方法 el.getElementsByTagName()</p>

###### getElementsByName(name)

* 描述：根据元素 name 属性值匹配并返回节点集合

* 参数：
    * `{String} name` 元素 name 属性值

* 返回值：
    * `{节点集合}` 节点集合

###### normalize()

* 描述：

合并调用该方法元素的多个子文本节点为一个子文本节点，其后代元素节点下的子节点也会被合并，注意，只有两个文本子节点相邻是才会被合并

#### 几何量 与 滚动几何量

##### 属性

###### offsetParent
* 读写特性：只读
* 描述：获取定位父级

offsetParent 的取值规则：

```
1、按照DOM树向上查找离调用该属性的元素最近的position属性不为static的祖先元素
2、如果没有找到，那么<body>元素就是 offsetParent 的值
3、如果在查询过程中，碰到 <td> <th> <table> 标签，那么 offsetParent 就为这些值
```

###### offsetTop
* 读写特性：读写
* 描述：

    读：获取元素边框外沿 到 其定位父级边框内沿 的上距离

    写：设置元素边框外沿 到 其定位父级边框内沿 的上距离

###### offsetLeft
* 读写特性：读写
* 描述：

    读：获取元素边框外沿 到 其定位父级边框内沿 的左距离

    写：设置元素边框外沿 到 其定位父级边框内沿 的左距离

###### offsetHeight
* 读写特性：只读
* 描述：获取元素高 （边框 + 填充 + 内容）

###### offsetWidth
* 读写特性：只读
* 描述：获取元素宽 （边框 + 填充 + 内容）

###### clientHeight
* 读写特性：只读
* 描述：获取元素的高 （填充 + 内容）

###### clientWidth
* 读写特性：只读
* 描述：获取元素的宽 （填充 + 内容）

###### scrollHeight
* 读写特性：只读
* 描述：获取滚动元素的高度

<p class="tip">注意：如果滚动区域内的子节点比滚动区域小，那么该属性返回滚动区域的高度</p>

###### scrollWidth
* 读写特性：只读
* 描述：获取滚动元素的宽度

<p class="tip">注意：如果滚动区域内的子节点比滚动区域小，那么该属性返回滚动区域的宽度</p>

###### scrollTop
* 读写特性：读写
* 描述：

    读：获取滚动元素当前已滚动的区域距离上边的距离

    写：使用JavaScript以编程的方式滚动元素到指定的距离

###### scrollLeft
* 读写特性：读写
* 描述：

    读：获取滚动元素当前已滚动的区域距离左边的距离

    写：使用JavaScript以编程的方式滚动元素到指定的距离

##### 方法

###### getBoundingClientRect()

* 描述：获取元素相对于整个页面的位置（top/right/bottom/left），以及元素的宽高

* 返回值：
    * `{Object}` 元素位置信息
        ```
        {
            top
            right
            bottom
            left
            width
            height
        }
        ```
<p class="tip">
    加特技：返回值中，width 和 height 为元素 （边框 + 填充 + 内容）的高度和宽度，与调用元素的 offsetHeight 与 offsetWidth 属性的返回值相同
</p>

###### scrollIntoView(position)

* 描述：滚动调用该方法的元素到滚动元素的视区

* 参数：
    * `{Boolean} position` true 滚动该元素到视区顶部，false 滚动该元素到视区底部。默认为 true

## 四、文本节点

#### 概述

* 节点类型：TEXT_NODE ---- 3
* 继承链： Object -> EventTarget -> Node -> CharacterData -> Text -> 'asdfasdg'

#### 文本节点属性

###### length
* 描述：文本节点拥有length属性，返回该节点文本内容的长度

###### data
* 描述：返回文本节点的字符串内容 与 nodeValue 的值相同

###### nodeValue
* 描述：与 data 属性的值相同

#### 文本节点方法

###### appendData(text)

* 描述：将text追加到节点末尾

* 参数：
    * `{String} text` 要追加的字符串

###### insertData(offset, text)

* 描述：在 offset 指定的位置前插入字符串 text

* 参数：
    * `{Number} offset` 位置
    * `{String} text` 要追加的字符串

###### deleteData(offset, count)

* 描述：在 offset 指定的位置开始，删除 count 个字符，包括 offset 位置

* 参数：
    * `{Number} offset` 位置
    * `{Number} count` 删除字符的数量

###### replaceData(offset, count, text)

* 描述：使用字符串 text 替换从 offset 指定的位置开始 count 个字符，包括 offset 位置

* 参数：
    * `{Number} offset` 位置
    * `{Number} count` 删除字符的数量
    * `{String} text` 字符串

###### substringData(offset, count)

* 描述：获取从 offset 指定的位置开始 count 个字符，包括 offset 位置

* 参数：
    * `{Number} offset` 位置
    * `{Number} count` 获取字符的数量

* 返回值：
    * `{String}` 获取到的字符串

###### splitText(offset)

* 描述：从 offset 指定的位置将调用该方法的文本节点分割成两个文本节点

* 参数：
    * `{Number} offset` 分割的位置，该位置将包含在后一个文本节点中

* 返回值：
    * `{TEXT_NODE}` 后一个文本节点的内容。

## 五、CSS 样式 与 样式表

#### CSS 样式

###### 元素的内联样式(style 属性)

```js
// 访问元素的 style 属性[el.style]，将返回 CSSStyleDeclaration 对象，仅包含该元素的内联样式，而不是计算后样式

el.style.驼峰属性名
el.style.setProperty('css属性', '值')
el.style.getProperty('css属性')
el.style.removeProperty('css属性')

// 使用一个由一系列css属性和值的字符串设置style的值
// 例：el.style.cssText = "width: 200px; height: 200px; background: red;"
el.style.cssText
```

###### 获取元素的计算后样式

* window.getComputedStyle(element)

* 描述：获取元素计算后的样式

* 参数：
    * `{Element} element` 元素

* 返回值：
    * `{CSSStyleDeclaration}` 包含元素属性键值对的对象

<p class="tip">
与 el.style 一样，返回 CSSStyleDeclaration 对象，但不同的是，使用 getComputedStyle 获得的 CSSStyleDeclaration 对象下的属性时只读的，而通过 style 属性获得的 CSSStyleDeclaration 是可设置的。
另外，getComputedStyle 获得的颜色值始终都是 rgb() 格式，而通过style获得的颜色值就是你再内敛样式中所写的样子，并且在通过 getComputedStyle 获取的 transform 属性值为矩阵 matrix
</p>

```
window.getComputedStyle()
作用：使用 window.getComputedStyle(el) 可以获取元素计算后的样式
一个参数：元素
返回值：与 el.style 一样，返回 CSSStyleDeclaration 对象，但不同的是，使用 getComputedStyle 获得的 CSSStyleDeclaration 对象下的属性时只读的，而通过 style 属性获得的 CSSStyleDeclaration 是可设置的。
另外，getComputedStyle 获得的颜色值始终都是 rgb() 格式，而通过style获得的颜色值就是你再内敛样式中所写的样子，并且在通过 getComputedStyle 获取的 transform 属性值为矩阵 matrix
```

#### CSS样式表 与 CSS规则

###### CSS样式表

使用 `<link>` 和 `<style>` 标签可以分别创建 外部 和 内部 样式表，一旦样式表被添加到HTML文档中，每个样式表将表示为一个 `CSSStyleSheet` 对象。该对象可以通过 `<link>` 或 `<style>` 标签元素的 sheet 属性访问： el.sheet

###### CSS规则

每个样式表都是由一条条规则组成的(如：`body{background-color: red;` 为一条规则)，CSS规则表示为一个 `CSSStyleRule` 对象，可以通过 `el.sheet.cssRules[n]` 或者 `el.sheet.rules[n]` 访问该样式表的第 `n` 条规则

###### 访问所有样式表

可以使用 `document.styleSheets` 访问该文档的所有样式表，该属性返回由 `CSSStyleSheet` 对象组成的 `StyleSheetList` 对象

## 六、DOM中的JavaScript

#### JavaScript默认是同步解析的

当DOM在解析时遇到 `<script>` 标签，将停止解析文档，并执行JavaScript脚本，如果是外部脚本，必须要下载后再解析，这将导致性能问题。

#### defer

可以使用 defer 属性推迟外部脚本的下载与执行，直到html文档解析完成。

#### async

使用 async 属性异步下载并执行外部JavaScript文件

* 不会阻塞DOM的解析与其他资源(如：图片、样式表)的下载
* 如果有多个外部JavaScript脚本拥有 `async` 属性，那么他们的执行顺序很可能不按照DOM中的顺序执行。先下载完的先执行
* 如果一个 `<script>` 元素同时存在 `defer` 和 `async` ，`async` 的优先级高
* 注意：使用JavaScript动态创建的 `<script>` 元素，并添加到DOM，那么该脚本将强制按照 `async` 的规则下载与执行
* 通过 `<script>` 元素的 `onload`、`onerror`、`load`、`error` 等事件，可以监听异步下载的JavaScript的下载情况
