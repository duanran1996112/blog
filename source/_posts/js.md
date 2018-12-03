---
title: js
date: 2017-07-25 10:24:18
category: 
    - "review" 
    - "js"
tags: 
    - "review"
    - "js"
description: "es5 + dom + bom + 正则 基础内容"
---

## js

### es5

#### js 数据类型

js 基本数据类型（5种： Number，String，Boolean，undefined(未定义)，null(空)

原始值：Number，String，Boolean，undefined，null

引用值：object(array)，function，date()，regExp()

默认为 false 的值：undefined，null，""，NaN，0，false

#### typeof & 类型转换

typeof 的返回值（6种）：numner、string、boolean、undefined、object、function

类型转换：
1.Number：Number(mix) 把<font color="red">其他类型</font>的数据转换成 数字类型 的数据。
2.String: String(mix) 把<font color="red">其他类型</font>的数据转换成 字符串类型 的数据。
3.Boolean: boolean(mix) 把<font color="red">其他类型</font>的数据转换成 布尔类型 的数据。
4.parseInt: parseInt(string,radix) 把<font color="red">字符串</font>的数据转换成 整型 数字类型 的数据。当 radix 不为空的时候，我们把第一个参数的数字当成几进制的数字来转换成十进制。
5.parseFloat: parseFloat(string) 把<font color="red">字符串</font>的数据转换成 浮点型 数字类型 的数据。
6.toString: obj.toString(mix) 把<font color="red">其他类型</font>的数据转换成 字符串类型 的数据。undefined 和 null 没有 toString 方法。

#### 预编译

预编译步骤（4步骤）：
1.创建 AO 对象。
2.寻找形参和变量声明，将变量和形参作为AO对象的属性名添加到对象中，值为undefined。值得注意的是，函数声明不叫变量。
3.将实参值和形参值相统一。
4.在函数体里面寻找函数声明，将函数名作为属性名，值为这个函数的函数体。

#### 对象 Object & new 操作符

创建一个对象（4种方法）：
1.对象字面量：var obj = {};
2.构造函数：var obj = new Object();
3.自定义构造函数：function Obj() {}; var obj = new Obj();
4.Object.create：var obj = Object.create({});

对象的增删改查：
1.增 obj.xxx = xxx;
2.改 obj.xxx = xxx;
3.查 obj.xxx
3.删 delete obj.xxx;

new 操作符：
问题：构造函数中 new 操作符干了什么？
答案：
1.在构造函数中，隐士的创建一个为 this 的空对象 var this = {};
2.在构造函数中，为 this 这个空对象添加属性 this.xxx = xxx;
3.在构造函数中，隐士的 return 这个添加了属性的 this 对象，return this;

#### 原型（prototype） & 原型链

原型定义: 原型是 function 对象的一个属性，它是构造函数制造出来的对象的公共祖先。通过这个构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象。

构造函数可以通过 function.prototype = {}; 来 添加 / 修改 原型上的属性
对象可以通过 object.__proto__ = {}; 来 添加 / 修改 原型上的属性

注意：undefined，null 没有原型

原型链定义: 原型的对象还有自己的原型，这样原型上还有原型的结构就构成了原型链。

例子：
``` bash
Father.prototype = {
    like: "you"
}

function Father () {
    this.name = "duanran";
    this.age = 18;
}

Son.prototype = new Father();

function Son () {
    this.height = '180';
}

var son = new Son();

son.__proto__ = {
    name: "DuanRan" // 修改原型的 name 属性
}

console.log(son);
```

#### this

this 的指向：
1.预编译过程中 this --> window 
2.全局作用域 this --> window
3.call / apply 可以改变 this 的指向
4.obj.function()，function() 中的 this 指向 obj，谁调用 this，this指向谁

改变 this 的指向（3种）：
1.bind：bind 函数用于将 当前函数 和 指定对象 绑定，返回一个 新的函数。当 新函数 被调用时，代码会在 指定对象 的 上下文 中执行。
2.call：Function.prototype 的一个方法。为了改变某个函数运行时的上下文。第一个参数为运行的上下文。
3.apply：Function.prototype 的一个方法。为了改变某个函数运行时的上下文。第一个参数为运行的上下文。

