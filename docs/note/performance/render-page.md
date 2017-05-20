## 浏览器渲染页面的过程

浏览器渲染页面的过程如下图：

<img src="../../asset/img/render.png"/>

#### 一、解析HTML创建DOM Tree

浏览器解析HTML文档，并构造一颗DOM树(DOM Tree)

#### 二、解析CSS计算样式数据

浏览器构造DOM树的同时，还会解析CSS样式并计算最终的样式数据，生成样式规则。

#### 三、构造渲染树(Render Tree)

根据 `DOM Tree` 和 样式数据构造一颗渲染树(Render Tree)

渲染树会忽略不需要渲染的DOM元素（如：head标签、display值为none的元素）

#### 四、layout布局

当渲染树构造完成后，浏览器会对渲染树进行布局，即分配固定的坐标点给DOM元素。

#### 五、paint绘制

布局完成后，浏览器将绘制最终的界面给用户
