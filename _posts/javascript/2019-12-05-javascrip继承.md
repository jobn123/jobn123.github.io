---
layout:     post
title:      "javascript继承"
subtitle:   " 🎯 "
date:       2019-12-04 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

# 原型链继承

每一个构造函数都有一个原型对象，原型对象有包含一个指向构造函数的指针，而实例则包含一个指向原型对象的指针。

```javascript
function Parent () {
	this.name = 'father';
  this.age = 40;
}

function Son () {
	this.grade = 'grade one';
}

Son.prototype = new Parent();
const son = new Son();
```

**缺点**

多个实例共享一个原型对象，当一个实例发生变化时，另一个也跟着变化。

# 构造函数继承

```javascript
function Parent() {
  this.name = 'father';
  this.age = 40;
}

Parent.prototype.getName = function () {
    return this.name
}

function Son () {
	this.grade = 'grade one';
  Parent.call(this);
}

const s = new Son()
console.log(s);
```

![image-20210418210938387](https://gitee.com/inkkk0516/typora/raw/master/image-20210418210938387.png)

**缺点**

子类无法继承父类原型链上的属性和方法，只能继承父类的实例属性和方法。

# 组合继承

```javascript
function Parent() {
  this.name = 'father';
  this.age = 40;
}

Parent.prototype.getName = function () {
    return this.name
}

function Son () {
  Parent.call(this);
}

Son.prototype = new Parent();
Son.prototype.constructor = Son; // 手动挂上构造器，指向自己的构造函数

var s1 = new Son();
var s2 = new Son();

s1.age = 12;
s2.age = 10;
```

![image-20210418211721108](https://gitee.com/inkkk0516/typora/raw/master/image-20210418211721108.png)

**缺点**

Parent调用了两次，多了一次性能开销。

**注意**

为什么要手动挂载构造器,看下`MDN`的例子

```javascript
function Parent() {};
function CreatedConstructor() {}

CreatedConstructor.prototype = Object.create(Parent.prototype);

CreatedConstructor.prototype.create = function create() {
  return new this.constructor();
}

new CreatedConstructor().create().create(); // error undefined is not a function since constructor === Parent
```

在上面的示例中，将显示异常，因为构造函数链接到Parent。

为了避免它，只需分配您将要使用的必要构造函数。

```javascript
CreatedConstructor.prototype.constructor = CreatedConstructor; // set right constructor for further using
```

# 原型式继承

```javascript
var parent = {
    name: 'parent',
    age: 40,
    friends: ["p1", "p2", "p3"],
    getName: function() {
        return this.name
    }
}

var p1 = Object.create(parent);
var p2 = Object.create(parent);

p1.age = 20;
p2.age = 30;

p1.friends.push("tom");
p2.friends.push("jack");
```

![image-20210418212910490](https://gitee.com/inkkk0516/typora/raw/master/image-20210418212910490.png)

**缺点**

多个实例的引用类型属性指向相同的内存，存在篡改的可能、

 # 寄生式继承

使用原型式继承可以获得一份目标对象的浅拷贝，然后利用这个浅拷贝的能力再进行增强，添加一些方法，这样的继承方式就叫作寄生式继承。

虽然其优缺点和原型式继承一样，但是对于普通对象的继承方式来说，寄生式继承相比于原型式继承，还是在父类基础上添加了更多的方法。

```javascript
var parent = {
    name: 'parent',
    age: 40,
    friends: ["p1", "p2", "p3"],
    getName: function() {
        return this.name
    }
}

function clone = (from) {
  var p = Object.create(from);
  p1.getFriends = function() {
    return this.friends
  }
  return p
}

let p1 = clone(p)
```

# 寄生组合式继承

```javascript
function clone (parent, child) {
  // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
  child.prototype = Object.create(parent.prototype);
  child.prototype.constructor = child;
}

function Parent6() {
  this.name = 'parent6';
  this.play = [1, 2, 3];
}

Parent6.prototype.getName = function () {
  return this.name;
}

function Child6() {
  Parent6.call(this);
  this.friends = 'child5';
}

clone(Parent6, Child6);

Child6.prototype.getFriends = function () {
  return this.friends;
}

let person6 = new Child6();
```

# ES6 extends

```javascript
class Person {
  constructor(name) {
    this.name = name
  }

  // 原型方法
  // 即 Person.prototype.getName = function() { }
  // 下面可以简写为 getName() {...}

  getName = function () {
    console.log('Person:', this.name)
  }
}

class Son extends Person {
  constructor(name, age) {
    // 子类中存在构造函数，则需要在使用“this”之前首先调用 super()。
    super(name)
    this.age = age
  }
}

const asuna = new Son('Asuna', 20)
asuna.getName() // 成功访问到父类的方法
```

