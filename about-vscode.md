title: "初玩Visual Studio Code"
date: 2015-04-30 16:52:15
categories: 工具
tags: [工具,Visual Studio Code]

---

![Visual Studio Code](http://7tt058.com1.z0.glb.clouddn.com/vscode.png)

微软在今年的Build大会上发布了一款新型的代码编辑器Visual Studio Code，它一出现就吸引了大多数开发者的关注，尤其是前端开发人员。

>Build and debug modern web and cloud applications. Code is free and available on your favorite platform - Linux, Mac OSX, and Windows.

如官网介绍，Code的特点是免费、跨平台，它专注于代码开发过程，提供调试工具，定位轻量级的代码编辑器，将竞争的矛头直接指向[Sublime Text](http://www.sublimetext.com/)、[Atom](https://atom.io/)。

笔者今天使用了一天Code的预览版，下面来简单介绍下这款工具的一些优缺点。

<!--more-->

### 定位

首先必须明确说明，Code仅仅是一个轻量级的代码编辑器，跟我们平时使用的Sublime、Atom类似，绝不是完整的IDE（集成开发环境），区别于Visual Studio等。

### 界面

不得不说这真是一款颜值颇高的编辑器，启动后的给人一种非常舒服的感觉。

![Code界面](http://7tt058.com1.z0.glb.clouddn.com/vscode-layout.png)

左侧除了有在Sublime中掺假的目录结构，还多了一个侧边栏的导航，功能包含了搜索、git和debug的功能。在不同的导航界面下，目录面板会有不同的功能设置。

还有一个需要注意的是，最右侧只剩下了一个导航条来显示当前视图在整个文件中的位置，而Sublime中使用的是文件内容缩略图。在文件内容较多的时候，滚动条无法提供一种“即视感”，所以我还是比较偏爱Sublime的这种设置。

### 文件和文件夹操作

首先，Code里打开一个项目就会新建一个视图窗口，而Sublime里可以同时在FOLDERS下同时打开多个项目，区别还是不小。不过貌似，这对专注单一项目的同学还好，要是对像我这种需要在不同项目里跳来跳去的人来说，还是Sublime的更加合适。

当鼠标放在项目根目录上时，会出现四个操作按钮，如下图所示：

![](http://7tt058.com1.z0.glb.clouddn.com/file-opertion.png)

可以新建文件、文件夹和执行目录折叠的操作等，同时，右键新建操作更加接近真实的创建场景，如果你使用过Sublime的相关新建操作，就会发现Code的方式还是更容易接受。

另外，当前编辑窗口的右上角多了一个Split Editor的按钮，用来将当前窗口快速分割为左右两个，类似“分身”。

 ![窗口分身](http://7tt058.com1.z0.glb.clouddn.com/file-splite.png)
 
当然，Sublime也可以完成相同的功能，而且还可以上下分割，但是需要快捷键操作或者选择菜单，Code这里做了按钮，简单直接。

另一个槽点，既然支持多行文件编辑，但是居然不对多行选择进行支持！在Sublime里很方便的一条命令，居然在Code里玩不起来了，好伤心。

### 智能提示

Sublime的智能提示是针对当前文件上下文的，试试Code的提示，简直要逆天了。

![智能提示](http://7tt058.com1.z0.glb.clouddn.com/code-suggest.png)

HTML、JS、Angular的补全，个人感觉非常强大。如果要实现上图node相关的提示，则需要在文件开头添加：

>/// <reference path="typings/node/node.d.ts"

### 代码高亮

目前Code的代码高亮功能完全不能跟Sublime相比，总是一种很单调的感觉。

但是，这个问题解决起来应该不会很难，只要Code在后续版本中支持插件扩展，万能的开发者肯定马上就可以让Code的Theme（目前只有dark和light两种风格）丰富起来。

### 代码调试

debug的选项让人眼前一亮，最近又正好在写node模块，如果能在Code里debug代码真是非常方便。

但是，要想使用调试功能，必须先安装[Mono](http://www.mono-project.com/download/)，不然连node的调试都进行不了。更悲催的是，VSCode的安装包有65M左右，但是Mono的安装文件居然要200多M，安装完后占据600多M空间。

![需要先安装Mono](http://7tt058.com1.z0.glb.clouddn.com/require-mono.png)

完成安装后，配置好加载项的相关配置，调试界面还是非常让人惊艳的：

![Code调试node代码](http://7tt058.com1.z0.glb.clouddn.com/code-debug.png)

### 性能

据说在打开大容量文件的速度上要比Sublime快不少，但我没有实际测试过。官网也有特别宣传编辑器的性能，所以有兴趣的同学可以私下测试对比下。

### 插件支持

目前预览版不支持插件化，但我认为这只是一时状况而已，微软不可能会忽略Sublime的成功之道。相信在正式版中会有比较完美的支持。

---

### 总结

从上面的对比分析可以看出，Code本身有自己的亮点，但是目前并不完善（当然这只是预览版而已）。相信Code的开发者也一定注意到了上文提到的几个问题，期待在正式版中有所改进。但就目前的情况而言，Code无法撼动Sublime的地位，所以日常开发中，我还是会有选择的使用Code和Sublime。

---

### 参考

1. [Visual Studio Code官网](https://code.visualstudio.com/)
2. [如何评价 Visual Studio Code？](http://www.zhihu.com/question/29984607)
3. [Code相关下载](https://code.visualstudio.com/Download)
4. [Mono下载](http://www.mono-project.com/download/)




