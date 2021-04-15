---
layout:     post
title:      "javascript内存空间与垃圾回收机制"
subtitle:   " 🎯 "
date:       2019-12-03 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

# 栈内存

js中的基本数据类型，由于它的值大小固定，往往是存储在栈内存中的。基本数据类型由系统自动分配存储空间。具有`后进先出`的规则。

![image-20210415213453158](https://gitee.com/inkkk0516/typora/raw/master/image-20210415213453158.png)

# 堆内存

javascript中的对象如**Object 、Array 、Function 、Date**等因其大小不固定，属于引用类型，保存在堆内存中。在js中不允许直接访问保存在堆内存中的对象，所以在操作访问一个对象时，先从栈中得到内存地址,根据这个地址取对象的值。就像摆在书架上的书，我们只需要知道书的名称，就能找到想要阅读的书了。

```javascript
var a1 = 0;   // 栈 
var a2 = 'this is string'; // 栈
var a3 = null; // 栈

var b = { m: 20 }; // 变量b存在于栈中，{m: 20} 作为对象存在于堆内存中
var c = [1, 2, 3]; // 变量c存在于栈中，[1, 2, 3] 作为对象存在于堆内存中
```

![image-20210415213705478](https://gitee.com/inkkk0516/typora/raw/master/image-20210415213705478.png)

# 内存生命周期

* 分配内存
* 读、写操作使用分配的内存
* 不使用时内存回收、释放

```javascript
var a = 1; // 在栈内存中申请内存空间
console.log(a); // 使用内存
a = null; // 释放内存，垃圾回收
```

# 垃圾回收

js有自动垃圾回收机制，垃圾收集器会每隔一段时间检测出不再继续使用的值，然后释放其占用的内存。在局部作用域中，当函数执行完毕，局部变量就被垃圾收集器回收了。但是全局变量需要什么时候释放往往很难判断，应避免全局变量的使用。

**js垃圾回收算法**

* 引用计数法

  引用计数法的核心就是看一个对象是否还有指向它的引用，如果没有其他对象指向它了，说明该对象也就不再需要了。

  ```javascript
  // 创建一个对象 person，他有两个指向属性 age 和 name 的引用
  var person = {
    age: 12,
    name: 'aaaa'
  };
   
  person.name = null // 虽然设置为null，但因为 person 对象还有指向 name 的引用，因此name 不会回收
   
  var p = person
  person = 1        // 原来的 person 对象被赋值为 1，但因为有新引用 p 指向原 person 对象，因此它不会被回收
  p = null           // 原 person 对象已经没有引用，很快会被回收
  ```

  引用计数算法是个简单有效的算法。但它却存在一个致命的问题：循环引用。如果两个对象相互引用，尽管他们已不再使用，垃圾回收器不会进行回收，导致内存泄露。

  ```javascript
  function cycle () {
    var o1 = {}
    var o2 = {}
    o1.a = o2
    o2.a = o1
    return "Cycle reference!"
  }
  cycle()
  ```

* 标记清除法

  标记清除算法将“不再使用的对象”定义为“无法达到的对象”。简单来说，就是从根部（在JS中就是全局对象）出发定时扫描内存中的对象。凡是能从根部到达的对象，都是还需要使用的。那些无法由根部出发触及到的对象被标记为不再使用，稍后进行回收。

