title: "WebGL编程起步"
date: 2015-05-25 11:05:16
categories: WebGL
tags: [WebGL, three.js]
description: 
---

![WebGL编程](http://7tt058.com1.z0.glb.clouddn.com/webgl.jpg)

上篇文章简单介绍了WebGL的一些相关概念、现状以及工作原理，没有涉及到具体编码，本文将使用WebGL的相关技术来实现一个简单的效果示例，从而让大家了解WebGL编程的基本流程和关键操作。最后，还介绍了一些WebGL的库和框架，恰当的使用这些第三方框架，可以大大提高我们的工作效率。

<!--more-->

### 开发流程

WebGL的开发过程主要包括以下几个步骤：

#### 1.准备容器

WebGL要在页面上执行相关的渲染，必须依赖一个特定的容器，在HTML5中，使用的是Canvas作为容器。

```html
<body onload="start()">
  <canvas id="glcanvas" width="640" height="480">
    Your browser doesn't appear to support the HTML5 canvas element.
  </canvas>
</body>
```

如上所示，我们在HTML中添加了一个canvas元素，并且设置了onload事件作为入口，后续的所有业务逻辑都会在start函数中进行处理。

```javascript
//WebGL全局变量
var gl；
function start() {
    var canvas = document.getElementById("glcanvas");  
    //初始化上下文
    gl = initGL(canvas);        
    //初始化着色器
    initShaders();  
    //生成/加载模型数据
    initBuffers();

    //设置擦除颜色为黑色，透明度不透明
    gl.clearColor(0.0, 0.0, 0.0, 1.0);  
    //开启“深度测试”，z-缓存      
    gl.enable(gl.DEPTH_TEST); 
        
    //渲染
    drawScene(); 
}
```

从上面的代码可以看出，我们整个的过程主要包含了四个比较重要的阶段：<i>初始化上下文</i>、<i>初始化着色器</i>、<i>加载模型数据</i>和<i>渲染</i>。下面会对这几个过程做详细介绍。

#### 2.初始化上下文

initGL方法主要用来初始化WebGL的绘制环境：

```javascript
function initGL(canvas) {  
  	gl=null;      
  	try {
    		//尝试创建标准上下文，如果失败，回退到试验性上下文
    		gl = canvas.getContext("webgl") || canvas.getContext("experimental-webgl");
  	}
  	catch(e) {}

  	//如果没有GL上下文，马上放弃
  	if (!gl) {
    		alert("Unable to initialize WebGL. Your browser may not support it.");
  	}
} 
```

需要注意的是，我们为了得到WebGL上下文，需要使用canvas的getContext方法，这里有两个参数值可以使用：<b>webgl</b>或者<b>experimental-webgl</b>。不同浏览器环境下使用的参数不一样，具体可参照[WebGL - 3D Canvas graphics](http://caniuse.com/#search=webgl)。

#### 3.初始化着色器

在3D场景的开发过程中会涉及到非常多的计算过程（颜色、位置等），为了提高效率，使用了硬件加速，这个时候GPU的强大计算能力得到了运用。但是，怎样告诉它正确执行相关计算逻辑呢？答案是着色器。着色器告诉相关计算单元做什么，而着色器语言（GLSL）来告诉它们怎么做。

[The OpenGL ES Shading Language](https://www.khronos.org/registry/gles/specs/2.0/GLSL_ES_Specification_1.0.17.pdf)

一般情况下，我们在HTML中定义着色器，然后在代码中获取并使用。

```html
<script id="shader-fs" type="x-shader/x-fragment">
    precision mediump float;
    varying vec4 vColor;

    void main(void) {
        gl_FragColor = vColor;
    }
</script>
<script id="shader-vs" type="x-shader/x-vertex">
    attribute vec3 aVertexPosition;
    attribute vec4 aVertexColor;

    uniform mat4 uMVMatrix;
    uniform mat4 uPMatrix;

    varying vec4 vColor;

    void main(void) {
        gl_Position = uPMatrix * uMVMatrix * vec4(aVertexPosition, 1.0);
        vColor = aVertexColor;
    }
</script>
```

上文的代码中定义了两个着色器：<b>片段着色器</b>和<b>顶点着色器</b>。

顶点着色器定义了模型中顶点的位置和颜色信息。

片段着色器通过差值的方法来处理WebGL中像素的相关数据，这里定义了像素上颜色的计算方法。

我们在initShaders方法中来查找使用着色器：

```javascript
var shaderProgram;

    function initShaders() {
        var fragmentShader = getShader(gl, "shader-fs");
        var vertexShader = getShader(gl, "shader-vs");
        //创建着色器
        shaderProgram = gl.createProgram();
        gl.attachShader(shaderProgram, vertexShader);
        gl.attachShader(shaderProgram, fragmentShader);
        //链接着色器程序
        gl.linkProgram(shaderProgram);
        //检查着色器是否成功连接
        if (!gl.getProgramParameter(shaderProgram, gl.LINK_STATUS)) {
            alert("Could not initialise shaders");
        }
        //连接成功后激活渲染器程序
        gl.useProgram(shaderProgram);

        shaderProgram.vertexPositionAttribute = gl.getAttribLocation(shaderProgram, "aVertexPosition");
        //启用顶点缓冲区数组
        gl.enableVertexAttribArray(shaderProgram.vertexPositionAttribute);

        shaderProgram.vertexColorAttribute = gl.getAttribLocation(shaderProgram, "aVertexColor");
        gl.enableVertexAttribArray(shaderProgram.vertexColorAttribute);

        shaderProgram.pMatrixUniform = gl.getUniformLocation(shaderProgram, "uPMatrix");
        shaderProgram.mvMatrixUniform = gl.getUniformLocation(shaderProgram, "uMVMatrix");
    }
```

上述代码加载了模型数据，并且通过一系列的绑定操作和相关设置，告诉GPU如何去处理相关的渲染过程。

这里使用了一个工具函数getShader，它的作用是从DOM中获取定义的相关着色器程序，并返回编译好的渲染器程序：

```javascript
function getShader(gl, id) {
    var shaderScript, theSource, currentChild, shader;
    
    shaderScript = document.getElementById(id);    
    if (!shaderScript) {
        return null;
    }
    
    //获取着色器的文本内容保存到theSource
    theSource = "";
    currentChild = shaderScript.firstChild;    
    while(currentChild) {
        if (currentChild.nodeType == currentChild.TEXT_NODE) {
            theSource += currentChild.textContent;
        }
        
        currentChild = currentChild.nextSibling;
    }
    //创建顶点着色器或片段着色器
    if (shaderScript.type == "x-shader/x-fragment") {
      shader = gl.createShader(gl.FRAGMENT_SHADER);
    } else if (shaderScript.type == "x-shader/x-vertex") {
      shader = gl.createShader(gl.VERTEX_SHADER);
    } else {
     //非法类型返回null
     return null;
    }
   gl.shaderSource(shader, theSource);
    
   //编译着色器代码
   gl.compileShader(shader);  
    
   //检查是否编译成功
   if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {  
      alert("An error occurred compiling the shaders: " + gl.getShaderInfoLog(shader));  
      return null;  
   }
   //成功后返回编译好的着色器
   return shader;
} 
```

#### 4.生成/加载模型数据

下面的initBuffers函数来将模型数据加载到缓冲器中，这样将顶点位置和颜色数据在上下文中准备就绪，随后才可以进行下一步的渲染操作。

```javascript
var triangleVertexPositionBuffer;
    var triangleVertexColorBuffer;

    function initBuffers() {
        triangleVertexPositionBuffer = gl.createBuffer();
        gl.bindBuffer(gl.ARRAY_BUFFER, triangleVertexPositionBuffer);
        var vertices = [
             0.0,  1.0,  0.0,
            -1.0, -1.0,  0.0,
             1.0, -1.0,  0.0
        ];
        gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(vertices), gl.STATIC_DRAW);
        triangleVertexPositionBuffer.itemSize = 3;
        triangleVertexPositionBuffer.numItems = 3;

        triangleVertexColorBuffer = gl.createBuffer();
        gl.bindBuffer(gl.ARRAY_BUFFER, triangleVertexColorBuffer);
        var colors = [
            1.0, 0.0, 0.0, 1.0,
            0.0, 1.0, 0.0, 1.0,
            0.0, 0.0, 1.0, 1.0
        ];
        gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(colors), gl.STATIC_DRAW);
        triangleVertexColorBuffer.itemSize = 4;
        triangleVertexColorBuffer.numItems = 3;
    }
```

#### 5.渲染

经过上一步的模型数据准备后，就可以直接通过WegGL来进行渲染了。下面的drawScene方法，对3D模型进行了一些基本设置，然后根据缓冲区中的相关数据，来绘制出相应的效果。

```javascript
function drawScene() {
        gl.viewport(0, 0, gl.viewportWidth, gl.viewportHeight);
        gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);

        pMatrix = okMat4Proj(45.0, gl.viewportWidth / gl.viewportHeight, 0.1, 100.0);
        mvMatrix = okMat4Trans(-1.5, 0.0, -7.0);

        gl.bindBuffer(gl.ARRAY_BUFFER, triangleVertexPositionBuffer);
        gl.vertexAttribPointer(shaderProgram.vertexPositionAttribute, triangleVertexPositionBuffer.itemSize,
  gl.FLOAT, false, 0, 0);

        gl.bindBuffer(gl.ARRAY_BUFFER, triangleVertexColorBuffer);
        gl.vertexAttribPointer(shaderProgram.vertexColorAttribute, triangleVertexColorBuffer.itemSize, 
gl.FLOAT, false, 0, 0);

        setMatrixUniforms();
        gl.drawArrays(gl.TRIANGLES, 0, triangleVertexPositionBuffer.numItems);
    } 
```

至此，一个完整的WebGL程序基本开发完毕了，通过上面的过程我们发现，使用原生的WebGL来进行特殊场景的开发简直就是一种折磨。我们需要了解各种实现细节和专业知识，这对于非专业的开发人员是一种极大的挑战。所以，自然而然就有大量的类库和框架出现，来简化我们的开发过程，提高我们的工作效率。

[点击这里可以看到用three.js来实现的上文效果](https://jsfiddle.net/p924bve3/)

### WebGL库和框架

下面简单介绍一些WebGL的类库或者框架：

[three.js](http://threejs.org/)，这是一个低复杂、轻量级的开源框架，也是目前应用最广泛的3D框架，模型文件支持多种格式，渲染器的选择也非常灵活，方便使用者快速开发各种效果。

[PhiloGL](http://www.senchalabs.org/philogl/)，这也是一个开源框架，注重性能，拥有非常强大的接口，比较适合数据可视化和游戏开发。

[Babylon.js](http://www.babylonjs.com/)，这是一款非常适合作为游戏引擎的开源框架，让WebGL的使用更加简单、强大，非常适合游戏开发。

[SceneJS](http://scenejs.org/)，这一开源框架的特点在于，对于高精度的细节控制非常好，适合的领域有工程学、医学建模等。

[CopperLicht](http://www.ambiera.com/copperlicht/)，这是一个非常出色的3D引擎，并且带有编辑器，唯一的缺点是，他不是开源的，如果商用需要购买。

### 参考

1. [突袭HTML5之WebGL 3D概述](http://www.uml.org.cn/html/201208151.asp)
2. [Adding 2D content to a WebGL context](https://developer.mozilla.org/en-US/docs/Web/WebGL/Adding_2D_content_to_a_WebGL_context)
3. [The OpenGL® ES Shading Language](https://www.khronos.org/registry/gles/specs/2.0/GLSL_ES_Specification_1.0.17.pdf)
4. [WebGL 入门 – WebGL框架](http://www.csdn.net/article/a/2013-12-18/15817486)
