title: "初识WebGL"
date: 2015-05-20 15:52:24
categories: JavaScript
tags: [WebGL]
description: 
---

###先来看看WebGL的官方介绍：

>WebGL - OpenGL ES 2.0 for the Web

WebGL -- Web上的OpenGL ES 2.0

>WebGL is a cross-platform, royalty-free web standard for a low-level 3D graphics API based on OpenGL ES 2.0, exposed through the HTML5 Canvas element as Document Object Model interfaces. Developers familiar with OpenGL ES 2.0 will recognize WebGL as a Shader-based API using GLSL, with constructs that are semantically similar to those of the underlying OpenGL ES 2.0 API. It stays very close to the OpenGL ES 2.0 specification, with some concessions made for what developers expect out of memory-managed languages such as JavaScript.

WebGL是一种跨平台并且可以自由使用的3D图形API，它基于OpenGL ES 2.0，并且通过HTML5 Canvas来将自己暴露为DOM接口。因为WebGL在语义上同OpenGL ES 2.0的基础接口非常类似，所以熟悉OpenGL ES 2.0的开发者会把WebGL当做使用GLSL（OpenGL Shading Language，OpenGL着色语言）的一套API。它同OpenGL ES 2.0的标准非常类似，只是在某些地方为顺应开发者需求做了妥协。

>WebGL brings plugin-free 3D to the web, implemented right into the browser. Major browser vendors Apple (Safari), Google (Chrome), Mozilla (Firefox), and Opera (Opera) are members of the WebGL Working Group.

WebGL为web带来了无插件化的3D技术，这种实现非常适合浏览器。目前，主要的浏览器生产商Apple (Safari), Google (Chrome), Mozilla (Firefox), and Opera (Opera)都成为了WebGL工作组的成员。

补充：2014年微软也加入了WebGL工作组(IE11已经支持WebGL)。

<!--more-->

##WebGL是什么？

WebGL是一种3D绘图标准，它提供了一套JS版本的API接口，通过调用这些接口可以直接使用OpenGL ES 2.0的功能，从而实现强大的3D绘图效果。

WebGL在HTML5中使用Canvas作为宿主，能够在渲染的过程中使用硬件加速，这让3D模型的展示更加流畅，在特定的业务场景中具有非常明显的优势。

最后需要重点提醒的是，WebGL在浏览器中展示3D效果不需要额外安装任何插件。对于支持WebGL的浏览器来说，可以直接用JavaScript来实现3D效果。

目前对WebGL提供支持的浏览器有[Firefox 4](https://developer.mozilla.org/en-US/docs/Firefox_4_for_developers)+ 、[Google Chrome](http://www.google.com/chrome/) 9+ 、[Opera](http://www.opera.com/) 12+和[Safari](http://www.apple.com.cn/safari/) 5.1+。

##3D图形API

我们平时了解较多的3D图形API主要有OpenGL、DirectX。

OpenGL（Open Graphics Library，开放图形库）使用非常广泛，因为其跨平台性，在各种操作系统下都有很多应用场景，开发游戏、工业建模、嵌入式设备等。

DirectX是微软创建的一系列为多媒体以及游戏开发服务的应用程序接口，主要应用场景是在windows系统下进行游戏开发，基本在这个领域下无任何对手。

OpenGL ES可以看做是OpenGL的一个子集，专注于嵌入式设备。OpenGL ES 2.0是其中一个主要版本，它采用了可编程的渲染管线，渲染能力有了非常大的提高，它要求设备必须有相应的GPU硬件支持，不支持软件模拟实现。


##WebGL、OpenGL与OpenGL ES 2.0

WebGL、OpenGL和OpenGL ES 2.0的相互关系可以参考下图：

![OpenGL相关生态体系](http://7tt058.com1.z0.glb.clouddn.com/OpenGL-related-Ecosystem.jpg)

从图中可以看出，OpenGL ES是OpenGL的子集，WebGL同时在OpenGL和OpenGL ES 2.0上基础上构建出了JavaScript API，依托于HTML5，在浏览器上实现了无插件依赖的3D技术。

WebGL是JavaScript的API接口，对于web开发人员来说更加友好，使用起来更容易上手。

OpenGL相对来说，门槛比较高，涉及的专业知识很多，如着色语言、投影变换、光照、纹理等，需要花费比较多的时间去学习整理。

##OpenGL ES 3.0

Khronos Group在SIGGRAPH 2012专业图形大会上宣布了新一代移动3D图形标准规范“OpenGL ES 3.0”。

WebGL是基于OpenGL ES 2.0的，到目前为止还没有考虑过3.0，虽然3.0提出了很多新的功能。

Android对于OpenGL ES 3.0的相关说明：

>OpenGL ES 3.0 - This API specification is supported by Android 4.3 (API level 18) and higher.

同时，它还声明，设备必须需要设备生产商来实现相关图形管线，也就是说如果设备生产商不实现相关支持，即使是4.3或者更高版本的系统也无法运行OpenGL ES 3.0的API。


##参考

1. [WebGL - OpenGL ES 2.0 for the Web](http://cn.khronos.org/webgl)
2. [WebGL](https://developer.mozilla.org/zh-CN/docs/Web/WebGL)
2. [Web的3D绘图标准 WebGL](http://www.oschina.net/p/webgl)
3. [移动3D图形新起点：OpenGL ES 3.0规范发布](http://news.mydrivers.com/1/236/236922.htm)
4. [OpenGL ES](http://developer.android.com/guide/topics/graphics/opengl.html)
5. [微软为何要在 2014 年加入 WebGL 小组？](http://www.zhihu.com/question/24813635)
6. [WebGL 是否需要以 OpenGL 为学习基础？](http://www.zhihu.com/question/19993499)
