# 关于清楚浮动

切记，要根据场景和需求，灵活运用原则发展出不同的清楚浮动的方法，而不要死记或拘泥于文中提到的方法

## 为什么要清楚浮动

直接看一个例子：

html：

```html
<div class="topDiv">
    <div class="floatDiv">float left</div>
    <div class="textDiv">...</div>
</div>
<div class="bottomDiv">...</div>
```

```css
.topDiv {
    width: 500px;
    border: 2px solid black;
}
.floatDiv {
    width: 100px;
    height: 100px;
    border: 2px dotted red;
    color: red;
    margin: 4px;
    float: left;
}
.bottomDiv {
    width: 500px;
    height: 100px;
    margin: 5px 0;
    border: 2px dotted black;
}
.textDiv {
    color: blue;
    border: 2px solid blue;
}
```

![上例样式](https://notebook-images.oss-cn-chengdu.aliyuncs.com/float/float-1.png)

上面这个布局很显然是有问题的，但是它可能存在如下问题：

* 文字围绕浮动元素排版，但我们可能希望文字排列在浮动元素下方，或者，我们并不希望 .textDiv 两边有浮动元素存在
* 浮动元素排版超出了其父级元素，父元素的高度出现了塌缩，若没有文字高度支撑，不考虑边框，父元素高度会塌缩成零
* 浮动元素影响了其父元素的兄弟元素的排版，因为浮动元素脱离了文件流。

解决方案如下：

* 第一个问题的解决方案：需要消除 .textDiv 周围的浮动
* 第二个第三个问题：因为父元素的兄弟元素只受父元素的影响，就需要一种方法将父元素撑起来，将浮动元素包括在其中，避免浮动元素影响父元素外部的元素排列

## 清楚浮动的方法

### 利用 clear 样式

我们可以给需要清楚浮动的元素添加如下样式：

```css
.textDir {
    clear: left;
}
```

![清楚浮动后的渲染效果](https://notebook-images.oss-cn-chengdu.aliyuncs.com/float/float-2.png)

通过上诉样式，.textDiv 告诉浏览器，我的左边不允许有浮动的元素存在，请清除掉我左边的浮动元素。然而，因为浮动元素位置已经确定，
浏览器在计算 .textDiv 的位置时，为满足其需求，将 .textDiv 渲染在浮动元素下方，保证了 .textDiv 左边没有浮动元素。同时可以看出，
父元素的高度也被撑起来了，其兄弟元素的渲染也不再受到浮动的影响，这是因为 .textDiv 仍然在文档流中，它必须在父元素的边界内，
父元素只有增高其高度才能达到此目的，可以说是一个意外收获

clear 的值为 both 也有相同的效果，哪边不允许有浮动元素，clear 就是对应方向的值，两边都不允许就是 both

但是，如果 floatDiv 和 textDiv 交换一下位置，父元素就没有办法撑起来了

所以，为达到撑起父元素高度的目的，使用 clear 清楚浮动的方法还是有适用范围的，我们需要更加通用和可靠的方法

### 父元素结束标签之前插入清楚浮动的块级元素

在有浮动的父级元素的末尾插入了一个没有内容的块级元素 div

```html
<div class="topDiv">
    <div class="textDiv">...</div>
    <div class="floatDiv">float left</div>
    <div class="blankDiv"></div>
</div>
<div class="bottomDiv">...</div>
```

```css
.blankDiv {
    clear: both; // or left
}
```

其原理和上面 clear 的原理完全一致，但是需要强调的是，父级元素末尾添加的元素必须是一个块级元素

### 利用伪元素 clearfix

给父元素添加一个 clearfix 类

```css
.clearfix:after {
    content: '.';
    height: 0;
    display: block;
    clear: both;
}
```

在父级元素的最后，添加了一个 :after 伪元素，通过清除伪元素的浮动，达到撑起父元素高度的目的

注意，该伪元素的 display 值为 block，他是一个不可见的块级元素，这也只不过是前一种清除浮动方法的另一种变形，其底层逻辑也完全一样

### 利用 overflow 清除浮动

```css
.topDiv {
    overflow: auto;
}
```

仅仅在父元素上新增 overflow: auto; 父元素的高度立即被撑起，将浮动元素包裹在内。

看起来浮动被清除了，浮动不再会影响到后续元素的渲染。严格讲，这和清除浮动没有一点关系，因为不存在哪个元素的浮动被清除。

其实，这里 overflow 值还可以是除了 visible 之外的任何有效值，它们都能达到撑起父元素高度，清除浮动的目的

不过，有些会带来一些副作用：
* scroll 会导致滚动条始终可见
* hidden 会使得超出边框部分不可见

### 为什么 overflow 可以清除浮动？ 块格式化上下文 BFC

要清楚这个原理，有一个概念始终绕不过去，那就是块格式化上下文（BFC）

关于 BFC 更详细的部分可以直接查看 CSS 文档对于 [BFC的定义](https://www.w3.org/TR/CSS2/visuren.html#block-formatting)

块格式化上下文 BFC 是 Web 页面的CSS 可视化渲染的一部分，他是一块区域，规定了内部块箱的渲染方法，以及浮动相互之间的影响关系

BFC 有如下特点：

1. BFC 就像是一道屏障，隔离出了 BFC 内部和外部，内部和外部区域的渲染相互之间不影响，BFC 有自己的一套内部子元素渲染的规则，不影响外部渲染，也不受外部渲染影响
2. BFC 的区域不会和外部浮动盒子的外边距区域发生叠加，也就是说，外部任何浮动元素区域和 BFC 区域是泾渭分明的，不可能重叠
3. BFC 在计算高度的时候，内部浮动元素的高度也要计算在内，也就说，即使 BFC 区域内只有一个浮动元素，BFC 的高度也不会发生塌缩，高度是大于等于浮动元素的高度的
4. HTML 结构中，当构建 BFC 区域的元素紧跟着一个浮动盒子时，即，是该浮动盒子的兄弟节点，BFC 区域会首先产生尝试在浮动盒子的旁边渲染，但若宽度不够，就在浮动元素的下方渲染

有了这几点，就可以解释为什么 overflow 值不为 visible 可以清除浮动了

当元素设置了 overflow 样式，且值不为 visible 时，该元素就创建了一个 BFC，从而撑起了父元素，达到了清除浮动的目的

注意，能够构建 BFC 的方法有很多，但是要考虑到每种方法的副作用，所以要根据不同的情况，使用不同的方法