call ／ apply 区别：除了第一个参数，call 后面的 参数 是 一个一个 传的，而 apply 后面的 参数 是 放进 一个数组里 传的。

#### 继承

继承（6种方法）：
1.原型链继承
function Father () {}
function Son () {}
Son.prototype = new Father()

缺点：会继承过多没有用的属性，造成大量的浪费。

2.构造函数继承 call / apply
function Father () {}
function Son () {Father.call(this)}

缺点：种方式不属于继承，也访问不了原型的原型。每次构造一个对象都要走两个构造函数，效率很低。

3.共享原型
function Father () {}
function Son () {}
Son.prototype = Father.prototype;

缺点：没办法改变子类的原型，一改就两个一起改了。

4.object.create()
function Father () {}
var son = Object.create(new Father);

5.圣杯模式
``` bash
var inherit = (function (){
    var F = function(){};
    return function (C,P){
        F.prototype = P.prototype;
        C.prototype = new F();
        C.prototype.constructor = C;
        C.prototype.uber = P;
    }
}());
```

F 函数的作用：沟通 P 和 C 的原型，这样我们改变 C 的原型的时候只会影响 F 而不会影响 P。

6.extands（es6 方法）
class Father {}
class Son extands Father {}

#### 作用域 & 闭包

作用域: 变量和函数生效的区域。

作用域链: 函数可以产生作用域，函数又可以嵌套，作用域之间就会产生嵌套关系，就产生了作用域链。

闭包: 闭包就是能够读取其他函数内部变量的函数

闭包定义: 在一个 函数内部 再定义 一个函数，并且这个 内部函数 与 外部函数 的 变量 有关联，通过返回这个 内部的函 来 访问 外部函数 里面的 变量。

立即执行函数: 解闭包的一个重要方法，通过另一个新闭包来消除上一个闭包的影响
(function () {} ())
(function () {})()

例子：
``` bash
function myAge () {
    var age = 0;
    return {
        add: function () {
            age++;
        },
        say: function () {
            console.log(age);
        }
    }
}

var my = myAge();
my.say(); // 0
my.add();
my.add();
my.say(); // 2
```

#### 数组 Array

创建一个数组（2种方法）：
1.数组字面量：var arr = [];
2.数组构造函数：var arr = new Array();

注意：数组可以溢出写，不可以溢出读

数组上的方法：改变原数组，不改变原数组

##### 改变原数组
1.push 后进 ／ pop 后出

2.unshift 头进 ／ shift 头出

3.splice 截取 ／ 删除 ／ 插入
a.splice(b,c,1,2,3); // 在 a 中 从 b 位置截取到 c 位置，并替换成 1，2，3

4.reverse 倒叙

5.sort 排序 可对字符串进行排序
若不传参数：按照字符的编码顺序进行排序。例：'a'在'b'前 , '11'在'12'前
若传参数：接受一个函数作为参数
``` bash
a.sort(function (a,b) { 函数中可以传递两个参数
    return ; // return一个数值
             // 值为正a在b前
             // 值为负a在b后
    return a - b; // 从小到大排序
    return b - a; // 从大到小排序
    retutn Math.random() - 0.5 // 乱序
})
```
sort 的原理（自我感觉...）
``` bash
function sort (arr) {
    for(var i = 0 ; i < arr.length ; i++) {
        for(var j = i + 1 ; j < arr.length ; j++) {
            if((arr[i] - arr[j]) > 0) {
                var t = arr[i];
                arr[i] = arr[j];
                arr[j] = t;
            }
        }
    }
}
```

##### 不改变原数组
1.concat 连接
var a = b.concat(c); // a 数组为 b 数组 连接 c 数组

2.slice 截取 
var a = b.slice(c,d); // a 数组为 在 b 中，从 c 位置 截取到 d 位置

3.join 连接
var a = b.join(); // 若 不传 参数，则以 逗号 连接

4.(...) 扩展运算符 起到连接数组的作用
var a = [1,2,...b]; // a 数组为 1，2，连接 b 数组


##### es5 数组上的新方法
1.forEach() // 改变原数组,遍历数组中每一个元素
``` bash
arr.forEach(function (value,index,arr) {
    // value 值
    // index 序号
    // arr 整个数组
    ...
})
```

