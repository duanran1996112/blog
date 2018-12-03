---
title: jquery源码
date: 2017-08-01 12:11:34
category: 
    - "review" 
    - "jquery"
tags: 
    - "review"
    - "jquery"
description: "jquery 原理 + jquery 上方法的原理"
---

## jquery 源码

### jquery 原理

jquery 原理：

采用封闭作用域的形式，使用立即执行函数，传递 window 和 一个回调函数 进去。
在回调函数中通过 extend 方法，为 jquery 对象上 增添 各种方法。
在使用过程中，首先 通过 init 方法 包装成 jquery 对象。
然后 通过 return this 完成链式调用。

### init 方法

init 方法：用于 选择 不同类型信息，并且 生成 jquery 对象的方法

init 方法有三个参数：$('','','')
selector：$()中第一个参数
context：$()中第二个参数
root：$()中第三个参数，如果没有传递即为 document

init 方法原理：

``` bash
1.如果 $() 中第一个参数传递的是空,直接返回自身 ----> $() / $(null) / $(undefined)
2.如果 $() 中第一个参数传递的 string 字符串
    (1).如果 字符串是一个 标签 且 长度 大于 等于三的, ----> $('<div/>')
        传递到一个长度为三的 match 数组的第二位 ----> match['','<div/>','']
    (2).否则进入一个正则匹配
        * 如果字符串中含有合法的标签, ----> $('<div/>123321')
          根据正则表达式 提取 出合法的标签,传递到 match 数组的第二位 ----> match['','<div/>','']
        * 如果字符串是以 '#' 开头,证明是一个 id,且后面跟着一个合法的字符串, ----> $('#id')
          根据正则表达式提取这个合法的字符串,传递到 match 数组的第三位 ----> match['','','id']
    (3).如果 match 数组不为空
        * 若 match 第二位有值 ,生成 dom,如果 $() 有第二个参数,且第二个参数是一个对象,
          遍历第 $() 二个参数,根据 键名 和 键值 ,添加 属性 或者 内容 ----> $('<div/>123321',{id: 'id',html: '123'})
        * 若 match 第三位有值,根原生 getElementById 方法选择 dom,并包装成 jquery 对象
    (4).否则如果 match 数组为空,且 $() 没有第二个参数 或者 第二个参数为 jquery 对象的时候, ----> $('li') / $('li',$('ul'))
        在 document 或者 第二个参数下, 根据 $(xxx).find() 方法寻找这个字符串
    (5).再否则如果 match 数组为空,且 $() 第二个参数为 字符串 或者 原生 dom 的时候, ----> $('li','ul') / $('li',dom)
        调用自身 init 方法,把第二个参数包装成 jquery 对象,在这对象下根据 $(xxx).find() 方法寻找这个字符串
3.如果 $() 中第一个参数传递的是原生 dom 节点,直接包装成 jquery 对象 ----> $(dom)
4.如果 $() 中第一个参数传递的是 函数 ,将参数传递给 document.ready(),dom ready 的简写 ----> $(function () {})
5.如果什么也不是,就包装成一个数组返回
```

### extend 方法

extend 方法：向 jquery 的原型上添加方法，也可进行深层 或者 浅层 克隆

extend 方法没有形参，根据实参的个数来判断，进行什么操作。

``` bash
1.生成一个变量 target 取出 第一个 实参
    (1).如果第一个实参不是 boolean 类型,默认标记为浅层克隆 ----> $.extend({},{},...)
    (2).如果第一个实参是 boolean,根据 boolean 的值标记为 深层 或者 浅层 克隆, ----> $.extend(true/false,{},{},...)
        true 为深层,false 为浅层,并将 第二个 实参赋值给 target          
2.如果 target 的值类型不是 对象 或者 函数 的话,将 target 变成一个空对象,target 现在就是 目标对象 ----> $.extend(123,{},...)
3.如果没有 源对象 的话, 再将 target 赋值为 jquery 自身,将 方法 或者 属性 克隆到 jquery 的原型上 ----> $.extend({...})
4.如果有 源对象 的话，根据 源对象 的个数进行循环,如果 源对象 不空的话,再依次选每一个 源对象 上的每一个属性
    (1).如果 源对象 这个属性的属性值 是 目标对象 的话, ----> $.extend(target,{src: target})
        使用 continue 进入了下一次循环,防止了无限引用,    
    (2).如果 标记 的是 深层 克隆,且 源对象 上的这个属性值是引用值的话
        根据 引用值 的 类型 创建 数组 或者 对象,
        如果 目标对象 上 已有 这个属性,就 无需 创建,直接使用,
        利用 递归调用 的方法,将 源对象 上的属性依次拷贝到 目标对象 上
    (3).否则就进行 浅层 克隆，将 源对象 的属性 赋值给 目标对象 
5.最后返回 目标对象
```

