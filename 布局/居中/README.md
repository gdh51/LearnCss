# 居中

总结一下各种的方位的居中方式,总体会以以下结构为基础,除 container 给出固定长宽外其他的待定

```html
<div class="container">
    <div class="outer">
        <div class="inner"></div>
    </div>
</div>
```

或最里面为内联元素

```html
<div class="container">
    <div class="outer">
        <p class="inner"></p>
    </div>
</div>
```

## 水平居中

当前只说明两种情况,其中`outer`指代父元素、`inner`指代子元素

```html
<div class="outer">
    <p class="inner"></p>
    OR
    <div class="inner"></div>
</div>
```

### 子元素为块级元素

当子元素为块级元素时,在设置了具体`height`与`width`属性时，只需在设置`margin: 0 auto;`即可做到水平居中

### 子元素为内联元素
内联元素(`display: inline/inline-block/inline-table/inline-flex`),只需设置父元素的`text-align: center;` 即可

## 水平垂直居中

### 已知父级元素宽高，父级元素定位非 static，子元素定位为 position: absolute/fixed

```html
<style>
    .outer {
        position: relative;
        width: 500px;
        height: 500px;
        background-color: black;
    }
</style>
<div class="outer">
    <div class="inner"></div>
</div>
```

#### 利用 margin 与 left 与 top 垂直居中

```css
.inner {
    position: absolute;
    top: 50%; /*为父元素高度一半*/
    left: 50%; /*为父元素宽度一半*/
    witdh: 100px;
    height: 100px;
    margin-left: -50px; /*为子宽度一半的负值*/
    margin-top: -50px; /*为子高度一半的负值*/
}
```

#### 利通 margin + top/left/right/bottom

```css
.inner {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    width: 50px;
    height: 50px;
    margin: auto;
    background-color: aqua;
}
```

#### 利用 translate 属性：位移基于本元素宽高

该属性只兼容 IE9+

```css
.inner {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 50px;
    height: 50px;
    transform: translate(-50%, -50%);
    background-color: aqua;
}
```

### 子元素为内联元素

```html
<div class="outer">
    <span class="inner"></span>
</div>
```

#### 父元素设置 line-height 为 height 等高

```css
.outer {
    width: 100px;
    height: 100px;
    line-height: 100px;
}
```

#### 利用伪元素撑起容器，实现垂直居中

```css
.outer {
    width: 100px;
    height: 100px;
    text-align: center;
    background-color: pink;
}
.outer::after {
    content: '';
    display: inline-block;
    height: 100%;
    vertical-align: middle;
}
```

#### 父元素使用 display: table-cell; 与 vertical-align: middle; text-align: center;实现垂直居中

```css
.outer {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    width: 500px;
    height: 500px;
    background-color: black;
}
```

这种方法同样适用于子元素为块级元素,子元素水平居中像之前一样设置 margin 即可

但有一种特殊情况请看下面

##### 当父元素为浮动元素时，使用该属性会有点偏差

此时需要在父级元素之下设置一个中间容器

```html
<style>
    .container {
        display: table;
        float: left;
        height: 100px;
        width: 100px;
        background-color: green;
    }
    .outer {
        display: table-cell;
        vertical-align: middle;
        text-align: center;
        width: 100%;
        height: 100%;
        background-color: pink;
    }
</style>
<div class="container">
    <div class="outer">
        <span class="inner">111</span>
    </div>
</div>
```

### flex

较为简单,直接设置父元素属性如下即可

```css
.outer {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 100px;
    background-color: pink;
}
```

需要兼容 IE10+

### grid 布局 + margin: auto

```css
.outer {
    display: grid;
    witdh: 100px;
    height: 100px;
}
.inner {
    width: 50px;
    height: 50px;
    margin: auto;
}
```
