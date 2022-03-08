@[TOC]目录

+ [基本用法](#基本用法)  
    + [canvas用法](#canvas用法)  
    + [渲染上下文](#渲染上下文)  
    + [检查支持项](#检查支持项)  
+ [绘制图形](#绘制图形)  
    + [绘制矩形方法](#绘制矩形方法)  
    + [绘制路径和线](#绘制路径和线)  
    + [圆弧](#圆弧)  
    + [二次贝塞尔曲线及三次贝塞尔曲线](#二次贝塞尔曲线及三次贝塞尔曲线)  
    + [Path2D对象](#Path2D对象)  
+ [文本样式](#文本样式)  
+ [阴影(Shadows)](#阴影(Shadows))  
+ [填充规则](#填充规则)  
+ [线型](#线型)  
+ [透明度(Transparency)](#透明度(Transparency))  
+ [裁剪(clip)](#裁剪(clip))  
+ [变形](#变形)  
+ [组合(Compositing)](#组合(Compositing))  
+ [图案样式(CanvasPattern)](#图案样式(CanvasPattern))  
+ [使用图片](#使用图片)  
+ [读取像素(pixel)](#读取像素(pixel))  
+ [写入像素](#写入像素)  
+ [反锯齿](#反锯齿)  
+ [渐变(CanvasGradient)](#渐变(CanvasGradient))  
+ [Path2D](#Path2D)  
+ [保存图片](#保存图片)  
## 基本用法
#### canvas用法
```
    <canvas id="canvas" width="150" height="150"></canvas>
```
_canvas_ 标签只有两个属性—— width和height。这些都是可选的，并且同样利用 DOM properties 来设置。当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。该元素可以使用CSS来定义大小，但在绘制时图像会伸缩以适应它的框架尺寸：如果CSS的尺寸与初始画布的比例不一致，它会出现扭曲。  
>__注意__: 如果你绘制出来的图像是扭曲的, 尝试用width和height属性为_canvas_明确规定宽高，而不是使用CSS。  
#### 渲染上下文
_canvas_ 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容。我们将会将注意力放在2D渲染上下文中。其他种类的上下文也许提供了不同种类的渲染方式；比如， WebGL 使用了基于OpenGL ES的3D上下文 ("experimental-webgl") 。  
canvas起初是空白的。为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。_canvas_ 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。对于2D图像而言，如本教程，你可以使用 CanvasRenderingContext2D。  
```
    var canvas = document.getElementById("canvas");
    var ctx = canvas.getContext("2d");
```
代码的第一行通过使用 document.getElementById() 方法来为 _canvas_ 元素得到DOM对象。一旦有了元素对象，你可以通过使用它的getContext() 方法来访问绘画上下文。  
#### 检查支持项
替换内容是用于在不支持 _canvas_ 标签的浏览器中展示的。通过简单的测试getContext()方法的存在，脚本可以检查编程支持性。上面的代码片段现在变成了这个样子：  
```
    var canvas = document.getElementById("canvas");
    if (canvas.getContext) {
        var ctx = canvas.getContext("2d");
    } else {
        \\
    }
```
- - -

## 绘制图形
如何在canvas上绘制三角形、直线、圆弧和曲线。须知左上角为原点坐标，所有元素的位置都相对于原点定位。  
#### 绘制矩形方法
```
    rect(x, y, width, height) // 绘制一个矩形
    fillRect(x, y, width, height) // 绘制一个填充矩形
    strokeRect(x, y, width, height) // 绘制一个矩形的边框
    clearRect(x, y, width, height) // 清楚指定矩形区域，让清除部分完全透明
```
矩形例子
```
    function draw() {
        var canvas = document.getElementById("canvas");
        if (canvas.getContext) {
            var ctx = canvas.getContext("2d");
            // 绘制一个起点在(25, 25)位置宽100，高100的矩形
            ctx.fillRect(25, 25, 100, 100);
            // 清除一个起点在(45, 45)位置宽60，高60的矩形
            ctx.clearRect(45, 45, 60, 60);
            // 绘制一个起点在(50, 50)位置宽50，高50的矩形边框
            ctx.strokeRect(50, 50, 50, 50);
        }
    }
```
#### 绘制路径和线
图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要额外的步骤。  
    1.首先，你需要创建路径起始点。  
    2.然后你使用画图命令去画出路径。  
    3.之后闭合路径。  
    4.生成路径后，你可以通过通过描边或填充来渲染图形。  
一下是用到的函数：  
```
    beginPath() // 新建一条路径的起点
    closePath() // 闭合路径，绘制命令重新指向上下文

    moveTo(x, y) // 将画笔移动到指定的(x, y)位置
    lineTo(x, y) // 绘制一条直线从当前位置到(x, y)位置

    stroke() // 绘制轮廓
    fill() // 绘制填充
```
__示例__
```
    const canvas = document.getElementById("canvas");
    const cxt = canvas.getContext("2d");
    cxt.fillRect(200, 100, 400, 200);
    cxt.clearRect(250, 150, 300, 100);
    cxt.strokeRect(300, 175, 200, 50);

    cxt.fillStyle = "#000000";
    cxt.rect(0, 0, 200, 100);
    cxt.fill();

    cxt.strokeStyle = "#000000";
    cxt.rect(600, 300, 200, 100);
    cxt.stroke();
```
![1](./images/1.jpg)

> __注意__：当前路径为空，即调用beginPath()之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是moveTo（），无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。  

> __注意__：当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。  
#### 圆弧
绘制圆弧或者圆，我们使用arc()方法。当然可以使用arcTo()，不过这个的实现并不是那么的可靠，所以我们这里不作介绍。  
```
    // (x, y)为圆心位置,radius为半径，startAngle和endAngle为开始和结束弧度，
    // anticlockwise为绘制圆弧的方向
    arc(x, y, radius, startAngle, endAngle, anticlockwise)

    // (x1, y1)第一个控制点，(x2, y2)第二个控制点，radius半径
    arcTo(x1, y1, x2, y2, radius)
```
> 注意：arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式: 弧度=(Math.PI/180)*角度。  
#### 二次贝塞尔曲线及三次贝塞尔曲线
```
    // 绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
    quadraticCurveTo(cp1x, cp1y, x, y)

    // 绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。
    bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
```

- - -

## 文本样式
- 当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 10px sans-serif。
  + font = value
- 文本对齐选项. 可选的值包括：_start_, _end_, _left_, _right_ or _center_. 默认值是 start。
  + textAlign = value
- 基线对齐选项. 可选的值包括：_top_, _hanging_, _middle_, _alphabetic_, _ideographic_, _bottom_。默认值是 alphabetic。
  + textBaseline = value
- 文本方向。可能的值包括：_ltr_, _rtl_, _inherit_。默认值是 inherit。
  + direction = value
```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  ctx.font = "48px serif";
  ctx.strokeStyle = "rgb(255, 0, 0)";
  ctx.textBaseline = "hanging";
  ctx.strokeText("Hello World!", 0, 0);
```

- - -

## 阴影(Shadows)
- shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
  + ```shadowOffsetX = float```

- shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
  + ```shadowOffsetY = float```

- shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。
  + ```shadowBlur = float```

- shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。
  + ```shadowColor = color```

**文字阴影**
```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  ctx.font = "48px serif";
  ctx.strokeStyle = "rgb(255, 0, 0)";
  ctx.textBaseline = "hanging";

  ctx.shadowOffsetX = 4;
  ctx.shadowOffsetY = 4;
  ctx.shadowBlur = 2;
  ctx.shadowColor = "rgba(225, 0, 0, .5)";

  ctx.strokeText("Hello World!", 0, 0);
```
**图形阴影**
```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  ctx.fillStyle = "rgb(255, 0, 0)";
  // ctx.shadowOffsetX = 4;
  // ctx.shadowOffsetY = 4;
  ctx.shadowBlur = 8;
  ctx.shadowColor = "rgba(225, 0, 0, .5)";
  
  ctx.beginPath();
  ctx.rect(20, 20, 100, 50);
  ctx.closePath();
  ctx.fill();
```

- - -

## 填充规则
> ```CanvasRenderingContext2D.fill(rule)```

> ```CanvasRenderingContext2D.clip(rule)```

> ```CanvasRenderingContext2D.isPointinPath(rule)```

rule可能的值:  
- _nonzero_ 默认值
- _evenodd_ 奇偶环绕
```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");
  /* 矩形清空 */
  ctx.fillRect(20, 20, 100, 50);
  ctx.clearRect(30, 30, 80, 30);

  /* 保存当前状态 */
  ctx.save();

  /* 使用裁剪 */
  ctx.beginPath();
  ctx.fillStyle = "rgb(255, 0, 0)";
  ctx.rect(20, 90, 100, 50);
  ctx.fill();
  ctx.closePath();
  ctx.clip();
  ctx.beginPath();
  ctx.fillStyle = "rgb(255, 255, 255)";
  ctx.fillRect(30, 100, 80, 30);
  ctx.closePath();

  /* 恢复上次保存的状态 */
  ctx.restore();

  /* 顺逆时针 绘制圆环 */
  ctx.arc(150, 45, 30, Math.PI*2, 0, false);
  ctx.arc(150, 45, 20, Math.PI*2, 0, true);
  ctx.fill();

  /* 奇偶环绕 绘制圆环 */
  ctx.arc(210, 45, 30, Math.PI*2, 0, false);
  ctx.arc(210, 45, 20, Math.PI*2, 0, false);
  ctx.fill("evenodd");
```

- - -

## 线型
- 线条宽度
  + ```lineWidth = value```

- 线条末端样式
  + ```lineCap = type```
    _butt，round 和 square_

- 线条与线条间接合处的样式
  + ```lineJoin = type```
    _round, bevel 和 miter_

- 两条线相交时交接处最大长度
  + ```miterLimit = value```

- 线条宽度
  + ```lineWidth = value```

- 返回一个包含当前虚线样式，长度为非负偶数的数组
  + ```getLineDash()```

- 设置当前虚线样式
  + ```setLineDash(segments)```

- 设置虚线样式的起始偏移量
  + ```lineDashOffset = value```

- - -

## 透明度(Transparency)
> ```CanvasRenderingContext2D.globalAlpha = transparencyValue```

> ```CanvasRenderingContext2D.strokeStyle = rgba(red, green, blue, opcity)```

> ```CanvasRenderingContext2D.fillStyle = rgba(red, green, blue, opcity)```

- - -

## 裁剪(clip)
> ```CanvasRenderingContext2D.clip()```
```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  /* 设置绘图区域 */
  ctx.beginPath();
  ctx.arc(50, 50, 50, 0, Math.PI*2, true);
  ctx.closePath();
  /* 裁剪绘图区域 */
  ctx.clip();

  /* 绘图 */
  ctx.fillStyle = "#0000ff";
  ctx.beginPath();
  ctx.arc(100, 50, 50, 0, Math.PI*2, true);
  ctx.closePath();
  ctx.fill();
```
- - -

## 变形
在了解变形之前，我先介绍两个在你开始绘制复杂图形时必不可少的方法  
__save()__ 保存当前画布所有状态  
__restore()__ save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照  
Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。一个绘画状态包括：  
+ 当前应用的变形
+ 以及下面这些属性：strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, lineDashOffset, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation, font, textAlign, textBaseline, direction, imageSmoothingEnabled
+ 当前的裁切路径（clipping path）
你可以调用任意多次 save方法。每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复  
变形的方法有以下几种：  
+ translate(x, y) 方法接受两个参数。x 是左右偏移量，y 是上下偏移量
+ scale(x, y) 方法可以缩放画布的水平和垂直的单位。两个参数都是实数，可以为负数，x 为水平缩放因子，y 为垂直缩放因子，如果比1小，会比缩放图形， 如果比1大会放大图形。默认值为1， 为实际大小
+ rotate(angle) 这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值
+ transform(a, b, c, d, e, f)
    + a (m11) 水平方向的缩放
    + b(m12) 水平方向的倾斜偏移
    + c(m21) 竖直方向的倾斜偏移
    + d(m22) 竖直方向的缩放
    + e(dx) 水平方向的移动
    + f(dy) 竖直方向的移动
+ setTransform(a, b, c, d, e, f) 这个方法会将当前的变形矩阵重置为单位矩阵，然后用相同的参数调用 transform 方法。如果任意一个参数是无限大，那么变形矩阵也必须被标记为无限大，否则会抛出异常。从根本上来说，该方法是取消了当前变形,然后设置为指定的变形,一步完成
+ resetTransform() 重置当前变形为单位矩阵，它和调用以下语句是一样的：ctx.setTransform(1, 0, 0, 1, 0, 0)
> __注意：__ 变形改变的是坐标系

- - -

## 组合(Compositing)
> ```CanvasRenderingContext2D.globalCompositeOperation = type```  

*type* 取值参考[链接地址](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation)
```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  ctx.fillStyle = "#0000ff";
  ctx.beginPath();
  ctx.arc(50, 50, 50, 0, Math.PI*2, true);
  ctx.closePath();
  ctx.fill();

  ctx.globalCompositeOperation = "color";
  
  ctx.fillStyle = "#ff0000";
  ctx.beginPath();
  ctx.arc(100, 50, 50, 0, Math.PI*2, true);
  ctx.closePath();
  ctx.fill();

  ctx.fillStyle = "#00ff00";
  ctx.beginPath();
  ctx.arc(75, 100, 50, 0, Math.PI*2, true);
  ctx.closePath();
  ctx.fill();
```
> **注意** ```ctx.globalCompositeOperation = "color"```位置不同可能结果不同

- - -

## 图案样式(CanvasPattern)
> 使用```createPattern(image, type)```方法
```
  var ctx = document.getElementById('canvas').getContext('2d');

  // 创建新 image 对象，用作图案
  var img = new Image();
  img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';
  img.onload = function() {

    // 创建图案
    var ptrn = ctx.createPattern(img, 'repeat');
    ctx.fillStyle = ptrn;
    ctx.fillRect(0, 0, 150, 150);

  }
```
加载的图案作为填充样式使用

- - -

## 使用图片
> 使用```drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)```方法  

> ```drawImage(image, dx, dy, dWidth, dHeight)```  

> ```drawImage(image, dx, dy)```  

![drawImage](./images/Canvas_drawimage.jpg)

- - -

## 读取像素(pixel)
> 使用```getImageData(left, top, width, height)```方法  

```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  const image = document.createElement("img");
  image.crossOrigin = "anonymous";
  image.src = "./images/timg.jpg";

  image.onload = function(src) {
    ctx.drawImage(image, 0, 0, 100, 74);
  }

  canvas.onmousemove = function(event) {
    const x = event.layerX;
    const y = event.layerY;
    const pixel = ctx.getImageData(x, y, 1, 1);
    console.log(pixel.data);
  }
```
通过设置image的属性crossOrigin来确保图片的安全性，在“被污染”的画布中调用一下方法将会抛出安全错误：
 - 在```<canvas>```的上下文上调用 getImageData()
 - 在```<canvas>```上调用 toBlob()
 - 在```<canvas>```上调用 toDataURL()

 - - -

## 写入像素
> 使用```putImageData(imageData, dx, dy)```方法  

```
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  const image = document.createElement("img");
  image.crossOrigin = "anonymous";
  image.src = "./images/timg.jpg";

  image.onload = function(src) {
    ctx.drawImage(image, 0, 0, 100, 74);
    const pixel = ctx.getImageData(0, 0, 100, 74);
    /* { pixel }
    * ImageData {
    *   data: Uint8ClampedArray,
    *   colorSpace: "srgb"
    *   height
    *   width
    * }
    */
    const datas = pixel.data;
    for(let i = 0; i < datas.length; i+=4) {
      datas[i] = 255 - datas[i];         // red
      datas[i + 1] = 255 - datas[i + 1]; // green
      datas[i + 2] = 255 - datas[i + 2]; // blue
    }
    ctx.putImageData(pixel, 100, 74);
  }
```

- - -

## 反锯齿

> CanvasRenderingContext2D.imageSmoothingEnabled = Boolean  

> CanvasRenderingContext2D.mozImageSmoothingEnabled = Boolean  

> CanvasRenderingContext2D.webkitImageSmoothingEnabled = Boolean  

> CanvasRenderingContext2D.msImageSmoothingEnabled = Boolean  

- - -

## 渐变(CanvasGradient)
> Linear Gradient```CanvasRenderingContext2D.createLinearGradient(x0, y0, x1, y1)```  

> Radial Gradient```CanvasRenderingContext2D.createRadialGradient(x0, y0, r0, x1, y1, r1)```  

> CanvasGradient的方法```addColorStop(offset, color)```
```
  var canvas = document.getElementById("canvas");
  var ctx = canvas.getContext("2d");

  var gradient = ctx.createLinearGradient(0,0,200,0);
  gradient.addColorStop(0,"green");
  gradient.addColorStop(1,"white");
  ctx.fillStyle = gradient;
  ctx.fillRect(10,10,200,100);
```

- - -

## Path2D
> 构造函数 Path2D()

**方法：**
- ```Path2D.addPath()```
- ```Path2D.closePath()```
- ```Path2D.moveTo()```
- ```Path2D.lineTo()```
- ```Path2D.bezierCurveTo()```
- ```Path2D.quadraticCurveTo()```
- ```Path2D.arc()```
- ```Path2D.arcTo()```
- ```Path2D.ellipse()```
- ```Path2D.rect()```

##### 创建和拷贝路径
```
  var canvas = document.getElementById("canvas");
  var ctx = canvas.getContext("2d");

  var path1 = new Path2D();
  path1.rect(10, 10, 100,100);

  var path2 = new Path2D(path1);
  path2.moveTo(220, 60);
  path2.arc(170, 60, 50, 0, 2 * Math.PI);

  ctx.stroke(path2);
```
##### 使用SVG路径
```
  var canvas = document.getElementById("canvas");
  var ctx = canvas.getContext("2d");

  var p = new Path2D("M10 10 h 80 v 80 h -80 Z");
  ctx.fill(p);
```

- - -

## 保存图片
> HTMLCanvasElement  提供一个toDataURL()方法，此方法在保存图片的时候非常有用。它返回一个包含被类型参数规定的图像表现格式的数据链接。返回的图片分辨率是96dpi。  

```
  canvas.toDataURL('image/png')  
  // 默认设定。创建一个PNG图片  
  canvas.toDataURL('image/jpeg', quality)
``` 
> 创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。  
你也可以从画布中创建一个Blob对像  
```
  canvas.toBlob(callback, type, encoderOptions)
```