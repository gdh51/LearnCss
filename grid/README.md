# grid

该布局为网格布局，它擅长将一个区域划分为几个主要的区域，然后定义这些区域的大小、位置和层次信息等关系。

## 父容器属性

### display

网格布局的容器一共分为两种，一种为块元素的行为，一种为内联元素的行为：

- `grid`：生成块级网格
- `inline-grid`：生成内联网格

### grid-template-columns/grid-template-rows

在声明了盒模型的行为模式后，就需要通过以下两个属性来定义盒模型中每个单独的网格的大小。

`grid-template-columns`：设置网格各列的宽度，每列依次排列以空格分隔。
`grid-template-rows`：设置网格各行的高度，每行依次排列以空格分隔。
如：

```css
.container {
    /*第一、二、三列宽为300px*/
    grid-template-columns: 300px 300px 300px;
    /*第一行高为100px*/
    grid-template-rows: 100px;
}
```

当在网格轨道值之间留出空格时，网格线会自动分配正数和负数的名称。也可以自己明确的指明网格线名称。如：

```css
.container {
    /*共有4条网格线，默认名称为1、2、3、4*/
    grid-template-columns: [line1] 300px [line2] 300px [line3] 300px [line4];
}
```

一个网格线可以定义多个值，取用其中一个即可

```css
.container {
    /*第三条网格线有两个名字，line3、line3-1，两个都能指向它*/
    grid-template-columns: [line1] 300px [line2] 300px [line3 line3-1] 300px [line4];
}
```

当我们定义多个重复值时，则可以用`repeat()`来简化定义：

```css
.container {
    grid-template-columns: repeat(3, 20px [col-start]);
}
```

等价于

```css
.container {
    grid-template-columns: 20px [col-start] 20px [col-start] 20px [col-start];
}
```

当多行共享相同名称，则可以通过其网格线名称和计数来引用它们。

```css
.item {
    /*引用名为col-start且*/
    grid-column-start: col-start 2;
}
```

在设置固定的宽度、高度后可用剩余的空间可以用类似与`flex-grow`属性的`fr`单位来等比例分配剩余空间。如：

```css
.container {
    grid-template-columns: 1fr 2fr 1fr;
}
```

以上将列分为宽度为 1：2：1 大小的空间。

```css
.container {
    grid-template-columns: 1fr 50px 1fr;
}
```

以上的剩余空间为 `总-50px` ，在此基础上按 `1：1` 分配

### grid-template-areas

通过引用`grid-area`属性指定的网格区域名称来定义网格模板。重复网格区域的名称导致内容跨越这些单元格。一个点号（.）代表一个空单元格。这个语法本身可视作网格的可视化结构。可用的值为：

- `网格区域名称`：由网格项的`gird-area`指定的网格区域名称
- `.`：代表一个空白网格单元
- `none`：不定义网格区域

```css
.item-a {
  grid-area: header;
  background-color: green;
}
.item-b {
  grid-area: main;
  background-color: red;
}
.item-c {
  grid-area: sidebar;
  background-color: black;
}
.item-d {
  grid-area: footer;
  background-color: aquamarine;

.container {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas:
  "header header header header"
  "main main . sidebar"
  "footer footer footer footer";  /*显示如这里的布局一样*/
}
```

Note：声明的每一行的网格数相同;用`.`表示时,只要没用空格分隔则`...`等价于`.`;当使用该命名视图时,会自动生成网格线,网格线名称为 `视图名-start/end`

### grid-template

定义`grid-template-rows`、`grid-template-columns`、`grid-template-areas`的简写属性。有以下值：

- `none`：使用三个值的初始值
- `grid-template-rows/grid-template-columns`：设置行高与列宽并设置`grid-template-areas`为`none`(注意带/)
    例如下面：

```css
.container {
    grid-template:
        [row1-start] 'header header header' 25px [row1-end]
        [row2-start] 'footer footer footer' 25px [row2-end]
        / auto 50px auto;
}
```

表示等价于：

```css
.container {
    grid-template-rows: [row1-start] 25px [row1-end row2-start] 25px [row2-end];
    grid-template-columns: auto 50px auto;
    grid-template-areas:
        'header header header'
        'footer footer footer';
}
```

Note：`grid-template` 不会重置隐式网格属性（`grid-auto-columns`,`grid-auto-rows`,和 `grid-auto-flow`）,建议使用 grid

### column-gap/row-gap

指定网格线的大小，相当于行间距。这个值只会产生在相邻的网格之间(即和父容器边缘没有间距)

### gap

`column-gap/row-gap`的简写，分别指代`row-gap`与`column-gap`

### justify-items

沿着 inline(行)轴线对齐网格项(设置每个网格项内部的项目位置)

- `start`：将网格项对齐单元格的左侧边缘位置
- `end`：将网格项对齐单元格的右侧边缘位置
- `center`：将网格项对齐单元格的中间位置
- `stretch`：默认值，填充单元格宽度

### align-items

沿着 block(列)轴线对齐网格项(设置每个网格项内部的项目位置)

- `start`：将网格项对齐单元格的上侧边缘位置
- `end`：将网格项对齐单元格的下侧边缘位置
- `center`：将网格项对齐单元格的中间位置
- `stretch`：默认值，填充单元格高度

### place-items

`justify-items/align-items`的简写，分别表示`align-items`与`justify-items`(**设置每个网格项内部的项目位置**)

### justify-content

表示父容器内**网格总体**沿 inline(行)的的对齐方式。
可能有的值：

