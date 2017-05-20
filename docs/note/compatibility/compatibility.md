###### 一、IE6/7/8 不支持HTML5标签
```
	解决方案一：
		可以使用 document.createElement() 动态创建HTML5标签，如 header footer 等，但是在 IE6/7/8 看来，这种方法创建的标签属于自定义标签，自定义标签默认为内联元素，所以为了将一些元素转为块元素，还需加上如下js代码

		header{
			display: block;
		}
	解决方案二：
		使用一套成熟的js库，github地址：https://github.com/aFarkas/html5shiv
```

###### 二、IE6/7 不支持 BFC

这导致 在标准浏览器中原本能够触发BFC的属性，不能在IE6/7中正常触发。会有很多奇怪的问题，比如两个float的元素的内容会互相影响 等等。

不过IE有自己的一个东西，叫做 haslayout ，

但以上描述的问题在IE6/7中基本可以靠 触发IE的haslayout解决，如：zoom:1

###### 三、IE6下，如果子元素的宽高超过父级元素的宽高，父级元素被子元素撑开。

不过，一般不会有人这么写，更不建议这么写

###### 四、IE6不支持 display: inline-block; 

```
	解决方法：
    {
        *display: inline;
        zoom: 1;
    }
```

###### 五、IE6下div最小高度 19px
```
	解决办法：
    {
        overflow: hidden;
    }
```

###### 六、IE6下块儿元素浮动会产生双倍 margin 值
```
	解决办法：
		将块儿元素转为行内元素，即添加 display: inline;
```

###### 七、li下元素都浮动，那么在IE6/7下，每个li元素间会有4px的间隔
```
	解决方案：
		给 li 元素添加 vertical-align: top;
```

###### 八、IE6文字溢出bug，两个浮动元素中间有内敛元素或者注释节点，且其中一个浮动元素的宽度和父级宽度相差小于3px。那么这个浮动元素内的文本会被复制出一个“小尾巴”

###### 九、IE6/7 下，父级的 overflow: hidden; 包不住拥有相对定位(position: relative;) 的子元素
```
	解决方案：
		给父级也加相对定位 position: relative;
```

###### 十、在IE6下绝对定位元素父级的宽高是奇数时，绝对定位元素的right和bottom值会有1px的偏差。
```
	解决办法：避免绝对定位元素的父级宽高是奇数
```

###### 十一、在IE6下，绝对定位元素和浮动元素并列，绝对定位元素消失
```
	解决办法：不让其同级就可以了
```

###### 十二、IE6下input表单元素上下各有1px的间隙。
```
	解决办法，给input元素加浮动
```

###### 十三、IE6输入类型的表单背景图随着用户输入而移动的bug
```
	解决办法：在设置background属性时加 fixed：(background: url(img/a.jpg) no-repeat fixed;)
```

###### 十四、IE8不会重绘伪类元素，除非修改伪类元素的 content 值
```
	解决办法：定义新样式，修改伪类元素content的值。
```

###### 十五、IE9 不支持 dataset

```
解决办法：使用 getAttribute/setAttribute/removeAttribute/hasAttribute 代替
```

###### 十六、IE8及以下版本不支持 pageX/Y

```
解决办法：可以通过 clientX/Y + scrollLeft/Top 计算
```

###### 十七、IE8及以下不支持 metaKey

```
event.shiftKey        // 按住 shift 键为true
event.ctrlKey        // 按住 Ctrl 键为true
event.altKey        // 按住 alt 键为true
event.metaKey        // Mac下按住 command 键为true，windows 按住 Windows 键为true
```

###### 十八、IE8及以下不支持 DOMcontentLoaded

```
解决办法：使用 window.onload 代替
```

###### 十九、部分 Android 手机中，键盘事件获取 event.keyCode 异常

```
使用 textInput 事件，根据按键内容区分按键
```