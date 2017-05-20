## transform 2D

#### 旋转   
* 单位：度数单位，常用角度(deg)
* 示例：
```css
transform: rotate(20deg);
```

#### 斜切  
* 单位：度数单位，常用角度(deg)
* 示例：
```css
transform: skew(20deg, 40deg);
transform: skewX(20deg);
transform: skewY(20deg);
```

#### 缩放  
* 单位：倍数，无需指定单位
* 示例：
```css
transform: scale(2, 1);
transform: scaleX(2);
transform: scaleY(2);
```

#### 位移  
* 单位：长度单位，常用(px)
* 示例：
```css
transform: translate(100px, 100px);
transform: translateX(100px);
transform: translateY(100px);
```

#### transform-origin 
* 描述：设置变换基点
* 示例：
```css
transform-origin: 关键字/百分比/距离单位;
```

<p class="tip">坑点：通过js的方式来设置变换基点（dom.style.WebkitTransformOrigin = '0 0';），不能快速同步给变换，在css中没问题</p>

#### 执行顺序

先写后执行，应该说 先写的变换 会影响 后边的变换

#### 矩阵 matrix(a, b, c, d, e, f)
* 默认值：matrix(1, 0, 0, 1, 0, 0)
    
    rotate / skew / scale / translate 等变换都是通过矩阵实现的，只不过是浏览器给我们封装好的函数

* 计算方法
```
x 轴位移：
    e = e + x
y 轴位移：
    f = f + y

x 轴斜切：
    c = Math.tan(Math.PI / 180 * x)
y 轴斜切：
    b = Math.tan(Math.PI / 180 * y)

x 轴缩放：
    a = a * x
    c = c * x
    e = e * x
y 轴缩放：
    b = b * y
    c = c * y
    f = f * y
旋转
    a = Math.cos(Math.PI / 180 * deg)
    b = Math.sin(Math.PI / 180 * deg)
    c = -Math.sin(Math.PI / 180 * deg)
    d = Math.cos(Math.PI / 180 * deg)
```

## transform 3D

#### perspective
* 描述：设置景深
* 单位：无
* 示例：
```css
perspective: 200;
```
* 注意：该属性要加给需要做3D变换的父级元素

#### perspective-origin
* 描述：景深基点，可以理解为视线方向
* 示例：
```css
perspective-origin: 关键字/距离单位;
```

#### transform-style
* 描述：当元素做3D变换时是否保留子元素的3D变换
* 示例：
```css
transform-style: flat; /* 不保留 */
transform-style: preserve-3d; /* 保留 */
```

#### backface-visibility
* 描述：隐藏背面
* 示例：
```css
backface-visibility: visible; /* 可见 */
backface-visibility: hidden; /* 不可见 */
```

#### 3D 旋转
* 单位：度数单位，常用角度(deg)
* 示例：
```css
/* 围绕Z轴旋转 */
transform: rotateZ();
/* XYZ结合写法 */
transform: rotate3D();
```

#### 3D 位移
* 单位：长度单位，常用单位(px)
* 示例：
```css
/* Z轴位移 */
transform: translateZ();
/* XYZ结合写法 */
transform: translate3D();
```












