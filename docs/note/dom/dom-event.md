## 一、绑定事件的方法

#### HTML内联属性绑定

```html
<div onclick="alert('fuck')"></div>
```

#### js获取DOM元素添加事件属性

```js
document.getElementById('box').onclick = function (){...}
```

#### 使用 addEventListener()

<p>
第一种方法，HTML内联属性绑定事件的方式不推荐，这违反了最佳实践。第二种方法的缺点是，只能同时给事件绑定一个callback，所以推荐一直使用 addEventListener() 方法给元素绑定事件
</p>

<p>
如果要移除一个通过 addEventListener 添加的事件处理函数，那么给 removeEventListener 传递的两个参数必须与 addEventListener 的前两个参数完全相同。这意味着，给一个元素绑定匿名事件处理函数将无法被移除
</p>

## 二、事件流

#### 事件流

页面中接收事件的顺序。分为三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段


#### 事件冒泡

事件首先由嵌套层次最深的节点接收，然后沿DOM树依次逐级向父代节点传播。

#### 事件捕获

不太具体的节点(或嵌套层次最浅的节点，通常是document)应该最先接收事件，然后沿DOM树依次向子代节点传递直到一个具体的子节点

## 三、绑定事件的方法

#### 标准方法

###### el.addEventListener(eventName, handle, useCapture)

* 描述：给DOM元素添加指定的事件处理函数

* 参数：
    * `{String} eventName` 事件名称
    * `{Function} handle` 事件函数
    * `{Boolean} useCapture` 是否在事件捕获阶段触发事件，true 代表捕获阶段触发，false 代表在冒泡阶段触发

###### el.removeEventListener(eventName, handle)

* 描述：移除通过 addEventListener 添加的事件处理函数

* 参数：
    * `{String} eventName` 事件名称
    * `{Function} handle` 事件函数
    
<p class="tip">
如果要移除一个通过 addEventListener 添加的事件处理函数，那么给 removeEventListener 传递的两个参数必须与 addEventListener 的前两个参数完全相同。这意味着，给一个元素绑定匿名事件处理函数将无法被移除
</p>

#### IE8及以下

addEventListener 和 removeEventListener 在IE8及以下不被支持

###### el.attachEvent(eventName, handle)

* 描述：给DOM元素添加指定的事件处理函数

* 参数：
    * `{String} eventName` 事件名称
    * `{Function} handle` 事件函数

###### el.detachEvent(eventName, handle)

* 描述：移除通过 addEventListener 添加的事件处理函数

* 参数：
    * `{String} eventName` 事件名称
    * `{Function} handle` 事件函数
    
#### 对比

attachEvent/detachEvent 与 addEventListener/removeEventListener 的区别：

* 由于IE8不支持事件捕获，所以通过 attachEvent/detachEvent 绑定的时间也只能在冒泡阶段触发
* 通过 attachEvent/detachEvent 绑定的事件函数会在全局作用域中运行，即： this === window
* 通过 attachEvent/detachEvent 绑定的事件函数以绑定时的先后顺序倒序被执行
* attachEvent/detachEvent 的第一个参数要在事件名称前面加 'on'


## 四、事件对象

#### 标准

##### 属性

###### event.bubbles
* 读写特性：只读
* 描述：表示事件是否冒泡

###### event.cancelable
* 读写特性：只读
* 描述：表示事件是否可以取消事件默认行为

###### event.currentTarget
* 读写特性：只读
* 描述：currentTarget的值始终等于 this，即指向事件所绑定到的元素

###### event.target
* 读写特性：只读
* 描述：真正触发事件的元素

###### event.defaultPrevented
* 读写特性：只读
* 描述：为 true 表示已经调用了 preventDefault()

###### event.detail
* 读写特性：只读
* 描述：与事件相关的细节信息

###### event.eventPhase
* 读写特性：只读
* 描述：调用该事件处理函数的阶段 `1` 表示捕获阶段 `2` 表示处于目标阶段 `3` 表示冒泡阶段

