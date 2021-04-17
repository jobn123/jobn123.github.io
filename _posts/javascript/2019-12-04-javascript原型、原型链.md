---
layout:     post
title:      "javascript内原型、原型链"
subtitle:   " 🎯 "
date:       2019-12-04 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

# prototype

`JavaScript` 是一种**基于原型的语言** ,每个对象都拥有一个原型对象，并以此为模板，从原型对象上继承、扩展方法和属性，这些属性和方法定义在对象构造器的`prototype`上。

![image-20210417194020317](https://gitee.com/inkkk0516/typora/raw/master/image-20210417194020317.png)

构造函数a有一个指向原型的指针，原型a.prototype有一个指向构造函数的指针a.prototype.constructor。它们之间互相引用。

# \_\_proto\_\_、constructor

每个对象（null除外）都有\_\_proto\_\_属性，它指向对象的原型。

每个原型都有一个constructor属性，指向该关联的构造函数。

```javascript
function Parent () {}

const Son = new Parent()

Son.__proto__ === Parent.prototype // true
Son.constructor === Parent // true

```

<img src="https://gitee.com/inkkk0516/typora/raw/master/image-20210417200946488.png" alt="image-20210417200946488" style="zoom:67%;" />

# 原型链

每个对象拥有一个原型对象，通过 `__proto__` 指针指向上一个原型 ，并从中继承方法和属性，同时原型对象也可能拥有原型，这样一层一层，最终指向 `null`。这种关系被称为**原型链 (prototype chain)**

当js引擎在查找属性时，会先查看本身有没有这个属性，如果不存在，就会在原型链上一层层查找。

```javascript
var A = function(){};
var a = new A();
console.log(a.__proto__); //A {}（即构造器function A 的原型对象）
console.log(a.__proto__.__proto__); //Object {}（即构造器function Object 的原型对象）
console.log(a.__proto__.__proto__.__proto__); //null
```

![image-20210417205258463](https://gitee.com/inkkk0516/typora/raw/master/image-20210417205258463.png)

**参考**

[三张图搞懂JavaScript的原型对象与原型链](https://www.cnblogs.com/shuiyi/p/5305435.html)