2.map() // 不改变原数组,遍历数组中每一个元素，进行操作 并 返回一个新的数组
``` bash
var arr1 = arr.forEach(function (value,index,arr) {
    ...
    return ...;
})
```
    
3.filter() // 不改变原数组,遍历数组中每一个元素，进行过滤 并 返回一个新的数组
``` bash
var arr1 = arr.forEach(function (value,index,arr) {
    ...
    return true / false;
})
```
    
4.
some() 不改变原数组,遍历数组中每一个元素,返回一个布尔值
``` bash
var boolen = arr.some(function (value,index,arr) {
    return true / false;
    // 有一组数据 return 出来则成功
})
```
every() 不改变原数组,遍历数组中每一个元素
``` bash
var boolen = arr.every(function (value,index,arr) {
    return true / false;
    // 全部数据 return 出来才成功
})
```
    
5.
reduce() 不改变原数组,从左向右依次选出两个值进行合并,返回一个布尔值
``` bash
var num = arr.reduce(function (a,b) {
    return a + b;
})
```
reduceRight() 不改变原数组,从右向左依次选出两个值进行合并
``` bash
var num = arr.reduceRight(function (a,b) {
    return a + b;
})
```

#### 数组去重 & 克隆（潜层 & 深层）

##### 数组去重

方法一：
数组中的这个数据出现过一次之后，就在一个对象中将这个元素的值的位置标记成 1。后面如果出现相同的属性值，因为这个位置已经是1了，所以就不会添加到新数组里面，从而达到了去重的效果。

``` bash
Array.prototype.unique = function () {
    var len = this.length,
    arr = [],
    obj = {};
    for (var i = 0; i < len; i++) {
        if (!obj[this[i]]) {
            obj[this[i]] = 1;
            arr.push(this[i]);
        }
    }
    return arr;
}
```

方法二：
利用 es6 中的 set 数据结构，该数据结构会消除 数组中 相同的值。再将 set 结构转化回数组结构。

``` bash
Array.prototype.dedupe = function { 
    return Array.from(new Set(this));
}
```

##### 克隆

浅层克隆（缺点：无法真正克隆引用值）源对象 里面有什么属性，目标对象 就有什么属性。但是当属性是属性是引用值的时候，改变源对象目标对象也会发生改变。
``` bash
function clone(src, tar) { // src 源对象，tar 目标对象
    var tar = tar || {}; 
    for(var prop in src) {
        tar[prop] = src[prop];
    }
    return tar; 
}
```

深层克隆：利用 递归调用 的方法，当检测到 源对象 里面的这个属性值是引用类型的。就在 目标对象 里面也创建一个 引用类型 的属性，如果是 数组 就 创建数组，如果是 对象 就 创建对象。将 源对象 里面的这个 引用值 和 目标对象 里面的 引用值 分别当做 新的 源对象 和 目标对象 进行克隆。
``` bash
function deepCopy(src,tar){
    var tar= tar|| {} ;
    for (var prop in src){
        if (typeof(src[prop]) == 'object'){
            tar[prop] = (src[prop].constructor === Array ) ? [] : {};
            deepCopy(src[prop],tar[prop]);
        }else {
            tar[prop] = src[prop];
        }
    }
    return tar;
}
```

#### 类数组

类数组：类数组并不是一个数组，但是它可以表现出数组的特性。

类数组的关键为一个 length 属性，代表其的长度
``` bash
var arr = {
    "0": "1",
    "1": "2",
    "2": "3",
    "length": "3"
}
```

问题：类数组运用在那里？
答案：jquery，zepto 的选择器，选出来的就是类数组

### dom

#### 节点的类型

节点的类型 与 对应的 nodeType（可用 nodeType 查看节点的 类型）

元素节点————>1 // 例：&lt;div&gt;
属性节点————>2 // 例：id,class
文本节点————>3 // 例：123
注释节点————>8 // 例：&lt;!—— ——&gt;
document————>9 // 例：#doucment
DocumentFragment————>11

#### 选择元素节点

