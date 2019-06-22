# 记录一下面试中关于CSS的问题
下面总结下我对问题的理解

## html中内联元素和块级元素的区别
+ 块级元素：独占一行，默认情况下，其宽度自动填满其父元素宽度；可以设置width、height属性；可以设置margin、padding属性
  代表元素有`h1~h6`/`table`全体/`div`/`section`...

+ 内联元素：相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，其宽度随元素的内容而变化；属性width、height、padding-top/padding-bottom、margin-top/margin-bottom会默认失效
  代表元素有`a`/`q`/`span`/`i`/`label`...

## 对html标签语义化的理解

### 什么是语义化
根据内容的结构化（内容语义化），选择合适的标签（代码语义化）**便于开发者阅读和写出更优雅的代码**的同时**让浏览器的爬虫和机器很好地解析**。

### 语义化作用
+ 呈现出很好地内容结构,即使没有CSS样式
+ 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
+ 方便其他设备监听/屏幕阅读设备/盲人阅读器
+ 方便团队协作开发

### 具体语义化标签
h1~h6/section/article/address/nav/header/footer

## CSS样式权重和优先级
在CSS样式中，每一种选择器都带有一定的等级，一共分为4个等级`0 0 0 0`其中每一个选择器表示不同阶级的一个等级如：
+ !important: 优先级最高,使用后无视其他优先级
+ 内联元素: `1 0 0 0`
+ ID选择器: `0 1 0 0`
+ 类选择器,属性选择器,伪类选择器: `0 0 1 0`
+ 元素,伪类元素: `0 0 0 1`
+ 通配符(*): `0 0 0 0`

网上有个说法之前也误导了我,都怪我没去实测——*内联样式 > 内部样式 > 外部样式* 这个的前提的建立在外部样式在内部样式之后,实际上这就是————**相同权重之下,样式离该dom元素越近的,则被录用**

例如：
```css
#id p{
  background-color: yellowgreen;
}/*权重为 1 0 0 1*/
#id p{
  background-color: pink;
}/*权重为 1 0 0 1*/
```
在该情况下会先使用后一种

```html
<style>
p{
  background-color: pink;
}
</style>
<link rel='stylesheet' href="cdd.css" type='text/css'>
<p></p>
```

link中外部引用样式为:
```css
p{
  background-color: yellowgreen;
}
```

因为此时样式权重相同,但外部引用的样式更靠近p元素,则录用外部样式表引用样式

## 两栏布局
见 [两栏布局]()

## 如何实现居中
见 [垂直水平居中]()

## 对盒子模型的理解
在一个文档中，每个元素都被表示为一个矩形盒子。每个盒子模型具有4个区域从内到外分别为`content`/`padding`/`border`/`margin`

### 标准盒子模型
即W3C中定义的盒子模型,这种情况下CSS中的`width`/`height`为盒子`content`区域的大小

css中的`box-sizing`属性对应`content-box`

### 怪异盒子模型
怪异盒子模型的`width`属性为`border`/`content`/`padding`三个属性的总和
怪异盒子模型会在IE中出现

css中的`box-sizing`属性对应`border-box`