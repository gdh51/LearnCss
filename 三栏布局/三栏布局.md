# 布局

## 三栏布局
html文件结构(除特殊声明都应用以下结构书写)
```html
<div class='container'>
    <div class='left'></div>
    <div class='center'></div>
    <div class='right'></div>
</div>
```
要求中间自适应左右固定

### 绝对定位写法
```css
.container{
  position: relative;
}
.container div{
  position: absolute;
  height: 100px;
}
.left{
  left: 0;
  width: 100px;
  background-color: green;
}
.right{
  right: 0;
  width:100px;
  background-color: blue;
}
.center{
  left: 100px;
  right: 100px;
  /* padding:0 100px; */
  background-color: aqua;
}
```

### float写法
```css
.left{
  float: left;
  width: 100px;
  background-color: green;
}
.right{
  float: right;
  width:100px;
  background-color: blue;
}
.center{
  padding:0 100px;
  background-color: aqua;
}
div{
  height: 100px;
}
```
此时需要改变下html结构
```html
<div class='container'>
  <div class='left'></div>
  <div class='right'></div>
  <div class='center'></div>
</div>
```
缺点：可能会高度塌陷

### table布局
```css
.container{
  display: table;
  width: 100%;
}
.left{
  width: 100px;
  background-color: green;
}
.right{
  width:100px;
  background-color: blue;
}
.center{
  background-color: aqua;
}
div{
  display: table-cell;
  height: 100px;
}
```
缺点：当其中一个单元格变高，其余也会跟随变高

### flex布局
```css
.container{
  display: flex;
}
.left{
  width: 100px;
  background-color: green;
}
.right{
  width:100px;
  background-color: blue;
}
.center{
  flex:1;
  background-color: aqua;
}
div{
  height: 100px;
}
```
缺点：不兼容IE8及其以下

### grid布局
```css
.container{
  display: grid;
  width: 100%;
  grid-template-columns: 100px auto 100px;
}
.container div{
  height: 100px;
}
.left{
  background-color: green;
}
.right{
  background-color: blue;
}
.center{
  background-color: aqua;
}
```


## 圣杯布局
和三栏布局类似但要求中间区域优先加载所以html结构必须为以下
```html
<div class='container'>
  <div class='center'></div>
  <div class='left'></div>
  <div class='right'></div>
</div>
```

```css
.container{
  padding: 0 100px;
}
.container div{
  float: left;
  position: relative;
  height: 100px;
}
.left{
  left: -100px;
  margin-left: -100%;
  width: 100px;
  background-color: green;
}
.right{
  right: -100px;
  margin-left: -100px;
  width: 100px;
  background-color: blue;
}
.center{
  width: 100%;
  background-color: aqua;
}
```

## 双飞翼布局
```html
<div class='container'>
  <div class='center'>
    <div class='inner'>Main</div>
  </div><!--优先加载-->
  <div class='left'></div>
  <div class='right'></div>
</div>
```

```css
.container div{
  float: left;
  height: 100px;
}
.left{
  width: 100px;
  margin-left: -100%;
  background-color: green;
}
.right{
  margin-left: -100px;
  width: 100px;
  background-color: blue;
}
.center{
  width: 100%;
  background-color: aqua;
}
.inner{
  margin: 0 100px 0 100px;
}
```
区别于圣杯布局就是通过内部inner来自适应大小