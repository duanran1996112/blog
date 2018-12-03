---
title: css
date: 2017-07-24 12:17:00
category: 
    - "review" 
    - "css"
tags: 
    - "review" 
    - "css"
description: "css + css3 基础内容"
---

## css & css3 

### css 选择器

css选择器：（权重值）
!important：（无穷大）
行间样式：（1000）
id：（100）
class、属性、伪类：（10）
标签、伪元素：（1）
通配符：（0）

### display 属性

display 属性：
none：此元素不会被显示
block：此元素将显示为块级元素，此元素前后会带有换行符
inline：此元素会被显示为内联元素，元素前后没有换行符
inline-block：行内块元素
flex：css3 弹性盒子

### position 属性 & z-index 属性 & 三栏布局

postion 属性: absolute，relative，fixed（相对于视口定位），static，inherit

position 与 z-index 的关系：同一级元素，若都有 position，谁在后谁在上面。若含有 z-index，谁 z-index 大，谁在上面。可以不用考虑 position，若 z-index 相同再考虑 position。

问题：若同一级元素，都是绝对定位，其中一个 z-index 为 10，另一个 z-index 为 20，但是 z-index 为 10 的元素中含有一个 z-index 为 30 的 子元素，谁在最上面？
答案：z-index 为 20 的元素在最上面。其次是 z-index 为 30 元素。最后是 z-index 为 10 的元素。（先考虑 父级 z-index，再考虑 子级 z-index。） 

问题：如何三栏布局？
答案：
1.使用 position
``` bash
// html

<div class="left"></div>
<div class="right"></div>
<div class="center"></div>

// css
.left {
    position: absolute;
    top: 0;
    left: 0;
    height: 1000px;
    width: 100px;
    background-color: red;
}

.right {
    position: absolute;
    top: 0;
    right: 0;
    height: 1000px;
    width: 100px;
    background-color: green;
}

.center {
    margin-left: 100px;
    margin-right: 100px;
    height: 1000px;
    background-color: blue;
}
```

2.使用 float
``` bash
// html

<div class="left"></div>
<div class="right"></div>
<div class="center"></div>

// css
.left {
    float: left;
    height: 1000px;
    width: 100px;
    background-color: red;
}

.right {
    float: right;
    height: 1000px;
    width: 100px;
    background-color: green;
}

.center {
    margin-left: 100px;
    margin-right: 100px;
    height: 1000px;
    background-color: blue;
}
```

3.使用 display: flex（调换了right 与 center 的位置）
``` bash
// html

<div class="left"></div>
<div class="center"></div>
<div class="right"></div>

// css
body {
    display: flex;
}
.left{
    width: 100px;
    height: 1000px;
    background-color: red;
}
.center {
    width: 100px;
    height: 1000px;
    flex-grow: 1;
    background-color: blue;
}
.right{
    width: 100px;
    height: 1000px;
    background-color: green;
}

```

### float 属性 & 清除浮动

float 属性：left，right，none，inherit

问题：如何清楚浮动？
答案：
1.为带有浮动元素的父元素，添加伪元素（最佳推选）
``` bash
:after {
    display: block;
    clear: both;
    content: "";
}
```
2.触发bfc，为带有浮动元素的父元素，添加 overflew：hidden
3.触发bfc，为带有浮动元素的父元素，添加 position：absolute
4.触发bfc，为带有浮动元素的父元素，添加 display：inline-block
5.触发bfc，为带有浮动元素的父元素，添加 zoom：1 // _zoom: 1 ie6 // *zoom: 1 ie6 ie7

### 盒子模型 & margin 塌陷 & margin 合并

盒子模型含有：
width 属性
padiing 属性： 若 4 个值（上，右，下，左），若 3 个值（上，左右，下），若 2 个值（上下，左右）
border 属性 
margin 属性 若 4 个值（上，右，下，左），若 3 个值（上，左右，下），若 2 个值（上下，左右）

问题：如何解决 margin 塌陷 与 margin 合并？
答案：
1.触发bfc，为含有 margin 的元素的父级，添加 overflew：hidden
2.触发bfc，为含有 margin 的元素的父级，添加 border

