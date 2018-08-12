---
title: "Vue入门 (二): Component组件"
date: 2018-08-12 14:22:42
categories:
- Vue
tags:
- Vue
---

### 简介

#### 组件的组织

  ![](https://cn.vuejs.org/images/components.png)
  
  图例为官方文档的图例。如图示，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。
  
  <!-- more -->
  
### 组件示例

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-12/17055914.jpg)

#### 全局注册组件

  `Vue.component` 
  
  组件命名有两种方式kebab-case(短横线分割) 与 Pascal-case(驼峰)。
  
  `Vue.component('TodoItem')` 可以同时使用`<todo-item>` 和 `<TodoItem>`两种方式引入。
  
  ```html
  <body>
  <div id="app">
    <input type="text" v-model="inputValue"/>
    <button @click="handleBtnClick">提交</button>
    <ul>
      <todo-item v-for="item in list" v-bind:content="item"></todo-item>
    </ul>
  </div>
  <script src="vue.js"></script>
  <script>
    Vue.component('TodoItem', {
      props: ['content'],
      template: '<li>{{content}}</li>'
    })
  
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
  
  + Vue.component('TodoItem',{}) 创建一个名为TodoItem的组件，页面可以使用`<todo-item>`引入。
  
  + `{props: ['content']`} 接收一个页面通过v-bind:content绑定的一个属性。示例绑定的item是v-for中的元素。
  
  + `template` 是组件的html内容。
  
  + @click 是 上节的 v-on:click 方法的缩写。

#### 局部注册组件

  `new Vue({'components':{'TodoItem': TodoItem}})` 注册
  
  ```html
  <body>
  <div id="app">
    <input type="text" v-model="inputValue"/>
    <button @click="handleBtnClick">提交</button>
    <ul>
      <!--<li v-for="item in list">{{item}}</li>-->
      <todo-item v-for="item in list" v-bind:content="item"></todo-item>
    </ul>
  </div>
  <script src="vue.js"></script>
  <script>
    var TodoItem = {
      props: ['content'],
      template: '<li>{{content}}</li>'
    }
  
    var app = new Vue({
      el: '#app',
      components:{
        'TodoItem': TodoItem
      },
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

### 组件间传参

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-12/12824632.jpg)
  
  预期效果是点击列表中数据，逐一删除。
  
  + `:index="index"` 父组件向子组件传递一个列表索引。索引从`(intex, index) in list`获取。`:index`等同于之前的`v-bind:index`
  
  + 子组件内容绑定click事件。`@click="handleItemClick`
  
  + 子组件定义handleItemClick函数。`this.$emit("delete", this.index)`通知父组件监听delete事件，并传递index。
  
  + 父组件监听delete事件，并根据传递索引删除元素`this.list.splice(index, 1);`。
  
  ```html
  <body>
  <div id="app">
    <input type="text" v-model="inputValue"/>
    <button @click="handleBtnClick">提交</button>
    <ul>
      <todo-item v-for="(item,index) in list"
                 :content="item"
                 :index="index"
                 @delete="doHandleDelete">
      </todo-item>
    </ul>
  </div>
  <script src="vue.js"></script>
  <script>
    var TodoItem = {
      props: ['content','index'],
      template: '<li @click="handleItemClick">{{content}}</li>',
      methods:{
        handleItemClick: function () {
          this.$emit("delete", this.index);
        }
      }
    }
  
    var app = new Vue({
      el: '#app',
      components:{
        'TodoItem': TodoItem
      },
      data:{
        list: [],
        inputValue:''
      },
      methods:{
        handleBtnClick:function () {
          this.list.push(this.inputValue);
          this.inputValue = '';
        },
        doHandleDelete: function (index) {
          this.list.splice(index, 1);
        }
      }
    })
  </script>
  </body>
  ```

### 小结

  + 组件的调用分为全局组件与局部组件。
    
    ```html
    <!-- 全局组件调用方式 -->
    <todo-item v-for="item in list" v-bind:content="item"></todo-item>
    Vue.component('TodoItem', {
          props: ['content'],
          template: '<li>{{content}}</li>'
        })
    <!-- 局部组件调用 -->
    var TodoItem = {
          props: ['content','index'],
          template: '<li @click="handleItemClick">{{content}}</li>'
        }
    var app = new Vue({
          el: '#app',
          components:{
            'TodoItem': TodoItem
          }
      })
    ```
  
  + 组件间的传参：
  
    + 父组件向子组件传参：父组件使用 `v-bind:content`或`:content` 传递参数，子组件使用 `props` 接收参数。
    
    + 子组件向父组件传参:  `this.$emit("delete", this.index);`
    
  + 其他知识点：
    
    + `this.$emit()` 方法的使用。
    
    + `this.list.splice(index, 1)` 方法的使用。
    
    + `@click` 等同于 `v-on:click`
    
    + `:content` 等同于 `v-bind:content`
  
  
### 参考文档

 + [Vue官方文档](https://cn.vuejs.org/v2/guide/)
 
 + [Vue2.5开发去哪儿网App 从零基础入门到实战项目](https://coding.imooc.com/learn/list/203.html)