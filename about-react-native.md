title: "来，我们谈谈React Native"
date: 2015-04-12
categories: 移动端开发
tags: [JavaScript, React, 移动端]
---

![React Native](http://7tt058.com1.z0.glb.clouddn.com/reactnative.png)

Facebook在今年的React.js Conf 2015上推出了React Native，而这已经成为目前最热门的讨论话题。

我们知道，传统的原生开发是需要一定门槛的，要转行为原生开发其实是比较困难的。这就导致了市场需求大，而相关开发人员不能满足的问题。相比较而言，web开发的入门就比较容易，相关的开发者也就比较多。如果能让一部分web开发者直接开发原生应用，这将是多么美好的一件事情啊，而这也是不少框架出现的直接原因。

如果你本身是就是各个平台原生应用的开发者，那么React Native貌似不会给你带来更大的兴趣。但是，作为使用javascript的web开发人员，它的出现无疑是影响巨大的。React Native让web开发人员通过习惯的开发方式(使用React)来开发原生应用成为了可能，更重要的是它提出了一种新的开发理念，而这也是它目前火爆的主要原因。

既然是React Native，那么它必然与Native有着或多或少的关系。没错，React是React Native的强有力支撑，在React Native的实现中占有非常重要的作用，但是这基本属于另一个话题，本文不会着重介绍React、Flux等相关概念。

<!--more-->

## What’s React Native?

> What we really want is the user experience of the native mobile platforms, combined with the developer experience we have when building with React on the web.

从上面的这段介绍，我们可以看出，React Native的目的就是使开发人员可以通过React来开发原生应用。这说明使用React Native也是需要有一定学习成本的，不是直接使用传统的javascript就可以开发的，你需要先学会使用React。

> Learn once, write anywhere.

React Native并不是像Java那样可以“write once, run anywhere.”。这是因为我们必须考虑不同原生平台上用户体验的差异性，过分要求应用在不同平台上的一致性其实是不合适的。所以，React Native的理念是，可以针对不同平台来写React代码，从而开发出合适的原生应用。

简而言之，React Native是这样的一个工具，它使用React作为用户界面构建的工具，并在此基础上重新包装了一个渲染引擎(不同平台可能不一致)，可以将Virtual DOM生成不同平台下的UI，并创建了一系列的bridge来让js同native code进行通信。这样，就可以通过编写React代码来生成Native应用了。

### 绝对不是Hybrid app

其实，使用hybrid的方式来开发app的框架很早就出现了，但是React Native绝对不是这样的，它生成的app里面没有webview，使用的全是原生的UI。

## React Native的运行机制

Facebook在React Native中使用了自己的开源工具[JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)、[css-layout](https://github.com/facebook/css-layout)。

eact Native运行机制类似于一个浏览器引擎的渲染过程，它根据以下步骤来运行(以iOS平台为例)：

1. 加载。native代码加载相关React文件，使用javascriptCore.framework构建OCbridge，这里将核心组件的方法注入到了js里，以供native和js进行通信。
2. 解析。解析的过程主要是把对应的解析结果映射到native上，类似于浏览器中DOM结构的解析过程。
3. 渲染。使用css-layout解析相关的css规则，从而确定各个结点上的属性值。
4. 绘制。最后将Native UI在视图上绘制出来。

至于React Native内部的通信机制，已经有两篇文章介绍的比较详细，可以参考：

1. [React Native通信机制详解](http://blog.cnbang.net/tech/2698/)
2. [React Native 初探（iOS）](http://www.hotobear.com/?p=1015)

## React Native的优势

### 同webapp相比，native控件拥有更好的用户体验

在webapp中，毫无疑问，不管是如何模拟出来的原生效果，都没有native本身的效果更加细腻合适。在流畅性方面，native也是具有得天独厚的优势。其他还有手势识别、线程模型等，这些都是webapp所无法超越的。而对于Hybrid app，native应用可以完爆其交互和性能问题。

### 同native相比，React符合web开发的习惯

虽然React的使用也需要一些前期的成本，但是同学习native开发相比，对web开发者来说还是方便不少。同时，React Native中的js是可以运行在桌面chrome中的，通过websocket连接Native代码和chrome可以让调试变得非常方便。在React Native提供的示例中，我们可以很方便的通过CMD+R直接看到代码更新后的效果。

## 目前还存在的问题

1. 平台一致的问题。目前React Native能支持Web和iOS，对于Android的支持需要稍后发布，不能保证各个平台同步改造。
2. 使用成本的问题。需要学习React相关知识，需要进行概念转换。
3. 依赖原生组件暴露出来的组件和方法。只能在已暴露的接口上做业务开发，限制了开发能力。同时对原生接口依赖性极强，不同平台要进行不同的开发。
4. 其他的问题还有iOS6中JavaScriptCore、javascriptCore.framework为私有，使用上可能会受apple政策影响。

总之，虽然现状并不完美，但是React Native已经让我们看到了无限的希望，它在未来必定会产生巨大的影响。

## 参考

1. [Introducing React Native](http://facebook.github.io/react/blog/2015/03/26/introducing-react-native.html)
2. [React](http://facebook.github.io/react/index.html)
3. [React Native](http://facebook.github.io/react-native/)
4. [如何评价 React Native？](http://www.zhihu.com/question/27852694)
5. [谈谈 React Native](http://blog.devtang.com/blog/2015/02/01/talk-about-react-native/)
6. [React Native通信机制详解](http://blog.cnbang.net/tech/2698/)
7. [React Native 初探（iOS）](http://www.hotobear.com/?p=1015)
8. [深入浅出 React Native：使用 JavaScript 构建原生应用](http://www.cocoachina.com/ios/20150408/11513.html)