### css 溢出打点

问题：css 单行如何益处打点？
答案：

``` bash
{
    width: 200px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}
```

问题：css 多行如何益出打点？
答案：

``` bash
{
    width: 200px;
    display: -webkit-box;
    -webkit-line-clamp: 2; // 几行就写几，这里是 3 行
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
}
```

### css 伪元素 与 伪类

css 伪元素: after，before，first-letter，first-line（使用时需要设置 content: 'xxx'）
css 伪类: hover，active，focus，link，visit，lang

### css3 兼容性

-moz- // firefox---->gecko
-ms- // ie---->trident
-webkit- // safari---->webkit
-o- // opera---->presto
无// chrome---->blink/webkit

### css3 选择器

css3选择器:

1.css3属性选择器 ele[attr="*(任意位置),^(开头),$(结尾)"]

2.css3伪类选择器

第一组：同一父级下的某个子元素
:first-child
:last-child
:nth-child(n) 同一父级下的第几个子元素 （正着来）
:nth-last-child(n) （倒着来）

第二组：（常用）一类中的某个元素

:first-of-type
:last-of-type
:nth-of-type(n) 一类中的第几个元素 （正着来）
:nth-last-of-type(n) （倒着来）

3.css3条件选择器

E>F  F是E的直接子元素
E+F  F是E的兄弟节点，而且是E后面的第一个
E~F  F是E后面的兄弟节点

### css3 图像变换 transform

1.旋转
transform: rotate(...deg) / rotateX / rotateY / rotateZ / rotate3d(0/1 , 0/1 , 0/1 , ...deg) // 1 变换，0 不变换

2.位移
transform: translate(...px , ...px) / translateX / translateY / translateZ / translate3d(...px , ...px , ...px)

3.缩放
transform: scale(数字) ／ scaleX / scaleY / scaleZ / scale3d(数字 , 数字 , 数字)

4.扭曲
transform: skew(...deg) / skewX / skewY

5.设置变换的原点
translate-origin: left top / 50% 50%
默认变换的原点为 50% 50%，为图形的中心

### css3 过度 transition

css3 过度：transition

transition 为一个复合属性，由下面几部分组成

1.transition-property 指定过渡或者动态模拟的css属性。若全部则写 all。
2.transition-duration 指定过渡所需要的时间。
3.transition-timing-function 指定过渡函数。个属性主要有：linear 动画从头到尾的速度是相同的、
ease 默认，动画以低速开始，然后加快，在结束前变慢、ease-in 动画以低速开始、ease-out 动画以低速结束、ease-in-out	动画以低速开始和结束。
4.transition-delay 指定延迟时间。

例子：

``` bash
{
    left: 1px;
    transition: all 5s linear 1s;
    // 过渡全部属性，五秒完成，匀速运动，延时一秒
}

{
    left: 100px;
    // 若该元素 left 变成 100px，则会触发过渡
}  
```

### css3 动画 animation

css3 动画：animation + keyframes

animation 为一个复合属性，由下面几部分组成：

1.animation-name：执行动画的 keyframes 的名字。
2.animation-duration：执行动画的总时长。
3.animation-timing-function：指定过渡函数。这个属性主要有：linear 动画从头到尾的速度是相同的、
ease 默认，动画以低速开始，然后加快，在结束前变慢、ease-in 动画以低速开始、ease-out 动画以低速结束、ease-in-out	动画以低速开始和结束。
4.animation-delay：执行延迟时间。
5.animation-direction：动画播放的方式。这个属性的值主要有：normal 正常播放模式、reverse 倒序播放、alternate 动画在奇数次正向播放偶数次倒序播放、alternate-reverse 动画在奇数次倒序播放偶数次正向播放。
6.animation-iteration-count：动画执行的次数。infinite 是无限次，写一个数字就是要执行几次。
7.animation-fill-mode：执行完动画后物体停止的位置。forwards 是停在结束的位置上、backwards 是快速执行初始帧（这个最好配合延迟来观察）、none 是回到初始帧的位置、both 是同时具有 forwards 和 backwards 的效果。
8.animation-play-state：控制动画的播放状态。running 是播放、paused 是暂停。

