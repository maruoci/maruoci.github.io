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
  
####  

