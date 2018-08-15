---
title: "Vue入门 (三): Vue语法"
date: 2018-08-12 17:32:42
categories:
- Vue
tags:
- Vue
---

> 本节着重针对Vue的主要语法做下整理。

<!-- more -->

### Vue 实例
---

#### 实例的构建

  + 通过new Vue 构建 Vue 实例 ，绑定id=root的元素，数据设定"hello world"可以使用插值表达式{{message}}来获取。定义handlerClick方法页面可以使用@click="handlerClick" 或者 v-on:click="handlerClick" 来绑定函数。

  ```html
  var vm = new Vue({
    el: '#root',
    data: {
      message: "hello world"
    },
    methods:{
      handlerClick: function () {
        alert("hello")
      }
    }
  })
  ```
  
  + Vue实例暴露了一些实例属性与方法。如 el 、 data、methods等。[更多](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)

#### Vue实例的生命周期

  每个 Vue 实例在被创建时都要经过一系列的初始化过程，在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。
  
  beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy 、destroyed

### 模板语法
---

#### 插值

  + 文本 
    + `{{ message }}` 绑定数据对象data中message指定的值，当message发生变化，html的内容会同时更新。
    + `<span v-once>{{ message }}</span>` 同上，但数据发生变化后，html内容不会更新。
  + Html
    + `{{ rawHtml  }}` 的数据会把数据解释为普通文本。而非html
    + `<span v-html="rawHtml"></span>` 可以解释html。
  + html属性
    + `<span v-bind:disabled="isDisable">test</span>"` 使用v-bind绑定。
  + JS表达式: 表达式会在Vue实例的数据作用域下作为Js被解析,但仅支持单个表达式。
    + `{{ number + 1}}`              
  + 指令
    +  `<p v-if="seen">你可以看到我</p>`   
  + 参数
    + `<a v-bind:href="url">...</a>`
    + `<a v-on:click="doSomething">...</a>`
  + 缩写
    + `<a v-bind:href="url">...</a>` 可以缩写为`<a :href="url">...</a>`
    + `<a v-on:click="doSomething">...</a>` 可以缩写为 `<a @click="doSomething">...</a>`

#### 计算属性

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-16/98702004.jpg)

  + 通过computed，声明一个reversed_message 计算属性。
  
  + 我们也可以使用之前的 methods中定义一个 reversed_message 函数来计算
  
  + 我们还可以使用watch 侦听属性
    
  三者的区别主要在于：
  1. computed方法是基于依赖进行缓存，而methods方法是没有缓存的。而方法每次都会触发计算。
  2. watch属性则会导致代码量相对繁琐且如果涉及多字段(FullName)计算时代码重复。
  
  所以可以使用computed的时候，尽量优先使用computed。  

 

#### 样式绑定

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-16/27930173.jpg)

  + `:class="{redClass: isActive}"` 当isActive为true，则class="redClass"
  + `:class="[activated]"` class为activated所绑定的数据值。

#### 条件渲染

  + `<span v-if="seen">hello</span>`
  + `<span v-show="seen">hello</span>` 
  
  v-if有更高的切换开销，当条件频繁切换时，v-show效率会更高一些。 
    
