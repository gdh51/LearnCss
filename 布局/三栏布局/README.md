# 布局

## 三栏布局

html 文件结构(除特殊声明都应用以下结构书写)

```html
<div class="container">
    <div class="left"></div>
    <div class="center"></div>
    <div class="right"></div>
</div>
```

要求中间自适应左右固定

### 绝对定位写法

```css
.container {
    position: relative;
}
.container div {
    position: absolute;
    height: 100px;
}
.left {
    left: 0;
    width: 100px;
    background-color: green;
}
.right {
    right: 0;
    width: 100px;
    background-color: blue;
}
.center {
    left: 100px;
    right: 100px;
    /* padding:0 100px; */
    background-color: aqua;
}
```

### float 写法

```css
.left {
    float: left;
    width: 100px;
    background-color: green;
}
.right {
    float: right;
    width: 100px;
    background-color: blue;
}
.center {
    padding: 0 100px;
    background-color: aqua;
}
div {
    height: 100px;
}
```

此时需要改变下 html 结构

```html
<div class="container">
    <div class="left"></div>
    <div class="right"></div>
    <div class="center"></div>
</div>
```

缺点：可能会高度塌陷

### table 布局

```css
.container {
    display: table;
    width: 100%;
}
.left {
    width: 100px;
    background-color: green;
}
.right {
    width: 100px;
    background-color: blue;
}
.center {
    background-color: aqua;
}
div {
    display: table-cell;
    height: 100px;
}
```

缺点：当其中一个单元格变高，其余也会跟随变高

### flex 布局

```css
.container {
    display: flex;
}
.left {
    width: 100px;
    background-color: green;
}
.right {
    width: 100px;
    background-color: blue;
}
.center {
    flex: 1;
    background-color: aqua;
}
div {
    height: 100px;
}
```

缺点：不兼容 IE8 及其以下

### grid 布局

```css
.container {
    display: grid;
    width: 100%;
    grid-template-columns: 100px auto 100px;
}
.container div {
    height: 100px;
}
.left {
    background-color: green;
}
.right {
    background-color: blue;
}
.center {
    background-color: aqua;
}
```

## 圣杯布局

和三栏布局类似但要求中间区域优先加载所以 html 结构必须为以下

```html
<div class="container">
    <div class="center"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>
```

```css
/* 方案1 */
.container {
    padding: 0 100px;
}
.container div {
    float: left;
    position: relative;
    height: 100px;
}
.left {
    left: -100px;
    margin-left: -100%;
    width: 100px;
    background-color: green;
}
.right {
    right: -100px;
    margin-left: -100px;
    width: 100px;
    background-color: blue;
}
.center {
    width: 100%;
    background-color: aqua;
}

/* 方案2 */
.container {
    position: absolute;
    padding: 0 100px;
}

.left {
    position: absolute;
    top: 0;
    left: 0;
    width: 100px;
    background-color: green;
}
.right {
    position: absolute;
    top: 0;
    right: 0;
    width: 100px;
    background-color: blue;
}
.center {
    width: 100%;
    background-color: aqua;
}
```

## 双飞翼布局

```html
<div class="container">
    <div class="center">
        <div class="inner">Main</div>
    </div>
    <!--优先加载-->
    <div class="left"></div>
    <div class="right"></div>
</div>
```

```css
.container div {
    float: left;
    height: 100px;
}
.left {
    width: 100px;
    margin-left: -100%;
    background-color: green;
}
.right {
    margin-left: -100px;
    width: 100px;
    background-color: blue;
}
.center {
    width: 100%;
    background-color: aqua;
}
.inner {
    float: none !important;
    margin: 0 100px;
}
```

区别于圣杯布局就是通过内部 `inner` 来自适应大小