选择元素节点的方法（6种）：
1.document.getElementById  // 在ie8以下的浏览器中，不区分大小写，而且标签的name属性也可以被当做id被选择出来
2.document.getElementsByClassName // ie8及以下的版本中没有这种方法
3.document.getElementsByTagName // 兼容所有浏览器
4.document.getElementsByName // 只有部分标签的name可以生效，表单、表单元素、img、iframe
css选择器: 
5.querySelector() // ie7及以下的版本中没有这种方法
6.querySelectorAll() // ie7及以下的版本中没有这种方法

#### 遍历节点树 & 遍历元素节点树

节点树的遍历：
1.dom.parentNode 查找父节点
2.dom.childNodes 查找子节点们
3.dom.firstChild 第一个子节点
4.dom.lastChild 最后一个子节点
5.dom.nextSibling 下一个兄弟节点
6.dom.previousSibling 上一个兄弟节点

元素节点树的遍历：
1.dom.parentElement 返回当前元素的父元素节点
2.dom.children 所有子元素节点
3.dom.childElementCount 这个属性就是子元素节点的数量
//  dom.children.length === dom.childElementCount
4.dom.nextElementSibling 下一个兄弟元素节点
5.dom.previousElementSibling 上一个兄弟元素节点

#### 增 ／ 插 ／ 替换 ／ 删除 操作

新增 dom 元素：
1.创建元素节点 document.createElement()
2.创建文本节点 document.createTextNode()
3.创建注释节点 document.createComment()
4.创建文档碎片 document.createDocumentFragment()

插入 dom 元素：
1.dom1.appendChild(dom2); // 在 dom1 中最后的位置插入 dom2
2.dom1.insertBefore(dom2, dom3); // 在 dom1 下，把 dom2 插入到 dom3 前

问题：insertAfter 怎么封装？
答案：
``` bash
Element.prototype.insertAfter = function (ele1,ele2) {
    if(ele2.nextElementSibling) {
        var nextElement = ele2.nextElementSibling;
        this.insertBefore(ele1,nextElement);
    } else {
        this.appendChild(ele1);
    }
}
```

替换 dom 元素：
1.dom1.replaceChild(dom2, dom3) // 在 dom1 下，用 dom2 替换 dom3 

删除 dom 元素：
1.dom1.removeChild(dom2) // 在 dom1 下，删除 dom2

#### 元素节点的 属性 & 方法

元素节点的属性：
1.dom.innerHTML() dom 元素里面的HTML结构，可以获取 也可以修改
2.dom.innerText() / dom.textContent() dom 元素里面的 文本 结构，可以获取 也可以修改
innerText 老版本的火狐浏览器不兼容，textContent 老版本的IE浏览器不兼容。

元素节点的方法：
1.dom.getAttribute('id / class / ...') 获取元素的行间属性
2.dom.setAttribute('id / class / ...', '属性值') 设置元素的行间属性

#### 脚本化 css

注意：在 js 代码中，css 属性要使用 小驼峰式 写法
例子：background-color —> backgroundColor

##### 获取 css 样式
1.window.getComputedStyle(dom,'伪元素').css; // 主流浏览器

var div = document.getElementsByTagName('div')[0];
window.getComputedStyle(div, 'after').width;

2.dom.currentStyle.css // ie

var div = document.getElementsByTagName('div')[0];
div.currentStyle.width;

``` bash
function getStyle(obj, prop, fake) {
    var fake = fake || null;
    if(obj.currentStyle) {
        return obj.currentStyle[prop];
    }else {
        return window.getComputedStyle(obj, fake)[prop];
    }
}
```

##### 查看元素的 几何 尺寸
1.dom.getBoundingClientRect().css（css 的值为 width / height / left / right / top / bottom ）// 获取元素的 宽 ／ 高 ／ left ／ right ／ top ／ bottom

var div = document.getElementsByTagName('div')[0];
div.getBoundingClientRect().width;

2.dom.offsetWidth / dom.offsetHeight / dom.offsetLeft / dom.offsetRight / dom.offsetTop / dom.offsetBottom // 获取元素的 宽 ／ 高 ／ left ／ right ／ top ／ bottom

##### 赋值 css 属性
1.dom.style.css // 赋值的 属性 为 行间样式
附加说明：改方法也可以查看 已有 的 行间样式。但是没有这个行间属性，获取出的值则是 undefined

