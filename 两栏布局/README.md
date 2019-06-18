# 两栏布局
左边固定，右边自适应

总体结构为：
```html
<div class='container'>
  <div class='left'></div>
  <div class='right'></div>
</div>
```

## 利用float
```css
.container{
  width: 500px;
  overflow: hidden;
  background-color: black;
}
.left{
  float: left;
  height: 100px;
  width: 200px;
  background-color: pink;
}
.right{
  height: 100px;
  margin-left: 200px;
  width: auto;
  background-color: green;
}
```

## 利用position: absolute;
```css
.container{
  position: relative;
  height: 100px;
  width: 500px;
  background-color: black;
}
.left{
  position: absolute;
  height: 100px;
  width: 200px;
  background-color: pink;
}
.right{
  position: absolute;
  left: 200px;
  height: 100px;
  right: 0;
  background-color: green;
}
```

## flex布局
```css
.container{
  display: flex;
  height: 100px;
  width: 500px;
  background-color: black;
}
.left{
  width: 200px;
  background-color: pink;
}
.right{
  flex: 1;
  background-color: green;
}
```