---
layout:     post
title:      "web components 初探"
subtitle:   " 🖇 "
date:       2021-01-23 11:49:00
author:     "Hiz"
header-img: "img/home-bg-o.jpg"
tags:
    - web copmponents
---

# 前言
web copmponents 是原生的组件化开发技术、可以让我们创建自定义的html元素，并且功能和样式封装在组件内部不影响其他组件、它与React、Vue不冲突、React、Vue也对web copmponents提供了支持、

web copmponents中三个比较重要的概念是*自定义元素*、*shadow dom*、*HTML模板*

# 自定义元素

```javascript
class CustomElemet extends HTMLElement {
  constructor() {
    super()
  }
}
```

## 元素生命周期

生命周期可以用来加载数据，注册监听和卸载事件

* connectedCallback() 				挂载时
* disConnectedCallback()           卸载时
* adoptedCallback                      移动时
* attributeChangedCallback       属性变化时

```javascript
class CustomElemet extends HTMLElement {
  constructor() {
    super()
    // ...
    this.buttonEle = this.shadowRoot.querySelector("button")
  }
  
  async connectedCallback() {
    // 事件监听
    this.buttonEle.addEventListener("click", this.troggle.bind(this))
    const res = await api.get()
    this.init(res.data)
  }
  
  // 事件处理
  troggle () {}
  
  // 处理数据
  init() {
    ...
  }
    
  // 组件卸载、移除事件监听
  disConnectedCallback () {
    this.buttonEle.removeEventListener("click", this.troggle())
  }
}
```

# shadow dom

shadow dom与普通的document几乎一样，用来操作自定义的html元素,也是一个树形结构，与普通的dom是隔离的，需让将他挂载到普通的dom节点上

```javascript
class CustomElemet extends HTMLElement {
  constructor() {
    super()
    const template = document.getElementById('template-id')
    
    // 允许通过shadow dom api操作、访问改自定义元素内部的dom树
    this.attachShadow({mode: 'open'}).appendChild(
    	template.content.cloneNode(true)
    )
  }
}

// 注册组件
// 名字中间必须带中划线
customElements.define('my-template', CustomElemet)
```

# HTML 模板

html模板是为了方便自定义元素的结构和样式，它包含两个标签*template*和*slot*

```html
<template>
  // 组件样式
	<style></style>
  
  <div>
    <h1></h1>
    // 站位插槽，可以替换为真实元素
    <solt name='content'></solt>
  </div>
</template>
```

# 总结

使用web copmponents创建自定义组件的主要步骤

* 编写模板代码以及样式
* 创建自定义class,继承自HTMLElement
* 使用customElements.define()注册元素
* 在构造函数中使用super()调用父类构造函数，编写初始化逻辑
* 使用生命周期加载数据，注册监听和卸载事件

