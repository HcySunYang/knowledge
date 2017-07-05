## CSS选择器整理

#### 基本选择器

| 选择器         | 名称           | 描述  |
| ------------- |:-------------:| :----- |
| *             | 通配选择器      | 选择文档中所有的HTML元素 |
| E             | 元素选择器      | 选择指定类型的HTML元素，例如：li、p等等 |
| #id           | ID 选择器      | 选择ID属性值为 “id” 的元素 |
| .class        | 类选择器       | 选择class属性值为 “class” 的一组元素 |
| selector1, selector2        | 群组选择器       | 将每一个选择器匹配的集合合并 |

#### 层次选择器

| 选择器         | 名称           | 描述  |
| ------------- |:-------------:| :-----|
| selector1 selector2  | 后代选择器(包含选择器) | 选择selector2所匹配的一组元素，且selector2是selector1的后代元素 |
| selector1 > selector2  | 子选择器 | 选择selector2所匹配的一组元素，且selector2是selector1的直接子元素 |
| selector1 + selector2  | 相邻兄弟选择器 | 选择selector2所匹配的元素，且selector2位于selector1的后面 |
| selector1 ~ selector2  | 通用选择器 | 选择selector2所匹配的一组元素，且该组元素位于selector1后面 |

#### 伪类选择器

##### 动态伪类选择器

| 选择器             | 名称                | 描述  |
| ------------------|:-------------------:| :-----|
| selector:link     | 连接伪类选择器   | 选择selector所匹配的元素，且该元素被定义了超链接并未被访问过，常用于a标签 |
| selector:visited  | 连接伪类选择器   | 选择selector所匹配的元素，且该元素被定义了超链接并已被访问过，常用于a标签 |
| selector:active   | 用户行为伪类选择器   | 选择selector所匹配的元素，且该元素被激活，常用于连接或按钮上 |
| selector:hover    | 用户行为伪类选择器   | 选择selector所匹配的元素，且用户鼠标停留在该元素上 |
| selector:focus    | 用户行为伪类选择器   | 选择selector所匹配的元素，且该元素获得焦点 |

##### 目标伪类选择器

| 选择器             | 名称                | 描述  |
| ------------------|:-------------------:| :-----|
| selector:target   | 目标伪类选择器   | 选择selector所匹配的元素，且该元素的ID值等于页面URL片段标识符(即#号后面的值)的值 |

##### 语言伪类选择器

| 选择器             | 名称                | 描述  |
| ------------------|:-------------------:| :-----|
| selector:lang(language)   | 语言伪类选择器   | 选择selector所匹配的元素，且该元素指定了值为 language 的 lang 属性，多用于多语言网站不同样式的处理上 |

##### UI元素状态伪类选择器

| 选择器             | 名称                | 描述  |
| ------------------|:-------------------:| :-----|
| selector:checked   | 选中状态伪类选择器   | 匹配选中的单选按钮/复选按钮 |
| selector:enabled   | 启用状态伪类选择器   | 选择selector所匹配的表单元素，其该元素为启用状态 |
| selector:disabled   | 禁用状态伪类选择器   | 选择selector所匹配的表单元素，其该元素为禁用状态 |

##### 结构伪类选择器

| 选择器                  | 描述                |
| -----------------------|:-------------------:|
| selector:first-child   | 选择selector所匹配的元素，且该元素是其父元素的第一个子元素(不算文本节点，也不区分元素类型)，等价于 selector:nth-child(1)   |
| selector:last-child    | 选择selector所匹配的元素，且该元素是其父元素的最后一个子元素(不算文本节点，也不区分元素类型)，等价于 selector:nth-last-child(1)   |
| selector:nth-child(n)  | 选择selector所匹配的元素，且该元素是其父元素的第n个子元素(不算文本节点，也不区分元素类型)，其中 n 的值可以使正数(1、2、3...)，也可以是关键字(even、odd)，也可以是公式(2n+1、2n-1...)，且 n 的起始值是1而不是0 |
| selector:nth-last-child(n)  | 选择selector所匹配的元素，且该元素是其父元素的倒数第n个子元素(不算文本节点，也不区分元素类型) |
| selector:first-of-type  | 选择selector所匹配的元素，且该元素是其父元素的第一个特定类型的子元素(不算文本节点，区分元素类型) |
| selector:last-of-type  | 选择selector所匹配的元素，且该元素是其父元素的最后一个特定类型的子元素(不算文本节点，区分元素类型) |
| selector:nth-of-type(n)  | 选择selector所匹配的元素，且该元素是其父元素的第n个特定类型的子元素(不算文本节点，区分元素类型) |
| selector:nth-last-of-type(n)  | 选择selector所匹配的元素，且该元素是其父元素的倒数第n个特定类型的子元素(不算文本节点，区分元素类型) |
| selector:only-child    | 选择selector所匹配的元素，且其父元素只有它一个子元素(不算文本节点，也不区分元素类型) |
| selector:only-of-type  | 选择selector所匹配的元素，且其父元素只有它一个特定类型的子元素(不算文本节点，区分元素类型) |
| selector:root          | 选择selector所匹配的元素所在文档的根元素，即html元素 |
| selector:empty         | 选择selector所匹配的元素，且该元素没有任何子元素(包括文本节点) |


##### 否定伪类选择器

| 选择器                  | 描述                |
| -----------------------|:-------------------:|
| selector1:not(selector2)   | 选择所有不包含selector2的selector1元素  |

#### 伪元素

| 选择器                  | 描述                |
| -----------------------|:-------------------:|
| selector::first-letter   | 选择文本块的第一个字母  |
| selector::first-line   | 选择文本块的第一行文本  |
| selector::before   | 用来为selector所匹配的元素的所有子元素前面插入内容，插入的内容不会成为DOM的一部分，但是依然可以设置样式  |
| selector::after   | 用来为selector所匹配的元素的所有子元素后面插入内容，插入的内容不会成为DOM的一部分，但是依然可以设置样式  |
| selector::selection   | 匹配被鼠标选中的文本  |

<p class="tip">双冒号代表伪元素，单冒号代表伪类</p>

#### 属性选择器

| 选择器                  | 描述                |
| -----------------|:-------------------:|
| selector[attr]   | 选择selector所匹配的元素，且该元素拥有attr属性。可以省略selector，表示匹配所有拥有attr属性的元素  |
| selector[attr=val]   | 选择selector所匹配的元素，且该元素拥有值为val的attr属性。可以省略selector，表示匹配所有拥有值为val的attr属性的元素  |
| selector[attr~=val]   | 选择selector所匹配的元素，且该元素attr属性值具有多个空格分隔的值，其中一个值等于val。可以省略selector|
| selector[attr*=val]   | 选择selector所匹配的元素，且该元素attr属性值的任意位置包含val。可以省略selector|
| selector[attr^=val]   | 选择selector所匹配的元素，且该元素attr属性值以val开头。可以省略selector|
| selector[attr$=val]   | 选择selector所匹配的元素，且该元素attr属性值以val结尾。可以省略selector|

除了上表中介绍的属性选择器之外，还有一个属性选择器没有写在其中，如下：

```css
/* 选择selector所匹配的元素，且该元素拥有值以val或val-开头的attr属性，可以省略selector */
selector[attr|=val] {

}
```

你可能会问我为什么没有这个选择器写在上面的表格中，是这样的，这个选择器中有字符 `|`，这个字符在markdown表格中是表格的分界线。日了狗了.....