var div = document.getElementsByTagName('div')[0];
div.style.left = '100px';

#### 滚动条 滚动距离 & 视口尺寸 大小

滚动条滚动的距离: 
1.window.pageYOffset ／ window.pageXOffset  // 主流浏览器
2.document.body.scrollTop ／ doucment.body.scrollLeft // ie

``` bash
function getScrollOffset(){
    if(window.pageYOffset) {
        return {
            x: window.pageXOffset,
            y: window.pageYOffset
        }
    }
    return {
        x: document.body.scollTop,
        y: document.body.scrollLeft
    }
}
```

查看视口尺寸：
1.window.innerHeight ／ window.innerwidth // 主流浏览器
2.document.body.clientHeight ／ document.body.clientWidth // ie

``` bash
function getViewportOffset () {
    if(window.innerWidth) {
        return {
            w: window.innerWidth,
            h: window.innerHeight
        }
    }
    return {
        w: document.body.clientWidth,
        h: document.body.clientHeight
    }
}  
```

#### 事件

##### 绑定事件

绑定事件：

主流浏览器：dom.addEventListener(事件名称, 回掉函数, false) // 是否开启事件捕获
ie：dom.attachEvent(事件名称, 回掉函数)
其他：dom.onclick = function () {} 句柄绑定

``` bash
function attachEvent(ele, type, handle) {
    if (ele.addEventListener) {
        ele.addEventListener(type, handle, null);
    }else if (ele.attachEvent) {
        ele['temp' + type + handle] = handle;
        ele[type + handle] = function () {
            ele['temp' + type + handle].call(ele);
        };
        ele.attachEvent('on' + type, ele[type + handle]);
    }else {
        ele['on' + type] = handle;
    }
}
```

##### 解除事件

解除事件：

主流浏览器：dom.removeEventListener(事件名称, 回掉函数, false)
ie：dom.detachEvent(事件名称, 回掉函数)
其他：dom.onclick = null 

``` bash
function remvoeEvent(ele, type, handle) {
    if(ele.removeEventListener) {
        ele.removeEventListener(type, handle, false);
    }else if (ele.detachEvent) {
        ele.detachEvent('on' + type, handle);
    }else {
        ele['on' + type] = null;
    }
}
```

##### 取消事件冒泡

什么是事件冒泡：同一事件，子元素 冒泡 向 父元素，并执行 父元素 的事件

取消事件冒泡：

主流浏览器：event.stopPropagation();
ie：event.cancelBubble = true;

注意：focus、blur、change、submit、reset、select 等方法就没有事件冒泡现象

``` bash
function cancelHandler(event) {
    if(event.preventDefault) {
        event.preventDefault();
    }else{
        event.returnValue = false;
    }
}
```

##### 阻止默认事件

阻止默认事件：

主流浏览器：event.preventDefault();
ie：event.returnValue = false;
其他：return false;

``` bash
function cancelHandler(event) {
    if(event.preventDefault) {
        event.preventDefault();
    }else{
        event.returnValue = false;
    }
}
```

##### 事件捕获 & 事件冒泡

事件捕获：结构上（非视觉上）嵌套关系的元素，会存在事件捕获功能，同一事件，自父元素捕获至子元素，结构上的自顶向下。

dom.addEventListener('',function () {} ,true) // true 开启事件捕获

事件冒泡：在结构上（非视觉上）嵌套关系的元素，会存在事件冒泡的功能，同一事件，子元素冒泡向父元素，结构上的自底向上。

##### 事件委托 & 事件源对象

什么是事件委托：父级添加事件，由于事件冒泡，点击子元素会调用父元素上的方法，这种方法被称为事件委托。被点击子元素被称为事件源对象

寻找事件源对象：

火狐：event.srcElement
ie：event.target
chrome：都支持

``` bash
dom.onclick = function (e) {
    var target = e.target || e.srcElement;
    console.log(target) // target 为事件源对象
}
```

事件源对象的好处：
1.不需要循环所有的子元素一个个绑定事件。
2.当有新的子元素被加入的时候不需要重新绑定事件。

##### 事件处理模型 & 事件处理机制

事件处理模型：先捕获后冒泡（从外向里，从里向外）

