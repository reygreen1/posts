title: "circle progress 环形进度条"
date: 2015-09-08
categories: Canvas
tags: [canvas, 进度条]
---

![circle progress](http://7tt058.com1.z0.glb.clouddn.com/progress.png)

最近开发中有这样一个需求，需要设计出环形进度条的效果，而且要兼容低版本浏览器。

我们知道，传统方式下，除非使用图片，不然没办法做出这种圆环的动态效果。但是进度数据的范围相对而言比较广（整数值状态下为 0%~100% ，约 100 个独立状态），每个状态用图片来做显然是不现实的。

既然传统方式不行，那就必须考虑其他方式来解决问题。CSS3、Canvas 等都是可以参考的解决方案。使用这些技术有利有弊，依赖其他一些辅助库或框架，可以让解决方案变得更加完美。

我们分析下使用这些技术是如何解决环形进度条需求的。

<!--more-->

## 传统方式

传统方式一般采用背景图片来解决上面的需求。

### 原理

具体做法是，将各个状态下的图片拼成一个 sprite ，定时更新 background-position 来显示各个状态下的图片，时间间隔掌握合适的话就形成了动画。这是最简单粗暴的做法。

### 优势

- 基本不用编写复杂的绘图代码，只要写好定时器部分的逻辑即可完成交互。
- 每个状态都可以用图来表示，展现复杂的进度效果很简单，只需要修改 PSD 即可。
- 整体来说，对开发人员的要求较低。

### 缺点

- 如前文所述，对状态点的数目很敏感。正常情况下的 100（0%~100%） 个状态需要相同数据的图片。当然也可以减少状态点的数目，但是这么做有可能会影响到后期状态切换时的渐进效果。
- 后期需要修改状态点展现时，需要重新制作 sprite 。使用工具可以解决这个问题。

## CSS3

CSS3 本身提供很多强大的属性，可以通过使用 border-radius、transform 等属性来完成上面的需求。

### 原理

先做一个环形的进度条，然后在它上面分别放置左右两个半圆（使用 clip 来显示一个圆的一半区域），最后定时旋转（使用 transform 中的 rotate 来实现）这两个半圆来显示出下面的进度条。连续执行后就形成了渐进的动画。

这里主要使用的 CSS 属性的相关用法可以参考下面的链接：

- [border-radius](http://www.w3cplus.com/css3/border-radius)
- [clip](http://www.w3cplus.com/css3/clip.html)
- [transform](http://www.w3cplus.com/content/css3-transform)

### 实例

<iframe height='268' scrolling='no' src='//codepen.io/reygreen1/embed/pjJJqe/?height=268&amp;theme-id=18663&amp;default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='http://codepen.io/reygreen1/pen/pjJJqe/'>circleProgress</a> by reygreen1 (<a href='http://codepen.io/reygreen1'>@reygreen1</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

### 优势

- 由于使用 CSS 属性设置进度条状态，修改状态就比较方便，只需要修改属性值即可。
- 已经有很多 CSS 效果的工具或者站点，类似效果也有很多 demo，可以直接拿来修改使用。

### 缺点

- 低版本浏览器的支持问题。使用的一些属性在低版本浏览器下不兼容，无法满足上文的第二条需求。有相关库或者脚本来解决 IE8 及更低版本浏览器对 CSS3 的支持问题（ [ie-css3.htc](http://www.zhangxinxu.com/wordpress/2010/04/%E8%AE%A9ie6ie7ie8%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81css3%E5%B1%9E%E6%80%A7/)、[CSS3 PIE](http://css3pie.com/)等）。
- 使用 CSS3 可以制作很酷炫的效果，但对开发人员的要求也较高。在效果制作中有比较复杂的编码过程，即使是使用已有的代码，也需要花费一定成本来了解实现原理。

## Canvas

Canvas 的实现就是使用 canvas 相关 API 在画板上画圆，定时刷新画板即可实现动态效果。

### 原理

使用 canvas 相关接口（ arc、fillStyle、fill等）在画板上画图，使用定时器定时刷新画板上的显示，达到渐进动画的效果。

canvas 的相关使用可以参考这个链接：[Canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)。

### 实例

<iframe height='268' scrolling='no' src='//codepen.io/reygreen1/embed/bVdpNy/?height=268&amp;theme-id=18663&amp;default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='http://codepen.io/reygreen1/pen/bVdpNy/'>drawCircleProgress</a> by reygreen1 (<a href='http://codepen.io/reygreen1'>@reygreen1</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

### 优势

- canvas 制作的动画效果非常丰富，API 的使用也非常方便。熟悉的开发人员可以很快开发出强大的效果。
- 更多的网站和 DEMO 供你参考选择。

### 缺点

- 低版本浏览器兼容性问题。不支持的浏览器无法看到任何 canvas 上展现的效果。可以使用 [excanvas.js](https://github.com/arv/ExplorerCanvas) 来解决兼容性问题。
- 要求开发人员对 canvas 开发有一定了解。

## 总结

上面主要分析了三种圆形进度条的实现方式，各有优劣。读者可以根据自己的情况选择合适的解决方案。

笔者选择的是 canvas 的方式，使用 excanvas.js（压缩后约 7.5K） 来解决低版本浏览器的兼容性问题。

excanvas.js 相关介绍：
> DESCRIPTION

> Firefox, Safari and Opera 9 support the canvas tag to allow 2D command-based 
drawing operations. ExplorerCanvas brings the same functionality to Internet 
Explorer; web developers only need to include a single script tag in their 
existing canvas webpages to enable this support.

但在使用过程中还是遇到了一些坑，如：

1. excanvas.js 会对 canvas 做处理，*处理后 canvas 元素中的文本内容会被清空*。所以，如果有相关需要后续处理的数据不能放到 canvas 里，而要作为它的一个属性值来保存。具体可以参考上文实例中的 *process* 属性的设置。
2. 使用 canvas 中的 arc 方法，如果是顺时针方向，在起始位置一致时，IE 不会进行渲染。excanvas.js 源码是这么写的：

```javascript
// IE won't render arches drawn counter clockwise if xStart == xEnd.
if (xStart == xEnd && !aClockwise) {
      xStart += 0.125; // Offset xStart by 1/80 of a pixel. Use something
                       // that can be represented in binary
}
```

即使它做了修正，在使用过程中还是可能出现问题，如果有类似场景需要特别注意。可以参考上文实例中关于进度为 100% 时做的修正代码。

总体来说，这个脚本还是非常给力的，而且它已经出现在了一些产品的生产环境中，所以读者可以放心使用。

## 参考

1. [利用jQuery和CSS实现环形进度条](http://www.w3cplus.com/css3/create-radial-progress-bar-with-jQuery-and-css3.html)
2. [canvas绘制旋转的圆环百分比进度条](http://caibaojian.com/canvas-circular.html)
3. [让IE6、IE7、IE8支持CSS3的脚本](http://www.jb51.net/css/28584.html)