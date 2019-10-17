# BFC
`BFC(block formatting context)`即我们常说的块级格式化上下文，常见的还有IFC、GFC、FFC...(自行百度)。

## 特征
BFC具有以下特点：
1. BFC容器内的元素不影响外界的布局(可以用于解决垂直方向`margin`重叠问题)
2. BFC中的`float`元素要参与高度的计算
3. BFC元素不会与`float`元素重合
4. BFC内部的块级元素会在垂直方向上排序

## 如何成为BFC
以下属性可以成为BFC元素：
1. `overflow`不为`visible`
2. `display`为内联相关属性如：`inline-block`、`inline-flex`、`table-cell`等
3. `position`不为`static`与`relative`
4. `float`不为`none`

## 作用
由上面的特征，我们可以得出其作用：
1. 消除垂直方向上`margin`重叠的问题
2. 解决`float`元素不会撑起父元素问题
3. 解决`float`元素与块级元素重叠问题