---
layout:     post
title:      "javascript 事件循环机制"
subtitle:   " 🎯 "
date:       2019-12-09 12:00:00
author:     "Hiz"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - Javascript
    - Javascript 进阶
---

Javascript是单线程的，事件循环为了高效执行异步等操作。总体分为调用栈、消息队列，微任务队列。微任务队列在调用栈执行完毕之后立即执行，消息队列在调用栈清空之后执行。

# 执行过程

**一次 Eventloop 循环会处理一个宏任务和所有这次循环中产生的微任务**。

1. JavaScript 引擎首先从宏任务队列（macrotask queue）中取出第一个任务；
2. 执行完毕后，再将微任务（microtask queue）中的所有任务取出，按照顺序分别全部执行（这里包括不仅指开始执行时队列里的微任务），如果在这一步过程中产生新的微任务，也需要执行；
3. 然后再从宏任务队列中取下一个，执行完毕后，再次将 microtask queue 中的全部取出，循环往复，直到两个 queue 中的任务都取完

# 调用堆栈（call stack）

负责跟踪所有要执行的代码，每当一个函数执行完毕，就会在调用栈中弹出。（后进先出）

![image-20210423064149571](https://gitee.com/inkkk0516/typora/raw/master/image-20210423064149571.png)

# 微任务

process.nextTick, Promises, Object.observe, MutationObserver

# 宏任务

script(整体代码),setTimeout,setInterval,setImmediate,I/O,UI rendering,event listner

# 消息队列（message queue）

调用栈在遇到宏任务时会将回调函数放入到消息队列中，在调用栈清空的时候执行。

# 微任务队列

使用promise、aysnc、await 时创建的异步操作会加入到微任务队列在调用栈清空的时候立即执行，并且处理期间新产生的微任务也要执行

# 实例

```javascript
var p = Promise(resolve => {
	console.log(4);
	resolve(5)
})

function func1() {
	console.log(1)
}

function func2() {
	setTimeout(() => {
    console.log(2)
  })

	func1();

  console.log(3);
  p.then(resolved => {
		console.log(resolved);
  }).then(() => {
		console.log(6)
  ]})
}

func2();

// 4 1 3 5 6 2
```

**过程**

* 首先调用栈将promise 压入栈中，打印出4，从栈中弹出
* 将func2压入栈中，遇到 setTimeOut将其回调函数放到消息队列中，执行func1()打印出1，接着打印出3，然后将两个then的回调函数压入到微任务队列中。func2执行完毕从栈中弹出。
* 此时调用栈为空，将微任务队列的任务压入栈中并执行，打印出5、6、
* 最后将消息队列中的任务压入栈中并执行，打印出2。