- `start`：与父容器左侧相切
- `end`：与父容器右侧相切
- `center`：处于父容器中间部分
- `stretch`：调整网格项宽度使其填充整个父容器(未设置网格项宽度情况下)
- `space-around`：最左右两侧网格项离父容器的距离是中间网格项的一半
- `space-between`：最左右的网格项与父容器左右相切，其余网格项间距平分空隙
- `space-evenly`：每个网格(包括与父容器边界)的间距相等

### align-content

设置父容器内**网格总体**沿 block(列)轴的对齐方式
可能有的值：

- `start`：与父容器顶部相切
- `end`：与父容器底部相切
- `center`：处于父容器中间部分
- `stretch`：调整网格项高度使其填充整个父容器(未设置网格项宽度情况下)
- `space-around`：最上下两侧网格项离父容器的距离是中间网格项的一半
- `space-between`：最上下的网格项与父容器上下相切，其余网格项间距平分空隙
- `space-evenly`：每个网格(包括与父容器边界)的上下间距相等

### place-content

`align-content/justify-content`的简写，分别表示该两个值

### grid-auto-columns/grid-auto-rows

指定任何自动生成的网格轨道(隐式网格轨道)的大小。当网格中的网格项多于单元格时，或者当网格项位于显式网格之外时，就会创建隐式轨道。
如下，指定不存在网格线位置的网格项：

```css
.container {
    grid-template-columns: 60px 60px;
    grid-template-rows: 90px 90px;
}
.item-a {
    grid-column: 1 / 2;
    grid-row: 2 / 3;
}
.item-b {
    grid-column: 5 / 6;
    grid-row: 2 / 3;
}
```

网格项 b 所在位置未定义，引用的网格线不存在，所以创建宽度为 0 的隐式网格轨道来填补空缺。这是可以指定标题中属性值来定义隐式轨道的大小。

### grid-auto-flow

如果你有一些没有明确放置在网格上的网格项(grid items)，自动放置算法 会自动放置这些网格项。该属性控制自动布局算法如何工作。有以下值：

-   `row`：告诉自动布局算法依次填充每行，根据需要添加新行 （默认）
-   `column`：告诉自动布局算法依次填入每列，根据需要添加新列
-   `dense`：告诉自动布局算法在稍后出现较小的网格项时，尝试填充网格中较早的空缺
    Note：`dense`只会更改网格项的可视顺序，并可能导致它们出现乱序，这对可访问性不利。

### grid

在一个声明中设置所有以下属性的简写： grid-template-rows, grid-template-columns, grid-template-areas, grid-auto-rows, grid-auto-columns, 和 grid-auto-flow 。(注意：您只能在单个网格声明中指定显式或隐式网格属性)
值的分别有：

-   `none`：将所以值设置为初始值
-   `<grid-template>`：即`grid-template`的简写
-   `<grid-template-rows> / [ auto-flow && dense ]<grid-auto-columns>`
-   `[ auto-flow && dense? ] <grid-auto-rows>? / <grid-template-columns>`

## 子容器 网格项属性

### Note:float，display: inline-block，display: table-cell，vertical-align 和 column-\* 属性对网格项无效。

### grid-column-start/grid-column-end/grid-row-start/grid-row-end

通过引用指定的网格线来指定网格项在网格中的位置。`start`表示开始的网格线，`end`表示结束的网格线
可以用以下值来引用：

-   `line`：数字或名称来引用网格线
-   `span <munber>`：该网格项将跨越所提供的网格轨道的数量
-   `span <name>`：该网格项将跨越到所提供的网格轨道的名称
-   `auto`：表示自动放置，自动跨度，默认会扩展一个网格轨道的宽度或者高度

```css
.item-a {
    grid-column-start: 2; /*网格项从第二条列线开始*/
    grid-column-end: five; /*网格项到第五条列线结束*/
    grid-row-start: row1-start; /*网格项从名为row1-start行线开始*/
    grid-row-end: 3; /*网格项到第三条行线结束*/
}
```

### grid-column/grid-row

分别为 `grid-column-start` + `grid-column-end` 和 `grid-row-start` + `grid-row-end` 的简写形式。
以`grid-column-start / grid-column-end`来表示开始和结束的位置

### grid-area

为网格项提供一个名称以便被网格容器`grid-tamplate-areas`属性创建的模版引用。该属性也可以作为`grid-row-start` + `grid-column-start` + `grid-row-end` + `grid-column-end` 的简写。

```css
.item-d {
    grid-area: 1 / col4-start / last-line / 6; /*列线从第一条开始到名为col4-start  行线从名为last-line开始到第6条线结束*/
}
.item-d {
    grid-area: header; /*直接分配网格布局中名为header的部分*/
}
```

### justify-self

定义该网格项内内容沿 inline(行)方向的对齐方式。权值大于`justify-items`。
拥有以下值：

-   `start`：将网格项对齐单元格的左侧边缘位置
-   `end`：将网格项对齐单元格的右侧边缘位置
-   `center`：将网格项对齐单元格的中间位置
-   `stretch`：默认值，填充单元格宽度

### align-self

定义该网格项内内容沿着 block(列)轴线对齐网格项。权值大于`align-items`。

-   `start`：将网格项对齐单元格的上侧边缘位置
-   `end`：将网格项对齐单元格的下侧边缘位置
-   `center`：将网格项对齐单元格的中间位置
-   `stretch`：默认值，填充单元格高度

### place-self

`justify-self/align-self`的简写，分别表示`align-self`与`justify-self`
