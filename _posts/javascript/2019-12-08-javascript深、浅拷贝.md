---
layout:     post
title:      "javascript 深、浅拷贝"
subtitle:   " 🎯 "
date:       2019-12-08 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

浅拷贝只是创建了一个新的对象，复制了原有对象的基本类型的值，而引用数据类型只拷贝了一层属性，再深层的还是无法进行拷贝。深拷贝则不同，对于复杂引用数据类型，其在堆内存中完全开辟了一块内存地址，并将原有的对象完全复制过来存放。

# 浅拷贝

> 创建一个新的对象，接受要重新复制或引用的对象值。如果对象属性是基本的数据类型，复制的就是基本类型的值给新对象；但如果属性是引用数据类型，复制的就是内存中的地址，如果其中一个对象改变了这个内存中的地址，肯定会影响到另一个对象。

**常用的浅拷贝方法**

```javascript
// 1、Object.assign
var a = {b: 1, c: {d: 1}};
var tar = Object.assign({}, a);

// 2、ES6扩展运运算符
var a = {b: 1, c: {d: 1}};
var tar = {...a};

// concat 拷贝数组
let arr = [1, 2, 3];
let newArr = arr.concat();
newArr[1] = 100;
console.log(arr);  // [ 1, 2, 3 ]
console.log(newArr); // [ 1, 100, 3 ]

// slice 拷贝数组
let arr = [1, 2, {val: 4}];
let newArr = arr.slice();
newArr[2].val = 1000;
console.log(arr);  //[ 1, 2, { val: 1000 } ]
```

![image-20210422211646261](https://gitee.com/inkkk0516/typora/raw/master/image-20210422211646261.png)

浅拷贝的限制所在了——它只能拷贝一层对象。如果存在对象的嵌套，那么浅拷贝将无能为力。

**手写浅拷贝**

```javascript
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? []: {};
    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
        cloneTarget[prop] = target[prop];
      }
    }
    return cloneTarget;
  } else {
    return target;
  }
}
```

# 深拷贝

> 将一个对象从内存中完整地拷贝出来一份给目标对象，并从堆内存中开辟一个全新的空间存放新对象，且新对象的修改并不会改变原对象，二者实现真正的分离。这两个对象是相互独立、不受影响。

**常用的深拷贝方法**

```javascript
// 1. JSON.stringify
```

**手写深拷贝**

```javascript
// 简陋版
const deepClone = function (target) {
    if (typeof target === "object" && target !==null) {
        let cloneTarget = Array.isArray(target) ? []: {};
        for (let key in target) {
            let v = target[key]
            cloneTarget[key] = typeof v === "object" ? deepClone(v) : v;
        }
        return cloneTarget
    } else {
        return target
    }
}

// 改进版
const isComplexDataType = obj => (typeof obj === 'object' || typeof obj === 'function') && (obj !== null)
const deepClone = function (obj, hash = new WeakMap()) {
  if (obj.constructor === Date) 
  return new Date(obj)       // 日期对象直接返回一个新的日期对象
  if (obj.constructor === RegExp)
  return new RegExp(obj)     //正则对象直接返回一个新的正则对象
  //如果循环引用了就用 weakMap 来解决
  if (hash.has(obj)) return hash.get(obj)
  let allDesc = Object.getOwnPropertyDescriptors(obj)
  //遍历传入参数所有键的特性
  let cloneObj = Object.create(Object.getPrototypeOf(obj), allDesc)
  //继承原型链
  hash.set(obj, cloneObj)
  for (let key of Reflect.ownKeys(obj)) { 
    cloneObj[key] = (isComplexDataType(obj[key]) && typeof obj[key] !== 'function') ? deepClone(obj[key], hash) : obj[key]
  }
  return cloneObj
}
```

