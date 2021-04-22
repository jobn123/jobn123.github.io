---
layout:     post
title:      "javascript new 的原理及实现"
subtitle:   " 🎯 "
date:       2019-12-06 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

> **`new` 运算符**创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

```
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

Car.prototype.run = function () {
	console.log(this.model+'car is running')
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);

console.log(car1.make);
car1.run();
```

当代码`new Car(...)`执行时，会发生以下的事情

* 一个继承自==Car.prototype==的新对象被创建
* 使用指定的参数调用构造函数Car,并将`this`绑定到新创建的对象
* 由构造函数返回的对象就是new表达式的结果。如果构造函数没有显示返回一个对象，则使用步骤1创建的对象。

集体而言 **`new`** 关键字会进行如下的操作：

1、创建一个空的简单Javascript对象（即{}）;

 2、链接该对象（设置改对象的constructor）到另一个对象；

3、将步骤1新创建的对象作为this的上下文；

4、如果函数没有返回对象，则返回this.

**手写new**

```javascript
// 第一版
function myNew (from, ...args) {
  if (typeof form !== 'function') throw TypeError('form 需为函数');
  
  let obj = Object.create();
  obj.__proto = Object.create(form.prototype);
  
  let res = from.apply(obj, args);
  
  let isObject = typeof res === 'object' && res !== null;
	let isFunction = typeof res === 'function';

  return isObject || isFunction ? res : obj;
}

// 第二版
function _myNew() {
	// 1、获得构造函数，同时删除 arguments 中第一个参数
  Con = [].shift.call(arguments);
	// 2、创建一个空的对象并链接到原型，obj 可以访问构造函数原型中的属性
  var obj = Object.create(Con.prototype);
	// 3、绑定 this 实现继承，obj 可以访问到构造函数中的属性
  var ret = Con.apply(obj, arguments);
	// 4、优先返回构造函数返回的对象
	return ret instanceof Object ? ret : obj;
};
```

