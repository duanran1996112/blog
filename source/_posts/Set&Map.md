---
title: Set&Map
date: 2017-06-05 13:36:07
category: "Es6" 
tags: "Es6"
description: "Es6新增数据结构"
---

## 前述

所谓数据结构，就是存储数据的一种方式

Es6新增加了 set 与 map 这两种数据结构 

***

## Set

### Set 数据结构的特点

特点：<font color="red">set 数据结构没有重复</font>

### Set 数据结构的声明

<font color="red">传递一个数组</font>

``` bash
// 1.
var set = new Set([1,2,3,4]);

// 2.
var arr = [1,2,3,4];
var set = new Set(arr);

```

结合 set 数据结构的特点与声明，我们来看一下下面这些例子：

``` bash
var set = new Set([1,1,2,2]);
console.log(set);

// Set {1, 2}
重复的 1 和 2 都被去除掉了
```

``` bash
var arr = [1,1,2,2,3,3,7,7,8,NaN,NaN,{},{}];
var set = new Set(arr);
console.log(set);

// Set {1, 2, 3, 7, 8, NaN, Object {}, Object {}}
// 重复的1, 2, 3, 7 和 NaN 都被去除掉了，而两个 空对象 被保存下来了
// 这说明 set 不能去除相同的引用值
```

### Set 数据结构上的方法

#### set.add()

为 Set 数据结构<font color="red">添加</font>某个值,返回 Set 结构本身

``` bash
var set = new Set([1,1,2,2]);
set.add(3)
set.add({})
console.log(set);

// Set {1, 2, 3, Object {}}
```

#### set.delete()

为 Set 数据结构<font color="red">删除</font>某个值,返回一个布尔值，表示删除是否成功

``` bash
var set = new Set([1,1,2,2,3,3]);
console.log(set.delete(3));
console.log(set);

// true
// Set {1, 2}
```

#### set.has()

表示 Set 数据结构<font color="red">是否含有</font>某个值,返回一个布尔值，表示该值是否为 Set 的成员

``` bash
var set = new Set([1,1,2,2,3,3]);
console.log(set.has(3));

// true
```

#### set.clear()

为 Set 数据结构<font color="red">清除所有</font>值,没有返回值

``` bash
var set = new Set([1,1,2,2,3,3,7,7,8,NaN,NaN,{},{}]);
set.clear();
console.log(set);

// Set {}
```

### Set 封装数组去重

我们利用新学的 set 数据结构，封装一下之前的数组去重：

``` bash
function dedupe (arr) { 
    return Array.from(new Set(arr));
}

var arr = [1,2,3,1,2,3];

var arr1 = dedupe(arr);
console.log(arr1);

// [1, 2, 3]
```

***

## Map

### Map 数据结构的特点

特点：map 数据结构，可以接受<font color="red">任何类型的值</font>，作为键值

它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

### Map 数据结构的声明

传递一个二维数组

``` bash
// 1.
var map = new Map([['name','duanran']]);
console.log(map)

// Map {"duanran" => "18"}

// 2.
var map = new Map([['name','duanran'],['age',18]]);
console.log(map)

// Map {"name" => "duanran", "age" => 18}

// 3.
var map = new Map([[{name: 'myName'},'duanran'],['age',18]]);
console.log(map)

// Map  {Object {name: "myName"} => "duanran", "age" => 18}

```

### Map 数据结构上的方法

#### map.size

表示 Map 数据结构的<font color="red">长度</font>

``` bash
var map = new Map([['name','duanran'],['age',18]]);
console.log(map.size);

// 2
```

#### map.set()

为 Map 数据结构<font color="red">添加属性</font>

``` bash
var map = new Map([['name','duanran']]);
map.set('age',18);
console.log(map);

// Map {"name" => "duanran", "age" => 18}
```

``` bash
var map = new Map([['name','duanran']]);
var obj = {
    age: 'myAge'
}
map.set(obj,18);
console.log(map);

// Map {"name" => "duanran", Object {age: "myAge"} => 18}
```

#### map.get()

获取 Map 数据结构某个属性的<font color="red">属性值</font>，返回值是这个属性的属性值

``` bash
var map = new Map([['name','duanran']]);
console.log(map.get('name'));

// duanran
```

#### map.has()

判断 Map 数据结构<font color="red">是否有该属性</font>，返回值是布尔值

``` bash
var map = new Map([['name','duanran']]);
console.log(map.has('name'));

// true
```

#### map.delete()

<font color="red">删除</font> Map 数据结构的某个属性，返回值是布尔值

``` bash
var map = new Map([['name','duanran'],['age',18]]);
map.delete('name');
console.log(map);

// Map {"age" => 18}
```

#### map.clear()

<font color="red">删除</font> Map 数据结构的<font color="red">所有</font>值

``` bash
var map = new Map([['name','duanran'],['age',18]]);
map.clear();
console.log(map);

// Map {}
```