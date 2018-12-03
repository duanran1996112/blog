---
title: Model模块化
date: 2017-06-05 18:07:01
category: "Es6" 
tags: "Es6"
description: "Es6前端模块化的基本引入与导出"
---

## 引入一个模块

### Es5

在 Es5 中，我们采用 <font color="red">require</font> 来引入一个模块（commonjs）

### Es6 （Es6 模块自动采用严格模式）

在 ES6 中，我们采用 <font color="red">import</font> 来引入一个模块 或者 变量

``` bash
// 我们导出一个变量
// demo.js
export var name = "duanran";

// 我们想引入这个变量
// index.js
import {name} from 'demo.js'
// 一定要加上 {}
// 静态加载，在 编译 的时候，就取出 name 的值

console.log(name)

// duanran
```

``` bash
// 我们要同时导出 name 与 render
// demo.js
var name = "duanran";
function render() {
    console.log('render')
}
export {name,render}

// 我们想引入 name 与 render
// index.js
import {name,render} from 'demo.js'
```

``` bash
// 若我们使用了 as
// demo.js
var name = "duanran";
function render() {
    console.log('render')
}
export {name as n,render}

// 我们想引入 name 与 render
// index.js
import {n,render} from 'demo.js'
// 写 name 则会获取不到
```

## 导出一个模块

### Es5

在 Es5 中，我们采用 <font color="red">module.exports</font> 导出一个模块

### Es6 （Es6 模块自动采用严格模式）

在 Es6中，我们采用 <font color="red">export</font>，<font color="red">export default</font> 导出一个模块

#### export 

参见上面的例子

#### export default

我们用 export default 来指定导出一个 默认的 模块

``` bash
// 我们导出一个匿名函数
// demo.js
export default function (){
    console.log('default');
}

// 我们想引入这个匿名函数
// index.js
import cons from 'demo.js'
// 不要写 {}

cons();

// default
```

``` bash
// 我们导出一个命名函数
// demo.js
export default function def(){
    console.log('default');
}

// 我们想引入 def 这个函数
// index.js
import cons from 'demo.js'
// 不要写 {}

cons();

// default
```