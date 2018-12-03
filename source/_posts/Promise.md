---
title: "Promise"
date: 2017-05-24 17:22:22
category: "Es6" 
tags: "Es6"
description: "Promise，异步执行 js 的方法"
---

## 前述

在 JavaScript 的世界中，所有代码都是单线程执行的

由于这个“缺陷”，导致 JavaScript 的所有网络操作，浏览器事件，都必须是异步执行

异步执行需要大量回调函数，导致页面结构混乱，形成很可怕的回调地狱

而 Promise 就是用来解决回调函数这种“缺陷”，采用链式调用的方法异步执行 JavaScript

***

## 概念

ES6 原生提供了 Promise 对象

所谓 Promise，就是一个对象，用来传递异步操作的消息。

Promise 对象有三个状态：<font color="red">Pending</font>，<font color="red">Resolved</font>，<font color="red">Rejected</font>

Pending（Promise 对象的起始状态，进行中）
Resolved（Promise 对象的成功状态）
Rejected（Promise 对象的失败状态）

Promise 对象的状态只可能从 <font color="red">Pending -> Resolved</font> 或者 <font color="red">Pending -> Rejected</font>，而且状态只要改变后不会再变

***

## Promise 对象

### 创建一个 Promise 对象

``` bash
var promise = new Promise(function(resolve,reject){ ... });
```

Promise 对象接受一个函数作为参数，函数的形参有两个参数，<font color="red">resolve</font>，<font color="red">reject</font>，是原生 Js 中自带的方法

resolve：改变 Promise 的状态，变成<font color="red">成功状态</font>，由<font color="red">（Pending -> Resolved）</font>，并且可以发送成功状态时的数据
reject：改变 Promise 的状态，变成<font color="red">失败状态</font>，由<font color="red">（Pending -> Rejected）</font>，并且可以发送失败状态时的数据

例如：我们想在 成功 时将 Promise 的状态变为<font color="red">成功</font>，并且发送 success，失败时将 Promise 的状态变为<font color="red">失败</font>，并且发送 fail

``` bash
var promise = new Promise(function (resolve,reject) {
    if(成功) {
        resolve('success'); 
        // 我们调用 resolve 这个方法，改变了 Promise 的状态
        // Promise 的状态由 Pending 变为 Resolved，并且发送 success
    } else {
        reject('fail'); 
        // 我们调用 reject 这个方法，改变了 Promise 的状态
        // Promise 的状态由 Pending 变为 Rejected，并且发送 fail
    }
});
```

### Promise 对象的 成功 与 失败 

在 Promise 对象的方法中
我们调用 <font color="red">resolve</font> 这个函数，就可以将 Promise 的状态变为<font color="red">成功</font>
我们调用 <font color="red">resject</font> 这个函数，就可以将 Promise 的状态变为<font color="red">失败</font>

例如：以<font color="red"> 图片预加预加载 </font>举例：当图片加载成功时我们把这个 img 标签发送，失败时我们发送 error 字符串

``` bash
var promise = new Promise(function (resolve,reject) {
    var img = new Image();
    img.onload = function () { 
        // 图片预加载成功
        resolve(img);
        // 我们调用 resolve 这个方法，将 Promise 状态改变为成功，并发送 img 这个标签
    }
    img.onerror = function () { 
        // 图片预加载失败
        reject('error');
        // 我们调用 reject 这个方法，将 Promise 状态改变为失败，并发送 error 这个字符串
    }
    img.src = url
})
```

### promise.then

在上面的例子中，我们都发送了数据，那我们发送的 数据 我们又如何获取到呢？

通过 promise.then 这方法来获取到，我们发送的数据

promise.then 接受<font color="red"> 两个函数 </font>作为参数（第二个参数可以不传）

第一个函数为 promise 对象成功时的<font color="red">回调函数</font>
第二个函数为 promise 对象失败时的<font color="red">回调函数</font>

两个函数均有一个形参
第一个函数形参为，promise 对象<font color="red">成功时传递来的数据</font>
第二个函数形参为，promise 对象<font color="red">失败时传递来的数据</font>

我们看一下下面这个例子：
``` bash
// 首先创建 Promise 对象
var promise = new Promise(function (resolve,reject) {
    if(成功) {
        resolve('success');
    } else {
        reject('fail');
    }
});

//利用实例化对象调用 promise.then 方法
promise.then(function (data) {
    // data 就是我们 resolve 发送的数据
    console.log(data)
    // 如果成功的话 控制台会打印 success
},function (error) {
    // error 就是我们 reject 发送的数据
    console.log(error)
    // 如果失败的话 控制台会打印 fail
})
```

### promise.catch

Promise 对象上还有一个 promise.catch 方法

如果 promise.then 不传第二个参数的话，我们还可以用 promise.catch 方法来获取 promise 对象失败时传递来的数据

promise.catch 接受<font color="red"> 一个函数 </font>作为参数

函数为 promise 对象失败时的<font color="red">回调函数</font>

函数形参为，promise 对象<font color="red">失败时传递来的数据</font>

例如：
``` bash
// 首先创建 Promise 对象
var promise = new Promise(function (resolve,reject) {
    if(成功) {
        resolve('success');
    } else {
        reject('fail');
    }
});

promise.then(function (data) {
    console.log(data);
}).catch(function (error) { 
    //利用实例化对象 链式 调用 promise.catch 方法
    // error 就是我们 reject 发送的数据
    console.log(error);
    // 如果失败的话 控制台会打印 fail
})
```

***

## 拓展

### Promise 实现 图片预加载

``` bash
function loadImage (url) {
    var promise = new Promise(function (resolve,reject) {
        var img = new Image();
        img.onload = function () {
            resolve(img);
        }
        img.onerror = function () {
            reject('error')
        }
        img.src = url
    })
    return promise;
}

var myImg = loadImage('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1495438302464&di=8e9f1d9c871d8d5c1f653c06e2889448&imgtype=0&src=http%3A%2F%2Fv1.qzone.cc%2Favatar%2F201303%2F18%2F17%2F14%2F5146daf314dfa660.jpg%2521180x180.jpg');

myImg.then(function (img) {
    document.body.appendChild(img)
},function (error) {
    console.log(error)
})
```

### Promise 实现 Ajax 获取数据

``` bash
function ajax (url,method,type,data) {
    var promise = new Promise(function (resolve,reject) {
        var xml = null;
        if(window.XMLHttpRequest) {
            xml = new window.XMLHttpRequest();
        } else {
            xml = new ActiveXObject('Mircosoft.XMLHTTP');
        }
        if(method == "GET") {
            url += '?' + data + new Date().getTime();
            xml.open(method,url,type);
        } else {
            xml.open(method,url,type);
        }
        xml.onreadystatechange = function () {
            if(xml.readyState == 4 && xml.status == 200) {
                resolve(xml.responseText);
            } else {
                reject('error');
            }
        }
        if(method == "GET") {
            xml.send();
        } else {
            xml.setRequestHeader('Content-Type', 'application/x-www-form-urle');
            xml.send(data);
        }
    }) 
    return promise;    
}

var myAjax = ajax('http://www.baidu.com','GET',false);

myAjax.then(function (data) {
    console.log(data);
},function (err) {
    console.log(err);
})
```