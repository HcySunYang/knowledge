## BOX

* 概念：CSS布局的基本单位
* 解释：BOX是CSS布局的基本单位，元素的类型和dispaly属性，决定了这个元素的的BOX类型，BOX的类型分为：
	* 【block-level box】
		* display属性值为：block、list-item、table 的元素会生成 block-levle box，并且参与 block formatting context 布局
	* 【inline-level box】
		* display属性值为：inline、inline-block、inline-table 的元素会生成 inline-level box，并参与 inline formatting context 布局
	* 【run-in box】
		* CSS3中定义

## Formatting Context

* 概念：Formatting Context 【格式化上下文】
* 解释：它是一个决定如何渲染文档的容器，常见的 Formatting Context 如下：
	* 【Block formatting context】(BFC)
	* 【Inline formatting context】(IFC) 
	* 【Grid formatting context】(GFC)
	* 【Flex formatting context】(FFC)

## BFC

创建一个独立的渲染区域，并规定了 block-level box 如何布局，且与这个区域外部毫不相关

BFC布局规则如下(注意BFC只影响块儿级盒)：

* 内部Box按垂直方向一个接一个的放置
* Box垂直方向的距离由margin值决定，并且属于同一个BFC的两个相邻的box的margin值会重叠
* 每个元素的 margin-box 的左边与包含块儿 border-box 的左边相接触
* BFC的区域不会与浮动盒子重叠
* BFC就是一个隔绝的容器，容器里面的子元素不影响外面元素的布局，反之亦然
* 计算BFC的高度时，浮动元素也参与计算

以下CSS属性，将会触发BFC：

* 根元素，即 `<html>`
* `float` 属性值不为 `none`
* `position` 属性的值为 `absolute` 或 `fixed`
* `overflow` 属性值不为 `visible`
* `display` 属性值为 `inline-block`, `table-cell`, `table-caption`

## BFC的应用

* 自适应两栏布局
	* 利用的是 BFC 不与浮动元素重叠的特性
* 清楚浮动
	* 利用BFC内浮动元素也参与BFC高度计算的特性
* 解决margin折叠(传递)
	* 子元素的margin-top传递到父级
* 防止margin重叠
	* 因为BFC内相邻元素的margin值会重叠，如果给其中一个元素包一层，并设置为BFC，又因为BFC内子元素的布局与外部元素互不影响的特性，就可以解决重叠的问题

## IE haslayout

`IE` 是个奇葩，自己搞一个叫做 `haslayout` 的东西，类似 `BFC`，一般在 `IE` 中显示有问题的东西都可以通过触发 `haslayout` 来解决，触发方法有很多：

* `zoom` 属性设置为除 `normal` 以外的值
* `width/height` 除 `auto` 以外的值
* `float` 除 `none` 以外的值

