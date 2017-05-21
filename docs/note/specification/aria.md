## 无障碍访问与ARIA

#### 无障碍访问

<p class="tip">概念：通俗的讲就是一句话，没有任何因素能够对你访问该网站产生障碍</p>

如果鼠标坏了，那么使用键盘应该能够正常访问，反之亦然，存在视力障碍的用户应该能够通过声音访问该网站，这就是所谓的无障碍访问。

例如使用“屏幕阅读器“访问网站时，软件会阅读屏幕的内容以声音的方式传达给视力障碍的用户，这个时候，我们的HTML页面要保证可访问性，例如该使用`<a>` 标签的地方使用 `<a>` 标签，如果你想要使用 `<a>` 标签充当按钮，记得要在 `<a>` 标签上添加 `role="button"` 属性，使得屏幕阅读器准确的访问网站，不至于误导用户。

#### ARIA

<p class="tip">ARIA(Accessible Rich Internet Applications[可访问的富互联网应用])</p>

[ARIA规范](https://www.w3.org/TR/html-aria/)

ARIA 分为三部分：

* role属性

```css
role="tab",
role="button",
role="radio",
role="checkbox",
role="link",
…
```

* aria属性

```css
aria-haspopup,
aria-label,
aria-owns
…
```

* aria状态属性

```css
aria-checked,
aria-checked,,
aria-selected,
aria-expanded,
aria-hidden,
aria-invalid,
…
```