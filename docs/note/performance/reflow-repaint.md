## 重排和重绘

#### 重排(reflow)

<p class="tip">重新构造渲染树(Render Tree)的过程，叫做重排</p>

当DOM元素的变化影响了几何属性（如：宽高、位置）时，浏览器需要重新计算元素的几何属性，同时其他元素的几何属性也可能受到影响，这个时候浏览器会使渲染树(Render Tree)中受影响的部分失效，并重新构造渲染树，这个过程叫重排。

> 浏览器在构造渲染树的时候，通常只需要遍历一次

###### 重排的触发时机

* 添加、删除、替换DOM节点
* 改变DOM元素的位置、尺寸、内容
* 浏览器窗口大小发生改变
* 页面初次渲染

#### 重绘(repaint)

浏览器绘制变化的部分到屏幕叫重绘

元素的变化不一定触发重排，但一定触发重绘

#### 渲染树变化队列和刷新

浏览器有自己的优化机制，即并不是每次变化都会触发重排，而是将变化缓冲在队列里，当变化达到一定数量后刷新变化队列，触发重排。但是我们在获取一下属性时，浏览器会强制更新变化队列触发重排，因为这些属性需要返回实时的值：

* offsetTop、offsetLeft、offsetWidth、offsetHeight

* scrollTop、scrollLeft、scrollWidth、scrollHeight

* clientTop、clientLeft、clientWidth、clientHeight

* getComputeStyle()、currentStyle(IE)

#### 性能优化（最小化重排和重绘）

在修改元素多种样式属性时，使用替换class名的方式 代替 使用js脚本逐个修改样式



