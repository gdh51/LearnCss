# 网页是如何渲染的

作为一个前端开发人员，我们有必要知道一个网页是如何渲染的。在浏览器渲染时，布局和样式以及我们的`JS`脚本都对渲染的优化造成影响，所以我们必须在书写它们时就有意的来避免一些后期性能上可能会带来的问题。

> 本文不会深入浏览器底层来讲述其如何渲染，只会介绍一些不同浏览器中通用的渲染原则。毕竟不同的浏览器底层实现不同

## 浏览器是如何渲染一个页面的

首先总体浏览器下一个浏览器是如何渲染一个页面的：

1. 从服务器获取到返回的`HTML`文件，并解析生成`DOM`（文档对象模型`Document Object Model`）。
2. 加载并解析样式表（`Styles`），并转化为`CSSOM`（`CSS Object Model`）
3. 基于`DOM`和`CSSOM`，生成一个渲染树 🌲（`Rendering Tree`）。它即等会要渲染的对象的集合。（基于`Webkit`的浏览器称渲染对象为`render object`或`renderer`；基于`Gecko`的浏览器称为`frame`）渲染 🌲 反应了`DOM`的结构（不包括不可见的元素，比如`head`元素或`display: none;`的元素）。一个文本字符串也表示一个独立的节点（`renderer`）。每一个渲染对象都包含了其相对应的`DOM`对象（或一个文本节点）以及计算好的样式。简而言之，渲染 🌲 就是一群可视`DOM`节点的呈现。
4. 计算渲染 🌲 中每一个（元素）节点的坐标，这个过程也称为布局（`Layout`）（我们常用的说法也就是重排`Reflow`）。浏览器使用一种流（`flow`）方法，只需要一次就可以计算出所有节点的位置。（表格这种特殊的节点需要多次计算）
5. 最后将这些节点展示在浏览器中，这个过程称为绘制（`Paint`）

当用户与一个页面交互或脚本对页面进行修改，导致页面结构发生改变时，上述的过程就会重复的发生。

## Repaint(重绘)

当改变一个节点的样式，但不影响其节点的位置时（这些样式比如`background-color/color/border-color/visibility etc.`），浏览器仅会用新的样式重新绘制节点。

## Reflow(重排)

当一个改变影响了文档的结构、内容或元素的位置时，一次重新布局（`relayout/reflow`）就会发生。通常是由于以下改变导致的：

- `DOM`节点的操作（添加、删除、改变元素）
- 内容变化，包括表单控件中的文本变化
- 计算或改变`CSS`属性(比如访问`document.body.offsetHeight`)
  - 添加或移除一个层叠样式表（前提是这个样式表对文档造成了影响）
  - 为节点添加或删除`class`（这个`class`会对该元素的位置造成影响）
- 浏览器视窗的交互（比如改变其尺寸或滚动）
- 伪类元素的激活（`:hover`）

## 浏览器是如何优化渲染的

浏览器总是尽最大努力去将重排和重绘限制在仅改变的元素上。比如，一个拥有绝对/固定（`absolute/fixed`）定位的元素的尺寸改变，仅会对其自身以及其子元素造成影响，而同样的改变在一个静态定位（`statically`）的元素上会造成整个**后续**元素的重排。

另一个优化就是当我们运行一连串的`JS`代码时，浏览器会缓存在代码执行中的变动，并在所有代码执行完后，一次性应用这些变动，比如下面的代码只会造成一次重排和重绘：

```js
const body = document.body
body.style.padding = '10px'
body.style.color = 'red'
body.style.margin = '10px'
```

但是，如上面提到的，访问一个节点的属性会导致一次强制的重绘（`reflow`）。

>这很好理解，当我们访问一个元素的样式属性时，为了得到改属性的精准信息，必须要重新布局渲染次，以得到精准的信息

如果我们添加下面的代码，那么会造成两次重排：

```js
const body = document.body
body.style.padding = '10px'

// 读取元素的样式
body.style.padding

body.style.color = 'red'
body.style.margin = '10px'
```

所以，如果有读取属性的需求，应该经历在一起进行执行，这样可以对渲染的性能进行优化。

### 有时你需要一次强制重排

当我们制作一个动画时，假如我们需要对`margin-left`做一个动画，在动画最初，`margin-left`的值为`100px`，在动画的最终，我们需要将`margin-left`的值变为`50px`

我们应用动画的过渡类为：

```css
.has-transition {
   -webkit-transition: margin-left 1s ease-out;
      -moz-transition: margin-left 1s ease-out;
        -o-transition: margin-left 1s ease-out;
           transition: margin-left 1s ease-out;
}
```

那么添加动画的代码为：

```js
const el = document.querySelector('.ball')

el.style.marginLeft = '100px'

// 添加过渡样式
el.addClass('has-transition')

// 变更属性开始动画（实际不会开始动画）
el.style.marginLeft = '50px'

// 动画结束移除过渡类
el.addEventListener('transitioned', () => el.removeClass('has-transition'))
```

由于我们在之前知道，浏览器会将代码对于元素的操作整合到最后应用，所以上面相当于直接应用了`margin-left: 50px;`而不会有动画效果。所以我们必须要求其强制重排次来达到动画效果：

```js
const el = document.querySelector('.ball')

el.style.marginLeft = '100px'

// 添加过渡样式
el.addClass('has-transition')

// 获取浏览器属性，强制重排
document.body.offsetHeight

// 变更属性开始动画（实际不会开始动画）
el.style.marginLeft = '50px'

// 动画结束移除过渡类
el.addEventListener('transitioned', () => el.removeClass('has-transition'))
```

这样动画就能如期执行了

## 优化的可实践建议

下面有几条推荐的优化建议：

- 创建有效的`HTML`元素与`CSS`样式，并且制定`HTML`文档的编码。将层叠样式表放在`head`中，将`JS`脚本放在`body`元素的闭标签之前。
- 简化和优化`CSS`选择器（大多数使用预处理器的开发者忽视了这点），保持选择器的层级在一个最小的范围（一般来说最大三层），下面是选择器的查找性能排行：
    1. 识别器（`Identificator`）：`#id`
    2. 类（`Class`）：`.class`
    3. 标签（`Tag`）：`div`
    4. 兄弟选择器：`a + i`
    5. 父子选择器：`ul > li`
    6. 通用选择器：`*`
    7. 属性选择器：`input[type="text"]`
    8. 伪类选择器和伪类元素：`a:hover`

浏览器处理选择器是从右到左的，这就是为什么越靠近右边的选择器应该在性能处理上最快的一个——`#id`或`.class`

```css
div * { ... } // bad
.list li { ... } // bad
.list-item { ... } // good
#list .list-item { ... } good
```

- 尽量减少对`DOM`的操作。缓存`DOM`的信息并复用它们。尽量在操作`DOM`时将其进行“离线”处理。（所谓离线就是将`DOM`片段从文档中移除后进行操作，然后在将其插入到文档中）
- 对执行动画的元素使用`fixed/absolute`布局
- 通过添加`class`来对元素的样式进行操作
- 在滚动时禁用复杂的`:hover`动画

Learn and sum up from [What Every Frontend Developer Should Know About Webpage Rendering](http://frontendbabel.info/articles/webpage-rendering-101/#practical-advice-on-optimization)