keyframes 声明一个运动函数，函数为 百分比 + 样式状态 的 结合体

例子：
``` bash
{
    left: 1px
    animation: move 5s linear 1s forwards;
    // 运动函数 move，五秒完成，匀速运动，延时一秒，停在结束的位置
}

@keyframes move {
    0% {
        left: 1px;
    }
    50% {
        left: 2px;
    }
    100% {
        left: 3px;
    }
}
```

### css3 三维动画

使用三维动画：

首先，给父级设置transform-style: preverse-3d
其次（可选/可不选），给父级设置景深: transform: prespective(...px) 或者 prespective: ...px // 景深就是父级的深度
然后，给子级使用tranform: translateZ(...px)，形成三维效果 
最后，调用 transform 上的各种 图形变换效果形成三维动画

### css3 动画加速

动画加速:

1.GPU硬件加速模式，让浏览器在渲染动画的时候从CPU转向GPU

transform: transtion3d(0,0,0);
transform: translateZ(0);

2.防止动画出现闪烁

backface-visibilty: hidden;
prespective: ...px;

### css3 多列布局 column

多列布局:

父级属性: 
1.columns: xxx // 分成几分就写几。 复合属性，含有（column-width ／ column-count）
2.column-gap: xxx //  布局两列中间宽度
3.column-rule: 20px solid green // 布局两列中间加一横线

子级属性：
1.column-span: all; // 此个子集元素独占完整一行

例子：
``` bash
//html
<div class="father">
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
</div>

// css
.father {
    columns: 5;
    column-gap: 0px;
    width: 1000px;
    height: 200px;
}
.child {
    height: 200px;
    background-color: red;
}
```

### css3 弹性盒子 flex

父级元素属性: 
1.display: flex
2.flex-wrap: no-wrap // 超出部分不换行,会压缩子元素
3.flex-direction: row（横向排列） || row-reverse（横向倒序排列） || column（纵向排列） || column-reverse（纵向倒序排列）
4.flex-flow 为 lex-direction 和 flex-wrap 的复合属性

子级元素属性:
1.flex-grow // 按比例分配剩余空间
2.flex-shrink // 按比例收缩超出空间
3.flex-basis // 设置伸缩基准值,相当于 width，会覆盖 width。 若基准值总和大于容器宽度，flex-basis 就等于 basis/(basis的总和) * 容器的宽度

问题：弹性盒子内元素如何水平垂直居中？
答案：

父级设置以下三个属性
1.display: flex;
1.justify-content: center;
2.align-items: center;

例子：
``` bash
//html
<div class="father">
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
</div>

// css
.father {
    display: flex;
    width: 1000px;
    height: 100px;
    justify-content: center;
    align-items: center;

}
.child {
    flex-basis: 100px;
    height: 50px;
    background-color: blue;           
}
```

### css3 盒子模型 box-sizing

盒子模型:

box-sizing: border-box // IE6混杂模式的盒模型 border + padding + content = 设置的宽／高（width: ...px / height: ...px）

所以 border-box 的总 宽／高 = content + margin

box-sizing: content-box // 标准模式的盒模型 content = 设置的宽／高（width: ...px / height: ...px）

所以 content-box 的总 宽／高 = content +  padding + border + margin

### css3 背景图片 background-size

1.contain：同比例放大或缩小，尽量让一张图以最大限度沾满容器，多出的部分重复显示

2.cover：同比例放大或缩小，一张图填满容器，多余的部分隐藏。

### 移动端布局（media／rem／%）

移动端布局：

1.使用 媒体查询 media，兼容不同尺寸的设备
@media (填写媒体查询属性) {}

2.使用 rem，相对于 html 跟元素大小
html {font-size: 62.5% ／ ...;}
然后在在设置 宽／高／字体大小 等属性的时候 使用 rem

3.宽高使用 百分比（%）布局，还原设计图尺寸
高无法设置百分比，可以选用 padding-top
padding-top 相对于宽设置百分比

4.使用 iconfont 等矢量图片，代替 png 等静态图片


