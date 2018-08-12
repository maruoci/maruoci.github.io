---
title: Vue入门(一)：安装与Hello World
date: 2018-08-12 10:36:29
categories:
- Vue
tags:
- Vue
---
### 简介

  + Vue是什么
  
  + 它的优势
  
  <!-- more -->
  
### 安装

  打开官方网站的[文档](https://cn.vuejs.org/v2/guide/),下载Vue的开发版本Vue.js文件。

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-12/97571434.jpg)

  这样我们在开发的时候，直接使用script标签引入该js文件即可。

### Hello World

  第一个Hello World程序

  ```html
  <body>
     <div id="app"> {{ content }}</div>
     <!-- 引入Vue.js文件- -->
     <script src="vue.js"></script>
     <script>
        // 通过new Vue 创建一个Vue实例
        var app = new Vue({
          el: '#app',    // 将该实例绑定到id='app'的dom元素
          data: {
            content: 'hello world2'
          }
        })
        setTimeout(function(){
          app.$data.content = 'bye world'
        }, 2000)
     </script>
  </body>

  ```
  
### TodoList

  TodoList是什么？
  
  ```
  <body>
  <div id="app">
    <input type="text" v-model="inputValue"/>
    <button v-on:click="handleBtnClick">提交</button>
    <ul>
      <li v-for="item in list">{{item}}</li>
    </ul>
  </div>
  <script src="vue.js"></script>
  <script>
    var app = new Vue({
      el: '#app',
      data:{
        list: [],
        inputValue:''
      },
      methods:{
        handleBtnClick:function () {
          this.list.push(this.inputValue);
          this.inputValue = '';
        }
      }
    })
  </script>
  </body>
  ```
  
  知识点：
  
  + v-model : 数据的一个双向绑定。当input的文本发生变化，inputValue同时变化。反之亦然。
  
  + v-on:click: 事件绑定方法。当button触发click事件时，handleBtnClick方法执行。

### 参考资料

  + [Vue官方文档](https://cn.vuejs.org/v2/guide/)
