---
layout:     post
title:      "javascript call、apply、bind"
subtitle:   " 🎯 "
date:       2019-12-07 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

# call

```javascript
Function.prototype.call = function(context, ...args) {
  var context = context || window;
  context.fn = this;

  var result = context.fn(...args);
  delete context.fn;
  return result;
}
```

# apply

```javascript
Function.prototype.apply = function(context, ...args) {
  var context = context || window;
  context.fn = this;

  var result = context.fn(args);
  delete context.fn;
  return result;
}
```

# bind

```javascript
Function.prototype.bind = function (context, ...args) {
  if (typeof this !== "function") {
    throw new Error("this must be a function");
  }
  var self = this;
  var fbound = function () {
    self.apply(this instanceof self ? this : context, args.concat(Array.prototype.slice.call(arguments)));
  }
  
  if(this.prototype) {
    fbound.prototype = Object.create(this.prototype);
  }
  
  return fbound;
}
```

bind 的核心在于返回的时候需要返回一个函数，故这里的 fbound 需要返回，但是在返回的过程中原型链对象上的属性不能丢失。因此这里需要用Object.create 方法，将 this.prototype 上面的属性挂到 fbound 的原型上面，最后再返回 fbound。这样调用 bind 方法接收到函数的对象，再通过执行接收的函数，即可得到想要的结果。