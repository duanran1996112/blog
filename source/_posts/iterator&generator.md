---
title: iterator&generator
date: 2017-06-05 16:40:28
category: "Es6" 
tags: "Es6"
description: "Es6新增遍历器和生成器"
---


## iterator

### iterator 概念

概念：Es6 新增的<font color="red">遍历器</font>，可以遍历 数组，Set，Map 这些结构，不可以遍历 对象

for of 循环，扩展运算符（...），Aarry.from，这些就是依据 iterator 遍历器实现的

### arrIterator 数组遍历器

#### arrIterator 原理

arrIterator 会一步一步遍历数组中每一个元素
``` bash
function arrIterator(arr) {
    var index = 0;
    return {
        next: function () {
            return index < arr.length ? 
            {value: arr[index++],done: true} :
            {value: undefined,done: false}
        }
    }
}

var arr= [1,2,3];
var it = arrIterator(arr);
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());

// Object {value: 1, done: false}
// Object {value: 2, done: false}
// Object {value: 3, done: false}
// Object {value: undefined, done: true}
```

#### arrIterator 用法

使用数组，调用新数据类型 <font color="red">Symbol</font> 上的 iterator 方法

``` bash
var arr = [1,2,3];
var arrIt = arr[Symbol.iterator]();
console.log(arrIt.next());
console.log(arrIt.next());
console.log(arrIt.next());
console.log(arrIt.next());

// Object {value: 1, done: false}
// Object {value: 2, done: false}
// Object {value: 3, done: false}
// Object {value: undefined, done: true}
```
***

### setIterator Set数据结构遍历器

#### setIterator 用法

使用Set，调用新数据类型 <font color="red">Symbol</font> 上的 iterator 方法

``` bash
var set = new Set([1,2,3]);
var setIt = set[Symbol.iterator]();
console.log(setIt.next());
console.log(setIt.next());
console.log(setIt.next());
console.log(setIt.next());

// Object {value: 1, done: false}
// Object {value: 2, done: false}
// Object {value: 3, done: false}
// Object {value: undefined, done: true}
```
***

### mapIterator Map数据结构遍历器

#### mapIterator 用法

使用Map，调用新数据类型 <font color="red">Symbol</font> 上的 iterator 方法

``` bash
var map = new Map([["name","duanran"],["age",18]]);
var mapIt = map[Symbol.iterator]();
console.log(mapIt.next());
console.log(mapIt.next());
console.log(mapIt.next());

// Object {value: Array[2], done: false}
// Object {value: Array[2], done: false}
// Object Object {value: undefined, done: true}
```

## generator

### generator 概念

概念：Es6 新增的<font color="red">生成器</font>，也是一种异步编程的解决方案

创建 generator 函数，会得到一个 iterator 的 遍历器，可以调用 next() 方法

### generator 函数的创建

需要在 function 后面加一个 * ，在函数中添加 yield 语句

``` bash
function* myGenerator() {
    yield 'hello';
    yield 'word';
    return 'ending';
}

var mg = myGenerator();

// 每次调用 next 方法，指针都会下移一位
console.log(mg.next());
console.log(mg.next());
console.log(mg.next());
console.log(mg.next());

// Object {value: "hello", done: false}
// Object {value: "word", done: false}
// Object {value: "ending", done: true}
// Object {value: undefined, done: true}
```

### generator 异步操作 javascript

可以一步一步执行 javascript（异步） ，同时解决毁掉地狱的缺陷

``` bash
function* longRunningTask(value1) {
    try {
        var value2 = yield step1(value1)
        var value3 = yield step1(value2)
        var value4 = yield step1(value3)
        var value5 = yield step1(value2)
    } catch(e) {

    }  
}

var lg = longRunningTask();

// 通过 next() 方法，想什么时候执行什么时候执行
lg.next();
lg.next();
lg.next();
lg.next();
```