# FLIP

## 含义

`FLIP`即代表`First`/`Last`/`Invert`/`Play`。细分下来即：

- **First**：一个（一些）元素的动画的初始状态。
- **Last**：一个（一些）元素的动画的最终状态。
- **Invert**：现在你知道了整个元素的初始和最终状态，现在你将它们反转（`invert`）过来。比如一个元素初始位置在最终位置的上方`90px`的位置，那么你需要为其最终状态应用`transforms: -90px`来将其还原到最初的位置。（实际上它们在最终状态的位置）
- **Play**：最后添加动画过渡属性并移除我们在`Invert`中应用的反向变化。这样就会产生一个虚假的状态——从第初始状态到最终状态的变化

那么简单解释下就是，首先元素从初始位置`1`到最终位置`2`，在一般情况下是没有动画效果的（比如元素位置的变动）。那么假若我们在肉眼无法察觉的时间内，将元素从最终位置通过某些 CSS 属性，重新弄回到位置`1`（此时元素实际位置还是在`2`）。此时再为这些属性设置`transition`属性，并摘除刚刚添加的 CSS 属性，这样就能使其从位置`1` 开始到最终位置`2`过渡。

## 代码示例

下面用一个简单的实例来创建一个`FLIP`动画：

```js
// 假设其为一个绝对定位的元素，初始的top为0px
const el = document.querySelector('#box')

// 获取其初始状态
const firstStatus = el.getBoundingClientRect()

// 假设这里改变其状态
el.style.top = '10px'

// 从这里得出最终状态为
const finalStatus = el.getBoundingClientRect()

// 得出需要反转的状态
const dx = firstStatus.left - finalStatus.left,
    dy = firstStatus.top - finalStatus.top

// 为元素应用计算得出的样式，使其回到初始状态位置(注意使用会产生独立图层的属性，比如transform和opacity)
el.style.transform = `translate(${dx}px, ${dy}px)`

// 现在开始为其应用过渡属性，并在浏览器绘制下一帧时执行动画
requestAnimationFrame(function() {
    el.transition = 'all .3s ease'
    el.style.transform = '
})

// 这里可以用以下代码代替上面这个函数，因为它造成reflow来强制浏览器计算一次当前的布局
// document.body.offsetHeight

// 在过渡结束时，删除刚刚应用的过渡动画属性
el.el.addEventListener('transitionend', function() { el.transition = '' })
```

现在我们可以用原生[`Web Animations API`](https://drafts.csswg.org/web-animations/)来代替上述的动画过程

```js
// 假设其为一个绝对定位的元素，初始的top为0px
const el = document.querySelector('#box')

// 获取其初始状态
const firstStatus = el.getBoundingClientRect()

// 假设这里改变其状态
el.style.top = '10px'

// 从这里得出最终状态为
const finalStatus = el.getBoundingClientRect()

// 得出需要反转的状态
const dx = firstStatus.left - finalStatus.left,
    dy = firstStatus.top - finalStatus.top

const player = el.animate(
    [
        {
            transform: `translate(${dx}px, ${dy}px)`
        },
        {
            transform: `translate(0px, 0px)`
        }
    ],
    {
        duration: 300,
        easing: 'cubic-bezier(0,0,0.32,1)'
    }
)

// 动画结束时，你可以做的事
player.addEventListener('finish', () => {})
```

## 这样做的意义

`FLIP`在某些不知道具体变化位置的场景中非常实用。比如一个元素的布局从屏幕左侧到屏幕中间（这里是举例，虽然你可以通过屏幕设备信息来其移动的距离，但这样做不具有普适性）。

值得我们做这种昂贵计算的另一个原因是，**当用户与页面进行交互后，会存在一个`100ms`的空窗期**，不会发生任何时，此期间我们可以做任何事情同时不会引起用户的注意。

![Taking advantage of user perception.](./imgs/100ms.jpg)

通过这些空窗期，我们可以操作`getBoundingClientRect()/getComputedStyle()`这两个`API`，同时将动画转化为`transform/opacity`这个两个精致、流畅、对复合图层友好的属性的动画。[为什么要使用这两个属性，因为它们会生成新的图层，进行独立的动画运算](https://aerotwist.com/blog/pixels-are-expensive/)）

> 原文：We can use that window of time to do all that getBoundingClientRect work (or getComputedStyle if that’s your poison) in JavaScript, and from there we make sure that we’re reducing the animation down to nice-and-fast, compositor-friendly, look-ma-no-paints transform and opacity changes. (Why just those? Check out my [Pixels are Expensive](https://aerotwist.com/blog/pixels-are-expensive/) post.)

一个动画应该尽量重构为用`transform/opacity`来表示。（Animations that can be remapped to transform and opacity changes are the perfect fit.）

## FLIP 注意事项

1. **不要超过`100ms`的空窗期**。超过`100ms`空气期意味着用户可能会感知到卡顿，所以你需要在开发时通过`devTool`对其进行把控。
2. **内容可能会失真**。当你制作一个放缩（`scale`）、位移（`transform`）动画时，一些元素可能会失真。

Learn and sum up from [FLIP Your Animations](https://aerotwist.com/blog/flip-your-animations/)
