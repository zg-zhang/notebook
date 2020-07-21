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

![上例样式](https://notebook-images.oss-cn-chengdu.aliyuncs.com/float-demo.png)

## 清楚浮动的方法