事件处理机制：
1.注册事件
2.初始化事件参数
3.触发事件（事件捕获，事件执行，事件冒泡）

##### 鼠标事件

鼠标事件：

mousedown，mouseup，click // 点击事件先后顺序 
mousemove // 在目标元素上移动
mouseover，mouseout // 移入 移出
contextmenu // 右键出现事件

问题：pc端小方框拖拽?
答案：
``` bash
// html
<div id="dragDiv"></div>

// css
#dragDiv {
    position: absolute;
    height: 100px;
    width: 100px;
    background: red;
}

//js
var div = document.getElementById('dragDiv');
var touchX,touchY;

div.addEventListener('mousedown',function (e) {
    var x = e.clientX;
    var y = e.clientY;
    var left = this.offsetLeft;
    var top = this.offsetTop;

    touchX = x - left;
    touchY = y - top;

    document.addEventListener('mousemove',move,false);

    div.addEventListener('mouseup',function () {
        document.removeEventListener('mousemove',move,false);
    },false)


},false);

function move (e) {
    div.style.left = e.clientX - touchX + 'px';
    div.style.top = e.clientY - touchY + 'px';
}
```

##### 键盘事件

键盘事件：

keydown，keypress，keyup // 键盘事件的先后顺序

问题：绑定回车事件？
答案：
``` bash
// html
<input type="text" id="input">

// js
var input = document.getElementById('input');
input.onkeypress = function (e) {
    if(e.keyCode  == 13) {
        console.log("回车");
    }
}
```

##### 滚轮事件

滚轮事件：
                 
scroll // 鼠标滚轮移动

问题：绑定瀑布流（滚轮滑动到底）事件？
答案：
``` bash
window.onscroll = function (e) {            
    var windowH = window.innerHeight;
    var scrollH = document.body.scrollTop;
    var documentH = document.body.scrollHeight;
    if(windowH + scrollH >= documentH) {
        console.log("滚动到底了");
    }
}
```

##### 触摸事件（移动端）

tap // 触摸事件
在移动端中 tap 事件没有延时，click 事件会有 200～300 毫秒延时，一般使用 tap。
但是 tap 会发生点击穿透，比如，为浮层绑定 tap 会触发浮层下边的点击事件。

touchstart，touchmove，touchend // 触摸事件先后顺序 （点击屏幕，在屏幕上移动，离开屏幕）

event.touches：表示当前跟踪的触摸操作的 touch 对象的数组。
event.targetTouches：特定于事件目标的 touch 对象的数组。
比如： 拖拽 等效果需要用到这两个属性。

问题：移动端小方块拖拽？
答案：
``` bash
// html
<div id="dragDiv"></div>

// css
#dragDiv {
    position: absolute;
    height: 100px;
    width: 100px;
    background: red;
}

//js
var div = document.getElementById('dragDiv');
var touchX,touchY;

div.addEventListener('touchstart',function (e) {
    var e = e.touches[0] || e.targetTouches[0];
    var x = e.clientX;
    var y = e.clientY;
    var left = this.offsetLeft;
    var top = this.offsetTop;

    touchX = x - left;
    touchY = y - top;

    document.addEventListener('touchmove',move,false);

    div.addEventListener('touchend',function () {
        document.removeEventListener('touchmove',move,false);
    },false)


},false);

function move (e) {
    var e = e.touches[0] || e.targetTouches[0];
    div.style.left = e.clientX - touchX + 'px';
    div.style.top = e.clientY - touchY + 'px';
}
```

#### 重绘 & 重排

重绘（repaint）：是元素自身的位置和宽高不变，只改变颜色的之类的属性而，不会导致后面的元素位置的变化，渲染树发生重绘。

重排（reset）：是元素自身的位置或者宽高改变了从而导致的整个页面的大范围移动的时候，渲染树发生重排，尽量避免。

### bom

#### window

window 对象表示浏览器中打开的窗口。
window 对象上有特别多的方法，一般来讲，能全局使用的都是 window 对象上的方法，例如我们常用的一些：

console，setInterval，setTimeout，navigator，screen，history，location

#### navigator

navigator对象：

1.navigator.userAgent  目前用户使用的是 pc端 还是 移动端，是什么 浏览器。

