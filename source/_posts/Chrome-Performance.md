---
title: Chrome 开发者工具中的 Performance
date: 2018-12-03 12:30:00
category: 
    - "工具"
tags: 
    - "工具"
description: "浅谈谷歌浏览器调试工具中的 Performance"
---

## 背景

使用Chrome DevTools 的 performance 面板可以记录和分析页面在运行时的所有活动。

## Let's Go!

我们会用 Performance 工具去分析一个现有的在线 DEMO，然后教会你怎么去分析，从而找到性能瓶颈。

### 准备

1. 由于我们的chrome浏览器安装了许多插件，这些插件的运行会影响页面的性能分析，因此最好进入一个无痕窗口。

  ![Performance 展示](/img/Performance/Performance1.jpg)

2. 在匿名模式下打开 <a href="https://googlechrome.github.io/devtools-samples/jank/">DEMO</a> 地址 

3. 打开控制台 Command + Opiton + I（Mac）或者 Control + shift + I (Windows, Linux) 

### 模拟移动端

由于 移动端 性能和 PC端 相差很大，当你想分析页面的时候，可以用 CPU 控制器（CPU Throttling）来模拟移动端设备 CPU。

1. 打开开发者工具的 Performance。

2. 确保选中 Screenshots。

3. 点击右侧的（⚙️）按钮，CPU 选中 4倍 低速。

  ![Performance 展示](/img/Performance/Performance2.jpg)

### 开始记录运行时的性能

  ![Performance 展示](/img/Performance/Performance3.jpg)

### 开始分析

#### 帧率

FPS（frames per second）是用来分析动画的一个主要性能指标，一般能够保持 60FPS 的话就很流畅了。通过 fps 图标发现是一个红色的长条，表面这些帧存在问题，存在卡顿，用户体验很差。一般在流畅的网页中，fps 呈现绿色。绿色的长条越高，说明 FPS 越高，用户体验越好。

  ![Performance 展示](/img/Performance/Performance4.jpg)

#### CPU 图表

在 CPU 图表中的各种颜色与 Summary 面板里的颜色是相互对应的，Summary 面板就在 Performance 面板的下方。CPU 图表中的各种颜色代表着在这个时间段内，CPU 在各种处理上所花费的时间。如果你看到了某个处理占用了大量的时间，那么这可能就是一个可以找到性能瓶颈的线索。

  ![Performance 展示](/img/Performance/Performance5.jpg)

#### NET & Network

NET 中每条横杠表示一种资源，横杠越长，表示请求资源所需的时间越长。每个横杠的浅色部分表示等待时间。在 Network 部分，显示详细的请求结果。

#### 屏幕快照

鼠标放在屏幕快照区域可以看出当时屏幕的截图，左右移动可以分析动画的细节。

  ![Performance 展示](/img/Performance/Performance6.jpg)

#### Frames 图表

在 Frames 图表中，把鼠标移动到绿色条状图上，DevTools 会展示这个帧的 FPS。每个帧可能都在 60 以下，都没有达到 60 的标准。

  ![Performance 展示](/img/Performance/Performance7.jpg)

### 定位问题

1. 通过查看 Summary 面板，发现 CPU 大量的时间花费在 render 上。

  ![Performance 展示](/img/Performance/Performance8.jpg)

2. 展开 Main 图表，DevTools 展示了主线程运行状况。X轴代表着时间。每个长条代表着一个 event。长条越长就代表这个 event 花费的时间越长。Y轴代表了调用栈（call stack）。在栈里，上面的 event 调用了下面的 event。

  ![Performance 展示](/img/Performance/Performance9.jpg)

3. 在性能报告中，有很多的数据。可以通过双击，拖动等等动作来放大缩小报告范围，从各种时间段来观察分析报告。

4. 在事件长条的右上角出，如果出现了红色小三角，说明这个事件是存在问题的，需要特别注意。

5. 双击这个带有红色小三角的的事件。在 Summary 面板会看到详细信息。如果点击了 app.js:95 这个链接，就会跳转到对应的代码处。

  ![Performance 展示](/img/Performance/Performance10.jpg)

6. 在 app.update 这个事件的长条下方，有很多被触发的紫色长条。如果放大这些事件长条，你会看到它们每个都带有红色小三角。点击其中一个紫色事件长条，DevTools 在Summary 面板里展示了更多关于这个事件的信息。确实，这里有很多 reflow 的警告。在 summary 面板里点击 app.js:70 链接，DevTools 会跳转到需要优化的代码处。

  ![Performance 展示](/img/Performance/Performance11.jpg)

### 附录

Summary 面板对应的 颜色 与 事件描述。

1. 蓝色(Loading)：网络通信和 HTML 解析。
2. 黄色(Scripting)：JavaScript 执行。
3. 紫色(Rendering)：样式计算和布局，即重排。
4. 绿色(Painting)：重绘。
5. 灰色(other)：其它事件花费的时间。
6. 白色(Idle)：空闲时间。

| 颜色 | 事件 |	描述 |
| :-----: | :-----: | :-----: | 
| 蓝色(Loading)	| Parse HTML | 浏览器执行 HTML 解析 |
| 蓝色(Loading)	| Finish Loading	| 网络请求完毕事件 |
| 蓝色(Loading)	| Receive Data | 请求的响应数据到达事件，如果响应数据很大（拆包），可能会多次触发该事件 |
| 蓝色(Loading)	| Receive Response| 响应头报文到达时触发 |
| 蓝色(Loading)	| Send Request| 发送网络请求时触发 |
| 黄色(Scripting) |	Animation Frame Fired	| 一个定义好的动画帧发生并开始回调处理时触发 |
| 黄色(Scripting) | Cancel Animation Frame | 取消一个动画帧时触发 |
| 黄色(Scripting) | GC Event | 垃圾回收时触发 |
| 黄色(Scripting) | DOMContentLoaded| 当页面中的DOM内容加载并解析完毕时触发 |
| 黄色(Scripting) | Evaluate Script | A script was evaluated |
| 黄色(Scripting) | Event | js事件 |
| 黄色(Scripting) | Function Call | 只有当浏览器进入到js引擎中时触发 |
| 黄色(Scripting) | Install Timer | 创建计时器（调用 setTimeout() 和 setInterval()）时触发 |
| 黄色(Scripting) | Request Animation Frame	| requestAnimationFrame() 调用预定一个新帧 |
| 黄色(Scripting) | Remove Timer | 当清除一个计时器时触发 |
| 黄色(Scripting) | Time | 调用 console.time() 触发 |
| 黄色(Scripting) | Time End | 调用 console.timeEnd() 触发 |
| 黄色(Scripting) | Timer Fired | 定时器激活回调后触发 |
| 黄色(Scripting) | XHR Ready State Change | 当一个异步请求为就绪状态后触发 |
| 黄色(Scripting) | XHR Load | 当一个异步请求完成加载后触发 | 
| 紫色(Rendering)	| Invalidate layout | 当 DOM 更改导致页面布局失效时触发 |
| 紫色(Rendering)	| Layout | 页面布局计算执行时触发 |
| 紫色(Rendering)	| Recalculate style | Chrome 重新计算元素样式时触发 |
| 紫色(Rendering)	| Scroll | 内嵌的视窗滚动时触发 |
| 绿色(Painting) | Composite Layers | Chrome 的渲染引擎完成图片层合并时触发 |
| 绿色(Painting) | Image Decode | 一个图片资源完成解码后触发 |
| 绿色(Painting) | Image Resize | 一个图片被修改尺寸后触发 |
| 绿色(Painting) | Paint | 合并后的层被绘制到对应显示区域后触发 |