###### event.trusted
* 读写特性：只读
* 描述：为true表示事件是由浏览器生成的，false表示事件是由人工使用JavaScript创建的

###### event.type
* 读写特性：只读
* 描述：事件类型

###### event.type
* 读写特性：只读
* 描述：事件类型

##### 方法

###### event.preventDefault()

* 描述：阻止事件的默认行为

<p>只有 event.cancelable 属性为 true 的事件，才能够通过 preventDefault() 方法取消默认行为</p>

###### event.stopImmediatePropagation()

* 描述：与 event.stopPropagation() 一样，可以阻止事件冒泡，除此之外，还能阻止执行该语句之后的所有事件监听

#### IE特有

###### IE中获取事件对象的方法

<p class="tip">IE中获取事件对象的方法与绑定事件的方式有关</p>

1、DOM0级绑定事件的方式，即 `el.onclick = function () {}`，其事件对象通过window获取：

```js
el.onclick = function () {
    window.event
}
```

2、DOM2级绑定事件对象的方式，即 `el.attachEvent('click', function (event) {})`，既可以通过window获取，也可以通过event参数获取：

```js
el.attachEvent('click', function (event) {
    window.event
    // 或
    event
})
```

##### 属性

###### event.srcElement
* 读写特性：只读
* 描述：与规范中的 event.target 属性相同。

###### event.returnValue
* 读写
* 描述：默认为 `true`，如果将其设为 `false` 即可取消事件默认行为，相当于规范中的：`event.preventDefault()`

###### event.cancelBubble
* 读写
* 描述：默认为 `false` ，如果设为 `true` 即可取消事件冒泡，相当于规范中的：`event.stopPropagation()`

#### 事件总结

<p class="tip">
在规范中，事件处理函数的this对象始终等于 event.currentTarget 属性，但在IE中就不一定。比如：使用 attachEvent 绑定的事件处理函数是在全局作用域中运行的，所以this对象指向window，而不是 event.srcElement
</p>

###### 对照表

```
| standard          | IE                    |
| -------------     | -------------         |
| target            | srcElement            |
| preventDefault()  | returnValue = false   |
| stopPropagation() | cancelBubble = true   |
```

## 五、事件类型及讲解

#### UI事件

###### load

```
window上触发：
    当页面完全加载完，包括所有图像、js文件、css文件、<object>内嵌对象等等
<img>图片：
    当图片加载完成后触发
<script>/<link>：
    当js文件或css文件加载成功后
    注意：<script>标签只能使用HTML内联属性添加事件的方式才能生效
```

###### resize

```
window上触发：
    当窗口大小改变时
```

###### scroll

```
window上触发：
    当滚动页面时
在可滚动元素上触发：
    当滚动可滚动的元素时
```

#### 焦点事件

###### focus

当元素获得焦点时触发，不冒泡，但是可以在捕获阶段触发

###### blur

当元素失去焦点时触发，不冒泡，但是可以在捕获阶段触发

#### 鼠标与滚轮事件

###### click

点击鼠标左键时触发

###### dblclick

双击鼠标左键

###### mousedown

按下鼠标任意按钮时触发

###### mouseup

释放鼠标按钮时触发

###### mouseenter/mouseleave 与 mouseover/mouseout 的区别