问题：若是 移动端 跳转 移动端 url，若是 pc端 跳转 pc端 url？
答案：
``` bash
if(/iphone|nokia|sony|ericsson|mot|samsung|sgh|lg|philips|panasonic|alcatel|lenovo|cldc|midp|wap|android|iPod/i.test(navigator.userAgent.toLowerCase())){
    window.location.href="移动端url";
} else {
    window.location.href="pc端url";
}
```

#### screen

screen对象：

1.screen.availHeight/screen.availWidth  可以查看除了 window 任务栏之外的 屏幕 的 高度 和 宽度。
2.screen.height/screen.width  返回 显示器 的 屏幕 的 高度 和 宽度,兼容的较少。
3.screen.deviceXDPI/screen.deviceYDPI  返回显示屏幕的分辨率。

#### history

history对象：

1.history.length  浏览历史的长度。
2.history.back()  进入到下一个历史页面。
3.history.forward()  返回到上一个历史页面。
4.history.go()  当参数是 正数 的时候，前进到 下一个 历史页面，当是 负数 的时候，回退到 上一个 历史页面。

#### location

location对象：

1.location.href  设置或返回当前的 url
2.location.host  返回当前的主机名 和 当前的 URL 端口号
3.location.search  设置或返回从 问号 开始的 URL（查询部分）
4.location.hash  跳转到 相应的 id 的 元素 的 位置
5.location.reload  重新加载 当前 页面

### 正则表达式

#### 创建一个正则表达式

创建正则表达式：

1.字面量：var reg = /xxx/gim;
2.构造函数：var reg = new RegExp('xxx','gim');

#### gim

gim：

g：全局匹配
i：忽略大小写
m：多行匹配

#### 表达式

表达式：
        
[]：匹配[]中的字符 // [a-zA-Z] 匹配 从 a 到 z，从 A 到 Z
^ ：不匹配这个字符(非的意思),在[^]使用 // [^a-zA-Z] 不匹配 从 a 到 z，从 A 到 Z
()：产生子表达式,提高优先级 // (\w)
| ：或，并列 关系和 [] 类似 // (\w|\W)

#### 元字符

元字符：

``` bash
. ：除了\n(换行符)，\r(回车符之外的)
\d：0~9
\D：除了 0~9 之外的
\w：a~zA~Z0~9_
\W：除了 a~zA~Z0~9_ 之外的
\s：\n，\r，\t(tab)，\v(垂直换行符)，\f(换页符)
\S：除了\n，\r，\t(tab)，\v(垂直换行符)，\f(换页符)之外的

[\d\D]，[\w\W]，[\s\S] 均可表示全集

\b：单词边界
\B：非单词边界
```

#### 量词

量词：

+：1~n 个该类型变量
*：0~n 个该类型变量
?：0~1 个该类型变量
{}: 几个 该类型的变量,{n} 或 {1,}={1,n}
^：以什么 开头
$：以什么 结尾
(?=)：正向 预查
(?!)：非正向 预查

#### 方法 & 例题

正则表达式上的方法：

1.reg.test(str) --> true/false // 是否可以匹配出
2.reg.exec(str) --> {"匹配出的表达式",index: "检索到的 str 位置",input: "str"}

字符串上的方法：

1.str.search(reg) --> 检索匹配出的正则表达式位置，成功则结束
2.str.match(reg) --> ["匹配出的表达式 1","匹配出的表达式 2",...]
3.str.split(reg) --> 根据正则表达式 划分
4.str.replace(reg,"") --> 根据正则表达式 替换，第二个参数可以传值（将匹配出的表达式替换为该值），也可以传一个函数 function ($,$1,$2) {} // 函数中的参数分别为：匹配出来的字符串，第一个子表达式，第二个子表达式

问题：将首字母大写？
答案：
``` bash
var str = "my name is duanran";
var reg = /\b(\w)/gim;       
console.log(str.replace(reg,function ($,$1) {
    return $1.toUpperCase();
}));
```

问题：将金钱三位一打点？
答案：
``` bash
var str = "111000000000";
var reg = /(?=(\B\d{3})+$)/gim;       
console.log(str.replace(reg,'.'));
```