### ajax-jsonp

问题：jquery 中，ajax 中 datatype 填 jsonp 为什么可以跨域？
答案：当检测到 datatype 为 jsonp 的时候，动态创建一个 script 标签，将 script 标签中的 src 赋值为：url（ 数据地址 ）+ ' ? 或 & ' + jsonp（回掉函数名称，一般为 callback 或 cb，默认值是 callback ）+ '=' + jsonpCallback（ 定义的处理函数名字，如果有 success 方法，也会经过此方法，所以也可用 success 方法代替 ） ，将 script 标签插入到页面中，使用 get 方法获取数据，获取到数据之后再删除掉 script 标签。

问题：jquery 的 jsonp 实现 百度搜索框？
答案：

方法一：自己定义处理函数名字
``` bash
// html
<input type="text" id="input" placeholder="请输入内容">
<ul id="ul"></ul>

// js
var input = document.getElementById('input');
var ul = document.getElementById('ul');

input.onkeyup = function (e) {
    var _this = this;
    $.ajax({
        url: 'http://suggestion.baidu.com/su?wd=' + _this.value,
        type: 'get',
        dataType: 'jsonp',
        jsonp: 'cb',
        jsonpCallback: 'doJsonp',
    })
}

function doJsonp (data) {
    var data = data.s;
    var html = '';
    for(var i = 0 ; i < data.length ; i++) {
        html += '<li>';
        html += '<a href="https://www.baidu.com/s?wd=' + data[i] + '" target="_blank">';
        html += data[i];
        html += '</a>';
        html += '</li>'
    }
    ul.innerHTML = html;
} 
```

方法二：使用 jquery 自带 success 方法
``` bash
// html
<input type="text" id="input" placeholder="请输入内容">
<ul id="ul"></ul>

// js
var input = document.getElementById('input');
var ul = document.getElementById('ul');

input.onkeyup = function (e) {
    var _this = this;
    $.ajax({
        url: 'http://suggestion.baidu.com/su?wd=' + _this.value,
        type: 'get',
        dataType: 'jsonp',
        jsonp: 'cb',
        success: function (data) {
            var data = data.s;
            var html = '';
            for(var i = 0 ; i < data.length ; i++) {
                html += '<li>';
                html += '<a href="https://www.baidu.com/s?wd=' + data[i] + '" target="_blank">';
                html += data[i];
                html += '</a>';
                html += '</li>'
            }
            ul.innerHTML = html;
        }
    })
}
```

### eq 方法 & end 方法

eq 方法：（选择 第几个 元素）

``` bash
1.首先 获取 调用 eq 方法的 jquery 对象 的 长度
2.再获取到 eq 传入 的 参数，对参数进行 下面的 操作，
  通过 + 号，将 字符串 转化为 数字，
  再通过 三目运算符，兼容 正数 于 负数，
  正数 则 加 0，负数 则 加 这个 jquery 对象 的 长度，
  将计算后的值 赋值 给 一个变量 j
3.如果 j 是 大于 0 ，小于 jquery 对象长度， 
  将 调用 eq 方法的 老的 jquery 对象 的 j 位置的 元素 ，
  调用 pushStack 方法，包装成 一个新的 jquery 对象，
  为这个新的 jquery 对象 添加一个属性 prevObject，
  用于记录 调用 eq 方法 老的 jquery 对象
4.最后返回这个新的 jquery 对象
```

end 方法：（回退操作）

``` bash
1.通过 当前 jquery 对象的 prevObject 属性，
  找到 调用 eq 或者 其他方法的 老的 jquery 对象
2.如果 prevObject 不空，返回 这个 老的 jquery 对象
  否则，返回一个空对象
```

### get 方法

get 方法：（用于获取 jquery 对象中 指定 位置 或 名称 的 属性值）

``` bash
1.jquery 对象 是一个 类数组，
  如果 get 方法 传空，将 这个 类数组，变成 一个 真正的 数组
2.如果 不传空，通过 三目运算符，兼容 正数 与 负数，
  如果是 正数，直接返回 这个 类数组 该位置的 属性值，
  如果是 负数，加上 类数组 的长度，返回 这个 类数组 该位置的 属性值
```
