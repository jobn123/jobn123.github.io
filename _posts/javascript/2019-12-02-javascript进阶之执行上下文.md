---
layout:     post
title:      "javascript进阶之执行上下文"
subtitle:   " 🎯 "
date:       2019-12-02 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

**执行上下文是当前 JavaScript 代码被解析和执行时所在环境的抽象概念。**

执行上下文分为三个类型

* 全局执行上下文，也就是`window`对象，只有一个。
* 函数执行上下文，只有在函数被调用的时候才会被创建，每次函数调用都会创建一个新的执行上下文。
* eval，运行在eval函数中的代码。

# **执行栈**

执行栈是存储在代码执行期间创建的所有执行上下文，具有后进先出的结构。

```javascript
var a = 'str';

function fn1() {
  console.log('this is fn1');
  fn2();
	console.log('fn1 again');
}

function fn2() {
  console.log('this is fn2');
}

fn1();
console.log('this is global context');

// this is fn1
// this is fn2
// fn1 again
// this is global context
```

首次运行Js代码的时候，js会创建一个全局的执行上下文，并push到当前的执行栈中，每当发生函数调用，浏览器引擎会创建一个新的函数执行上下文并push到当前执行栈的栈顶。栈顶函数执行完毕之后会从当前执行栈中弹出。上下文控制权移交到当前执行栈的下一个执行上下文。

![image-20210413213259261](https://gitee.com/inkkk0516/typora/raw/master/image-20210413213259261.png)

执行上下文有分为创建和执行两个阶段

**创建阶段**

1、确定this的值

	* 全局执行上下文中，this指向window
	* 函数执行上下文中，this的指向取决于函数调用的方式。

2、词法环境被创建

	* 存储变量和函数声明的实际位置
	* 对外部环境的引用

3、变量环境被创建

伪代码

```javascript
ExecutionContext = {  
  ThisBinding = <this value>,     // 确定this 
  LexicalEnvironment = { ... },   // 词法环境
  VariableEnvironment = { ... },  // 变量环境
}
```

**执行阶段**

完成对所有变量的分配，执行代码。