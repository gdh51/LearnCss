# webp格式图片

`webp`是由谷歌推出的新一代图片格式，在压缩率上比`jpeg`格式更优越，同时提供了有损压缩与无损压缩的图片文件格式，在质量相同的情况下，`webp`格式图像的体积要比`jpeg`格式图像小`40%`；无损压缩后比`png`文件少`45%`的文件大小，即使这些文件经过其他压缩工具压缩之后，`webp`也可以再压缩`28%`文件大小。

[具体的各种格式图片压缩效果展示](https://isparta.github.io/compare-webp/index.html#234511)

## 优点

`webp`格式图片的优势可以总结为以下三点：

- `png`图片转`webp`的压缩率要高于`png`原图的压缩率，同样支持无损和有损
- 转化后的`webp`图片体积大幅度减少同时图片质量几乎保存不变
- 转化后的`webp`图片支持`alpha`透明和`24-bit`颜色数，不存在`png8`色彩不够丰富和在浏览器中可能出现毛边的问题。

## 缺点

说完优点，那么肯定也要说明其缺点，这些缺点主要集中在：

- 兼容性差，浏览器支持率不高
- 编解码速度慢(这个不太需要关心)

那么在兼容性方面它支持`PC`端`chorme 10+`（14 ~ 16 有渲染 bug）、`opera 11+` 、`safri`均支持`webp`格式图片，`Firefox`也曾拒绝支持`webp`，如果你打算在 App 中使用 `webp`，除了`Android4.0`以上提供的**原生**支持外(顾名思义，安卓基本上都能用)，其他版本以及`iOS`都可以直接使用官方提供的解析库。

## 兼容性解决方案

那么针对以上(兼容性)的缺点，具体的解决方案一般分为三种：

1. 服务器判断图片返回格式。
当服务器收到浏览器的图片请求时，通过`User-Agent`字段来判断是否应该返回``webp``格式的图片。这种方式优势很明显，前端几乎不用做任何处理(此时要讨好后端`:)`)，但是前端此时要注意的是使用图片`url`时不能为其添加图片的后缀。
2. 浏览器判断是否支持`webp`格式图片
相对于服务器端来做图片的兼容，浏览器端做图片的兼容就比较麻烦，需要分别知道两种格式图片的`url`，且内嵌在`css/html`中的图片无法享受兼容处理，仅动态`url`能享受(当然你可以强行写个脚本来处理全部的`url`，但是就麻烦了)。
3. 替浏览器写解析程序
这个方法就是替浏览器来解析图片。对于不支持`webp`格式的浏览器，通过`JS`代码将其转化为浏览器支持的格式，再以`base64`的形式赋值给元素的`url`。这个方法能够处理各种形式的图片，缺点是会对性能产生一定的影响。

综上呢，我们可以看到最好的方法还是第一种，也是目前各大网站的做法啦。

### 浏览器端判断浏览器是否兼容

在浏览器端我们可以通过简单的加载webp格式图片来判断是否兼容：

```js
(function () {    var WebP = new Image();
    WebP.onload = WebP.onerror = function () {        if (WebP.height != 2) {            // 不支持WebP
        } else {            // 支持WebP
        }
    };
    WebP.src = 'data:image/webp;base64,UklGRjoAAABXRUJQVlA4IC4AAACyAgCdASoCAAIALmk0mk0iIiIiIgBoSygABc6WWgAA/veff/0PP8bA//LwYAAA';
})();

// 或则使用canvas来尝试导出webp格式的图片，看是否能正常导出
function checkWebp() {    try {        return (document.createElement('canvas').toDataURL('image/webp').indexOf('data:image/webp') == 0);
    } catch (err) {        return false;
    }
}
```

[什么是 WebP 图片](https://www.leiue.com/1703)
[前端性能优化之WebP](https://www.cnblogs.com/163yun/p/9834604.html)