```
    mouseenter 只会在鼠标在元素外部进入元素内部时触发，如果该元素有子节点，当移入其子节点内部时，会在捕获阶段在该节点触发，当鼠标再从子节点移出到该节点时，不会再触发。

    父元素绑定事件，子元素溢出父元素，括号的数字代表 event.eventPhase 的值
    1、先移入子元素
        mouseover: 事件在捕获阶段触发一次（1）
        mouseenter: Firefox、chrome 先在 处于目标阶段触发一次 再在 捕获阶段触发一次（2-1）
                    Safari 先在 捕获阶段触发 再在 处于目标阶段触发 （1-2）

    1.2、从子元素直接移出到父元素
        mouseover: 事件在处于目标阶段触发（2）
        mouseenter: 不触发事件

    1.3、再从父元素直接移动到子元素
        mouseover: 在捕获阶段触发（1）
        mouseenter: 在捕获阶段触发（1）

    2、先移入父元素
        mouseover: 在处于目标阶段触发（2）
        mouseenter: 在处于目标阶段触发（2）

    2.1、再从父元素直接移动到子元素
        mouseover: 在捕获阶段触发（1）
        mouseenter: 在捕获阶段触发（1）

    总结：
        大前提：父元素绑定事件，子元素溢出父元素。如果只考虑在目标阶段触发的话，即 event.eventPhase 的值为2时，不妨把子元素覆盖的区域当做附加区，那么：

        mouseover触发时机: 鼠标从外部移入该元素，或从附加区移入该元素
        mouseout触发时机: 鼠标从该元素移到外部，或从该元素移到附加区

        mouseenter触发时机: 鼠标从外部移入附加区，或从外部移入该元素
        mouseleave触发时机: 鼠标从附加区移入外部，或从该元素移入外部

        mouseover 和 mouseout 的作用区域 = 该元素区域 - (该元素 与 附加区交集)
        mouseenter 和 mouseleave 的作用区域 = 该元素区域 和 附加区并集

        mouseover 先于 mouseenter 
        mouseout 先于 mouseleave

        鼠标事件 只有 mouseenter 和 mouseleave 不冒泡（但是可以在捕获阶段触发）

    mouseover 和 mouseout 的相关元素：
        1、标准
            event.relatedTarget
        2、IE8及以下
            event.fromElement
            event.toElement
```

###### 鼠标事件对象中的位置信息：

* 客户区坐标位置

    event.clientX/Y
* 页面坐标位置

    event.pageX/Y
    <p class="tip">
    注意：IE8及以下版本不支持 pageX/Y，不过可以通过 clientX/Y + scrollLeft/Top 计算
    </p>
    
* 屏幕坐标位置（相对电脑屏幕的坐标位置）

    event.screenX/Y

###### 修改键

```
event.shiftKey		// 按住 shift 键为true
event.ctrlKey		// 按住 Ctrl 键为true
event.altKey		// 按住 alt 键为true
event.metaKey		// Mac下按住 command 键为true，windows 按住 Windows 键为true
```

<p class="tip">注意：IE8及以下不支持 metaKey</p>

#### 键盘事件

###### keydown

按下键盘任意键时触发

###### keypress

按下键盘任意字符键时触发

###### keyup

松开键盘任意键时触发

<p class="tip">可以通过 event.keyCode 获取键码</p>

#### 文本事件

###### textInput

输入框，文本在插入输入框之前触发，`event.data` 按下的字符

#### HTML5事件

###### contextmenu

鼠标右键事件，常用于制定自定义菜单

###### beforeunload

在页面卸载之前触发，用来询问用户是否真的要离开该页面

<p class="tip">该事件在window上触发，调用此方法的必须将 提示信息设置为 event.returnValue 的值，并return 该值</p>

* 示例：

```js
window.addEventListener('beforeunload', function (event) {
    event.returnValue = 'what?'
    return 'what?'
}, false)
```

###### DOMcontentLoaded

形成完整DOM树之后触发

<p class="tip">注意：IE8及以下不支持</p>

###### pageshow

页面显示时触发，`load` 事件只会在第一次加载页面是触发，之后页面会被 `bfcache`（往返缓存）管理，通过前进后退按钮来显示页面时，`load` 事件并不会触发，但是 `pageshow` 事件会触发

###### pagehide

页面卸载时触发。

<p class="tip">注意：pageshow/pagehide 必须添加到 window对象上</p>

###### hashchange

`#` 号后面的字符串发生变化时，在window对象上触发。






















