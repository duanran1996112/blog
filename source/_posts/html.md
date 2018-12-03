---
title: html
date: 2017-07-23 17:59:19
category: 
    - "review" 
    - "html"
tags: 
    - "review" 
    - "html"
description: "html + html5 基础内容"
---

## html & html5

### 行级元素 & 块级元素

行级元素（inline）：span，a，em，strong，br
块级元素（block）：div，h1~h6，p，ul，li，ol
行级块元素（inline-block）：img，input，textarea，select，button

### 单标签 & 双标签

单标签：img，input，meta，link，br，hr
双标签：div，span，h1~h6，p，script，style

### meta 标签

&lt;meta charset="utf-8/gbk/..."/&gt;
用于设置字符集

&lt;meta name="keywords" content="xxx"/&gt;
用于告诉搜索引擎，你网页的关键字

&lt;meta name="description" content="xxx"/&gt;
用于告诉搜索引擎，你网站的主要内容

&lt;meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"/&gt;
用于设计移动端网页（页面可视区大小 等于 手机尺寸）

&lt;meta name="robots" content="none"/&gt;
用于搜索引擎爬虫的索引方式

&lt;meta name="author" content="xxx"/&gt;
用于标注网页的作者 　

### input 标签

input 标签传统 type 属性：text／password／image／radio／checkbox／button／submit／reset／file
input 标签 h5 type 属性：email／url／number／range／date／...

问题：input 默认显示 你好，若输入内容则消失？
答案：

方法一：onfocus，onblur
``` bash
<input type="text" 
       onfocus="if(this.value=='你好'){this.value=''}" 
       onblur="if(!this.value){this.value='你好'}" 
       value="你好">
```

方法二：h5 placeholder
``` bash
<input type="text" placeholder="你好">
```

### h5 新增标签

结构性标签：主体标签，非主体标签
主体标签：header（页眉），footer（页脚），hgroup（标题组），address（地址栏）
非主体标签：article（一个模块），section（一个区块），nav（导航），aside（侧边栏）

非结构性标签：audio，video，canvas

问题：若 ie 不兼容 h5 新标签怎么办？
答案：使用 js 的 createElement 方法，在 js 代码生成标签插入到页面里。

### Audio & Video 标签

audio 和 video 标签上的一些 属性 与 方法:

自动播放: autoplay（行间属性）
进度条: controls（行间属性）
播放: play（js 方法）
暂停: pause（js 方法）
已经播放时间: currentTime（js 方法）
视频的总时长: duration（js 方法）
音量: volume（js 方法）（0～1）
播放速度: playbackRate（js 方法）（默认1）

### canvas 基本操作（canvas 位图 缩放失真）

使用canvas：
var canvas =document.getElementById('myCanvas');
var ctx = canvas.getContext('2d');

绘制线段：ctx.moveTo(x1,y1) + ctx.lineTo(x2,y2) + ctx.fill() / ctx.stroke()
绘制矩形：ctx.rect(x,y,dx,dy) + ctx.fill() / ctx.stroke() 或 ctx.fillRect(x,y,dx,dy) 或 ctx.strokRect(x,y,dx,dy)
绘制圆形：ctx.arc(x,y,r,起始弧度,结束弧度,弧度方向(0或1)) + ctx.fill() / ctx.stroke()
绘制文本：ctx.fillText('xxx') 或 ctx.strokeText('xxx')
嵌入图片：ctx.drawImage(img,x,y,dx,dy)

设置 canvas 填充 & 描边 样式：ctx.fillStyle = 'xxx' ～ ctx.strokeStyle = 'xxx'
开始绘制 & 结束绘制：ctx.beginPath() ～ ctx.closePath()

问题：canvas高斯模糊怎么做？
答案：
``` bash
var canvas =document.getElementById('myCanvas');
var ctx = canvas.getContext('2d');

// w：图片的宽，h：图片的高，canvasW：画布的宽，canvasH：画布的高，gaussBluer：高斯模糊封装函数

ctx.drawImage(img, 0, 0, w, h, 0, 0, canvasW, canvasH) // 将图片放到canvas

var pixel = ctx.getImageData(0, 0, canvasH, canvasH) // 获取图片像素信息，受到同源策略的限制

gaussBlur(pixel); // 高斯模糊

ctx.putImageData(pixel, 0, 0); // 将像素信息放回canvas

var imageData = canvas.toDataURL(); // 将canvas的内容抽取成一张图片，受到同源策略的限制

dom.css('background-image', 'url(' + imageData + ')'); // 变成图片的src
```

### svg 基本操作（svg 矢量图 缩放不失真）

使用svg：
&lt;svg width=“500px” height=“500px”&gt;&lt;/svg&gt;

