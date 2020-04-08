@[TOC]目录

* [基本用法](#基本用法)
    * [canvas用法](#canvas用法)
    * [渲染上下文](#渲染上下文)
    * [检查支持项](#检查支持项)

## 基本用法
#### canvas用法
```
    <canvas id="canvas" width="150" height="150"></canvas>
```
<canvas> 标签只有两个属性—— width和height。这些都是可选的，并且同样利用 DOM properties 来设置。当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。该元素可以使用CSS来定义大小，但在绘制时图像会伸缩以适应它的框架尺寸：如果CSS的尺寸与初始画布的比例不一致，它会出现扭曲。  
>__注意__: 如果你绘制出来的图像是扭曲的, 尝试用width和height属性为<canvas>明确规定宽高，而不是使用CSS。  
#### 渲染上下文
<canvas> 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容。我们将会将注意力放在2D渲染上下文中。其他种类的上下文也许提供了不同种类的渲染方式；比如， WebGL 使用了基于OpenGL ES的3D上下文 ("experimental-webgl") 。  
canvas起初是空白的。为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。<canvas> 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。对于2D图像而言，如本教程，你可以使用 CanvasRenderingContext2D。  
```
    var canvas = document.getElementById("canvas");
    var ctx = canvas.getContext("2d");
```
代码的第一行通过使用 document.getElementById() 方法来为 <canvas> 元素得到DOM对象。一旦有了元素对象，你可以通过使用它的getContext() 方法来访问绘画上下文。  
#### 检查支持项
替换内容是用于在不支持 <canvas> 标签的浏览器中展示的。通过简单的测试getContext()方法的存在，脚本可以检查编程支持性。上面的代码片段现在变成了这个样子：  
```
    var canvas = document.getElementById("canvas");
    if (canvas.getContext) {
        var ctx = canvas.getContext("2d");
    } else {
        \\
    }
```

