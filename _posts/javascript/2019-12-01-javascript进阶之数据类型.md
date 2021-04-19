---
layout:     post
title:      "javascript数据类型"
subtitle:   " 🎯 "
date:       2019-12-01 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

# 数据类型

Number、String、undefined、null、Boolean、Symbol、BigInt、Object

其中前七种属于`基础数据类型`存储在`栈`内存中，object属于`引用类型`存储于`堆`内存中。存储的是一个地址，多个引用会指向同一个地址、引用类型细分下来又有Array、Date、Math、Function、RegExp等、

## 数据类型检测

* typeof

  ```javascript
  typeof 1 // "number"
  typeof '1' // "string"
  typeof undefined // "undefined"
  typeof true // "boolean"
  typeof Symbol() // "symbol"
  typeof [] // "object"
  typeof [].slice // "function"
  typeof {} // "object"
  typeof null // "object"
  ```

  从上面的结果可以看出`typeof`对基础数据类型能做出准确的判断，但是对于引用类型则是统一返回了`object`,对于null的判断可以直接使用`=== null`进行判断

* instanceof

  ```javascript
  let bycicle = function (name) {
  	this.name = name
  }
  
  let fenghuang = new bycicle('fenghuang')
  
  fenghuang instanceof bycicle // true
  ```

  * 手写instanceof

    ```javascript
    function myInstanceOf (left, right) {
    	if (typeof left !== 'object' || left === null) return false
      
    	let proto = Object.getPrototypeOf(left)
    	// 循环查找，直到找到相同额的原型对象
    	while (true) {
    		// 直到null最终    
    		if (proto === null) return false
        if (proto === right.prototype) return true 
    		
    		proto = Object.getPrototypeOf(proto)
    	}
    }
    
    myInstanceOf(new Number(1), Number)
    myInstanceOf(fenghuang, bycicle)
    ```

* Object.prototype.toString

  ```javascript
  Object.prototype.toString.call(1) // "[object Number]"
  Object.prototype.toString.call('str') // "[object String]"
  Object.prototype.toString.call(true) // "[object Boolean]"
  Object.prototype.toString.call(Symbol('1')) // "[object Symbol]"
  Object.prototype.toString.call(console.log) // "[object Function]"
  Object.prototype.toString.call(null) // "[object Null]"
  Object.prototype.toString.call(undefined) // "[object Undefined]"
  Object.prototype.toString.call([1]) // "[object Array]"
  Object.prototype.toString.call({}) // "[object Object]"
  Object.prototype.toString.call(window) // "[object Window]"
  Object.prototype.toString.call(document) // "[object HTMLDocument]"
  ```

- 统一通用的数据类型检测函数

  ```javascript
  function getType (obj) {
  	// 非引用类型直接返回  
    if (typeof obj !== 'object') {
  		return typeof obj
  	}
    
    return Object.prototype.toString.call(obj).replace(/^\[object(\S+)\]$/, $1)
  }
  ```

  ## 数据类型转换

  ### 强制类型转换

  <img src="https://gitee.com/inkkk0516/typora/raw/master/image-20210327213011287.png" alt="image-20210327213011287" style="zoom:50%;" />

  

### 隐式类型转换

通过逻辑运算符（&&、 ||、！ ），运算符（+、-、*、/、）关系运算符（>、<、<=、>=） ==等，或者if、while操作。如遇到两个两个类型不一致就会出现隐式类型转换

```javascript
1+'2' //"12"
'1' + 2 //"12"
1 + null // 1
1+undefined // NaN
'1'+null //"1null"
'1' + undefined //"1undefined"
1+true //2
1+false //1
1+{} //"1[object Object]"
```

#### object 类型转换规则

* 如果部署了Symbol.toPrimitive方法，优先调用在返回

* 调用valueOf(),如果转换为基础类型则返回

* 调用toString(),如果转换为基础类型则返回

* 如果都没有返回基础类型则会报错

  ```javascript
  var a = {
      valueOf() {
          return 10
      },
      toString() {
          return '10'
      },
    	[Symbol.toPrimitive]() {
  			return 100
  		}
  }
  
  1+a // 101
  
  // 将Symbol.toPrimitive 注释掉
  1+a  // 11
  
  // 将将Symbol.toPrimitive， valueOf方法都注释掉
  1+a // 110
  
  1+ {} // '1[object Object]'
  ```