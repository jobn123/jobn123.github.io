---
layout:     post
title:      "javascript进阶之闭包与作用域"
subtitle:   " 🎯 "
date:       2019-12-02 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

# 作用域链

在说明作用域链之前，想来讲讲什么是作用域。

**在 JavaScript 中, 作用域为可访问变量，对象，函数的集合。**

作用域一般分为三种

- 全局作用域
- 函数作用域
- 块作用域

块级作用域是es6新增的，以`{}`形成的作用域

```javascript
// es5
var a = 0;
if(a < 10)
{
    a++;
    var b = a;
}
console.log(b); //1  b是全局变量。处于全局作用域,会成为全局对象window对象的属性

// es6
var a = 0;
if(a < 10)
{
    a++;
    let b = a; // 之前当前代码块中生效
}
console.log(b); //报错 b是if代码块中的变量，只在'if'代码块{}中生效。处于块级作用域。
```



通过下面一段代码进行理解全局作用域和函数作用域

```javascript
var a = 0;
var d = 4;

function foo () {
	var a = 1;
  var b = 2;

	function fn () {
		var b = 3;
		console.log(a); // 1
		console.log(b); // 3
		console.log(d); // 4
  }
  
  fn();
  console.log(a); // 1
}

foo();
```

<img src="https://gitee.com/inkkk0516/typora/raw/master/image-20210413191411866.png" alt="image-20210413191411866" style="zoom:75%;" />

当Javascript访问一个变量时，解释器会现在当前的作用域中查找标识符。如果没有就去父级作用域查找。一直查找到window。这样一层层向父级查找就是作用域链。

作用域链的顶端是全局对象，在全局环境中定义的变量就会绑定到全局对象中。

# 闭包

`闭包`是指有权访问另外一个函数作用域中的变量的函数

```javascript
function foo () {
  var a = 'str';
  
  function getA() {
    // 访问了外部的变量a
    console.log(a);
  }
  
  return getA;
}

var func = foo(); // func指向函数foo
func()
```

简述一下上面的执行过程

1、进入全局代码，创建全局的执行上下文，全局执行上下文压入执行上下文栈

2、全局执行上下文初始化

3、执行foo函数，创建foo函数执行上下文，foo执行上下文压入执行上下文栈

4、foo函数执行上下文初始化，创建变量对象、作用于链、this等

5、foo函数执行完毕，foo执行上下文从执行上下文栈中弹出

6、执行getA函数，创建getA函数执行上下文，getA执行上下文被压入执行上下文栈

7、执行getA上下文初始化，创建变量、作用域链、this等

8、getA执行完毕，从执行上下文栈中弹出。

根据上面的执行过程，getA函数执行的时候，foo函数上下文已经被销毁了，为什么函数getA还能取到a的值呢？

其实函数getA维护了一个作用域链。会指向foo作用域，作用域链是一个数组，结构如下

```javascript
getAContext = {
  Scope: [AO, foo.AO, globalContext.AO]
}
```

即使foo被销毁了，但是Javascript依然会foo.AO活动在内存中，getA函数可以通过自己的作用域链找到它，这是闭包实现的关键。

**闭包的作用也就显而易见了**

* 读取以及操作外部函数的私有变量
* 使得外部函数的私有变量一直活动在内存中，不会被回收清除掉
