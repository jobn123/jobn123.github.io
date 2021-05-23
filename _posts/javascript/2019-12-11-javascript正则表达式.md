---
layout:     post
title:      "javascript 正则表达式"
subtitle:   " 🎯 "
date:       2019-12-11 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

正则表达式主要用来查找、替换、验证、分割

# 创建正则

```javascript
// 1、字面量创建
let str = 'sda1231nnini44';
let reg = /\d+/g;
let res = str.match(reg);

// 2、构造函数创建
// 需要多加一个\转义，可以匹配变量
let str = 'sda1231nnini44';
// let abc = "sda";
// let reg = new RegExp(abc, "g");
let reg = new RegExp("\\d+", "g");
let res = str.match(reg);
```

# 正则匹配方法

## 正则相关方法

* test

  ```javascript
  // 匹配到返回ture 否则 返回 false
  let str = 'sda1231nnini44';
  let reg = /\d+/g;
  
  console.log(reg.test(str)); // true
  ```

  

* exec 一次匹配一个符合的结果，每次匹配成功会记住上次匹配成功的lastIndex

  ![image-20210428202701484](https://gitee.com/inkkk0516/typora/raw/master/image-20210428202701484.png)

## 字符串相关方法

* split

  ```javascript
  // 通过数字 或者 字符创 分割
  let str = 'sda1231nnini44';
  str.split(1) // ["sda", "23", "nnini44"]
  
  // 通过正则分割
  let str = 'sda1231nnini44';
  str.split(/\d+/) // ["sda", "nnini", ""]
  ```

* search

  ```javascript
  // 返回匹配到的第一个索引值,会忽略全局匹配，找不到返回-1
  let str = 'sda1231nnini44';
  str.search(1) // 3
  
  let str = 'sda1231nnini44';
  str.search(/n/g) // 7
  ```

* match

  ```javascript
  // 区分全局匹配，局部匹配
  let str = 'sda1231nnini44';
  let reg = /\d+/g;
  str.match(reg); //  ["1231", "44"]
  ```

* replace

  ```javascript
  // 1、
  let str = 'sda1231nnini44';
  let reg = /\d/g;
  
  let res = str.replace(reg, '*'); //  sda****nnini**"
  
  // 2 回调函数
  tr.replace(reg, function(arg) {
    console.log(arg);
    return *;
  })
  ```


# 正则 元字符 （特殊含义的非字母字符）

## 字符相关

* `\w` : 数字、字母、下划线
* `\W `: 非数字、字母、下划线
* `\s` : 匹配空格
* `\S` : 匹配非空格
* `\d` : 匹配数字
* `\D` : 非数字
* `. `   : 匹配非 \n \r \u2028 \u2029;

## 数量相关

* `{}`

  ```javascript
  // 匹配固定次数
  let str = 'abbbbcdef';
  let reg = /ab{3}c/g;
  reg.test(str); // false
  
  // 匹配1次或多次
  let str = 'abbbbcdef';
  let reg = /ab{1,4}c/g;
  reg.test(str); // true
  
  // 匹配1次或无穷次
  let str = 'abbbbcdef';
  let reg = /ab{1,}c/g;
  reg.test(str); // true
  ```

* `?` {0, 1}  匹配0次或者1次

* `+` {1,} 1次或者无限次

* `*`  {0,} 0次到无限次

## 位置相关

* `^` 字符串开始的位置

  ```javascript
  let str = 'abc';
  let reg = /^a/;
  str.replace(reg, '*'); // "*bc"
  
  let str = 'abc';
  let reg = /^/;
  str.replace(reg, '*'); // "*abc"
  ```

* `$` 字符结尾的位置

  ```javascript
  let str = 'abc';
  let reg = /c$/;
  str.replace(reg, '*'); // "ab*"
  
  let str = 'abc';
  let reg = /\w$/;
  str.replace(reg, '*'); // "abc*"
  ```

* `\b` 边界符 非 `\w`的都是边界

  ```javascript
  let str = 'this is a apple';
  let reg = /\bis\b/g;
  str.match(reg) // ["is"]
  
  let str = 'this is a apple';
  let reg = /is/g;
  str.match(reg) // ["is", "is"]
  ```

  

* `\B` 非边界

  ```javascript
  let str = 'this is a apple';
  let reg = /\B\w{2}\b/g;
  str.match(reg) //  ["is", "le"]
  ```

  

## 括号相关

* `()` 分组、提取值、替换

  ```javascript
  // 1、分组
  let str = 'abababc';
  let reg = /(ab){3}c/;
  
  reg.test(str) // true
  
  // 2、提取值
  let str = '2019-01-02';
  let reg = /\d{4}-\d{2}-\d{2}/;
  str.match(reg); // ["2019-01-02", index: 0, input: "2019-01-02", groups: undefined]
  
  let str = '2019-01-02';
  let reg = /(\d{4})-(\d{2})-(\d{2})/;
  str.match(reg); 
  // ["2019-01-02", "2019", "01", "02", index: 0, input: "2019-01-02", groups: undefined]
  
  console.log(RegExp.$1) // 2019
  console.log(RegExp.$2) // 01
  console.log(RegExp.$3) // 02
  
  // 3、替换 2019-01-02  --->  01/02/2019
  let str = '2019-01-02';
  let reg = /(\d{4})-(\d{2})-(\d{2})/;
  // 第一种替换方式
  str.replace(reg, "$2/$3/$1");  "01/02/2019"
  
  // 第二种替换方式
  str.replace(reg, function(arg, year, month, date) {
    return `${month}/${date}/${year}`
  }); 
  
  // 反向引用
  let className = 'news_container_nav';
  let reg = /\w{4}(-|_)\w{9}(-|_)\w{3}/;
  reg.test(className); // true
  
  // 等同于
  let reg2 = /\w{4}(-|_)\w{9}(\1)\w{3}/;
  
  ```

* `[]` 字符集合 

  ```javascript
  // 1、或
  let str = 'my name is LiLy';
  let reg = /Li[lL]y/g;
  reg.test(str) // true 
  
  // 2 - 匹配区间 ASCII码必须是连续的 -   [0-9] [a-z] [A-Z]
  let str = '123654987'
  let reg = /[0-9]/g
  reg.test(str);
  
  // 3 ^ 非区间
  let str = '123654987'
  let reg = /[^0-9]/g
  reg.test(str); // false
  
  // \d = [0-9]  \w = [a-zA-Z0-9_]
  ```

  

* `{}` 区间匹配

# 匹配模式

* `g` 全局匹配
* `i` 忽略大小写
* `m` 多行模式
* `s` 让`.`匹配到换行
* `u` 匹配unicode 编码
* `y` 粘性模式 // 用于 exec匹配到连续符合结果的值

# 命名分组

![image-20210429070225824](https://gitee.com/inkkk0516/typora/raw/master/image-20210429070225824.png)

# 零宽断言

## 正向零宽断言

* 肯定 `?=`

  ```javascript
  let str = 'iphone3iphone4iphone5iphoneX';
  let reg = /iphone(?=\d)/g;
  str.replace(reg, '苹果'); // "苹果3苹果4苹果5iphoneX"
  ```

* 否定 `?!`

  ```javascript
  let str = 'iphone3iphone4iphone5iphoneX';
  let reg = /iphone(?!\d)/g;
  str.replace(reg, '苹果'); // "iphone3iphone4iphone5苹果X"
  ```

  

## 负向零宽断言

* 肯定 `?<=`

  ```javascript
  let str = '1px10px2px20pxzpx';
  let reg = /(?<=\d+)px/g;
  str.replace(reg, "像素"); // "1像素10像素2像素20像素zpx"
  ```

* 否定 `?<!`

  ```javascript
  let str = '1px10px2px20pxzpx';
  let reg = /(?<!\d+)px/g;
  str.replace(reg, "像素"); // "1像素10像素2像素20像素zpx" // "1px10px2px20pxz像素"
  ```

# 贪婪匹配

```javascript
let str = '123456789';
let reg = /\d{2,4}/g;
str.match(reg); // ["1234", "5678"]
```

# 惰性匹配

```javascript
let str = '123456789';
let reg = /\d{2,4}?/g;
str.match(reg); // ["12", "34", "56", "78"]
```