绘制直线: &lt;line x1="xxx" y1="xxx" x2="xxx" y2="xxx"&gt;&lt;/line&gt;
绘制矩形: &lt;rect x="xxx" y="xxx" width="xxx" height="xxx" rx="xxx" ry="xxx"″&gt;&lt;/rect&gt;
绘制圆形: &lt;circle r="xxx" cx="xxx" cy="xxx"&gt;&lt;/circle&gt;
绘制椭圆: &lt;ellipse rx="xxx" ry="xxx" cx="xxx" cy="xxx"&gt;&lt;/ellipse&gt;

路径:
&lt;path d = "M xxx xxx L xxx xxx" /&gt; // 从(xxx,xxx)到(xxx,xxx)
&lt;path d="M xxx xxx H xxx V xxx"/&gt; // H横向移动 V纵向移动
A 命令绘制圆弧
Z 命令回到起点

### html5 地理位置

html5获取当前地理位置：window.navigator.geolocation

为设备绑定运动状态的事件：devicemotion // 可实现摇一摇等功能

``` bash
window.addEventListener('devicemotion',function () {},false)
```

### html5 拖拽

html5拖拽:
&lt;div draggble="true"&gt;&lt;/div&gt; 元素可以拖拽

dragstart 被拖拽元素 开始时触发
dragend 被拖拽元素 结束时触发
dragenter 目标元素 拖拽元素进入目标元素
dragover 目标元素 拖拽元素在目标元素上移动
drop 目标元素 被拖拽的元素在目标元素上同时鼠标放开触发的事件

h5 中小方块拖拽：
``` bash
// html
<div draggable="true" id="dragDiv"></div>

// css
#dragDiv {
    position: absolute;
    height: 100px;
    width: 100px;
    background: red;
}

// js
var div = document.getElementById('dragDiv');
var touchX,touchY;

div.addEventListener('dragstart',function (e) {
    var x = e.clientX;
    var y = e.clientY;
    var left = this.offsetLeft;
    var top = this.offsetTop;

    touchX = x - left;
    touchY = y - top;

    document.addEventListener('dragover',move,false);

    div.addEventListener('dragend',function () {
        document.removeEventListener('dragover',move,false);
    },false)


},false);

function move (e) {
    div.style.left = e.clientX - touchX + 'px';
    div.style.top = e.clientY - touchY + 'px';
}
```

### html5 运动

html运动：requestAnimationFrame() // 每秒 60 贞，进行运动

使用方法：递归调用
``` bash
function move(){
    requestAnimationFrame(move);
    // ... 运动代码
};

var animationFrame = requestAnimationFrame(move);
```

解除 requestAnimationFrame 的运动:cancelAnimationFrame()

解除方法：
``` bash
function move(){
    requestAnimationFrame(move);
    // ... 运动代码
};

var animationFrame = requestAnimationFrame(move);

setTimeout(function () {
    cancelAnimationFrame(animationFrame)
},10000)
```

问题：兼容各个浏览器的 requestAnimFrame
答案：
``` bash
window.requestAnimFrame = (function(){
    return window.requestAnimationFrame ||
        window.webkitRequestAnimationFrame ||
        window.mozRequestAnimationFrame ||
        function( callback ){
            window.setTimeout(callback, 1000 / 60);
        };
})();
```

### html5 客户端存储 & storage & cookie

客户端存储:
localStorage: 存储量在5M以上，浏览器关闭不会失效
sessionStorage: 存储量在5M以上，浏览器关闭失效
cookie: 存储量不大于4k，过期失效

localStorage/sessionStorage 操作方法:
1.setItem(name,val) 设置属性值
2.getItem(name) 获得属性值
3.removeItem(name) 移出属性值
4.clear() 清除所有属性

localstorage 的 查看 与 存储：
``` bash
JSON.parse(window.localStorage.getItem('xxx'))
window.localStorage.setItem('xxx',JSON.stringify({...}))
```

cookie 操作方法:

1.存储 cookie：document.cookie = "name=duanran;expires=10000;" // 时间为10000毫秒
2.查看 cookie：document.cookie
3.删除 cookie：document.cookie = "name=;expires=-1;" // 时间设置为-1

cookie 的封装函数：
1.存储 cookie
``` bash
function setCookie(name, value, time) {
    var oDate = new Date();
    oDate.setDate(oDate.getDate() + time);
    document.cookie = name + '=' + value + ';expires=' + oDate + ';';
}
```

2.查看 cookie
``` bash
function getCookie(key) {
    var arr = document.cookie.split(';');
    for(var i = 0, len = arr.length; i < len; i++) {
        var temp = arr[i].split('=');
        if(temp[0] == key) {
            return temp[1];
        }
    }
}
```

3.删除 cookie
``` bash
function removeCookie (key) {
    setCookie(key, '', -1);
}
```
