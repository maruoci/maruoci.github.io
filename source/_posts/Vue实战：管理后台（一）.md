---
title: "Vue实战：管理后台（一）"
date: 2018-08-23 11:35:42
categories:
- 前端
tags:
- Vue
---

> 完成项目初步搭建，登录页面的初步设计，前后端的交互和mock数据的获取

**本项目参照地址**  [http://www.cnblogs.com/fhen/p/6721930.html](http://www.cnblogs.com/fhen/p/6721930.html)

### 准备工作

#### 环境准备

  + NodeJS  
  
  + Git
  
  ```
  C:\Users> node -v
  v9.9.0
  C:\Users> npm -v
  6.1.0
  C:\Users> git --version
  git version 2.14.1.windows.1
  ```
  
  <!-- more -->

#### 项目准备

  + gitee代码管理环境搭建
  
  建立一个私有项目
    
  + 安装vue-cli脚手架
  
  ```
  C:\Users> vue --version
  2.9.6
  ```
  
  + 项目初始化
  
  `vue init webpack vue-admin`
  
  + 项目插件
  
  ```json
   {
      "axios": "^0.18.0",
      "axios-mock-adapter": "^1.15.0",
      "element-ui": "^2.4.6",
      "mockjs": "^1.0.1-beta3",
      "stylus": "^0.54.5",
      "stylus-loader": "^3.0.2",
      "vue": "^2.5.2",
      "vue-router": "^3.0.1",
      "vue-svg-icon": "^1.2.9"
  }
  ```
  
  + 配置插件  
  
  ```
  //1. 配置Element-ui
    //main.js 
  import 'element-ui/lib/theme-chalk/index.css';
  Vue.use(ElementUI);
  //2. 配置axios则在使用的组件中引入即可
  import axios from 'axios';
  //3. 配置stylus
  //<style type="text/css" lang="stylus" scoped>
  ```
    
  + 启动项目
  
  `npm run dev`
  
#### 编辑器准备  
  
  + VSCode
  
  [相关插件及说明](https://github.com/varHarrie/varharrie.github.io/issues/10)

### 登录页面

  + 进入首页, 路由在 router.beforeEach 时校验用户是否登录。登录则进入Home 主页，否则进入Login登录页。
  
  + 输入用户名与密码，登录，调用api.js中的requestLogin函数。访问`/api/user/login`地址进行登录。
  
    + 当开启Mock数据时，/mock/index.js会监听`/api/user/login`请求，并返回模拟数据。
    
    + 后端有服务时，/config/index.js会根据设置的代理`proxyTable`访问后端服务。
  
  + 根据返回响应码判断是否登录成功。成功则将token设置进入sessionStorage并进入主页。

#### Login.vue
  
  ```
  
  <template>
    <div class="login-container">
      <el-form class="login-form" autoComplete="on" ref="loginForm" label-position="left" :model="loginForm" :rules="loginRules">
        <div class="title-container">
          <h3 class="title">系统登录</h3>
        </div>
  
        <el-form-item prop="username">
          <el-input name="username" type="text" v-model="loginForm.username" autoComplete="on" placeholder="账号"/>
        </el-form-item>
  
        <el-form-item prop="password">
          <el-input name="password" v-model="loginForm.password" autoComplete="on"
                @keyup.enter.native="handleLogin" placeholder="密码" />
        </el-form-item>
  
        <el-button type="primary"  style="width:100%;margin-bottom:30px;" 
          :loading="loading" @click.native.prevent="handleLogin">登录
        </el-button>
  
      </el-form>
    </div>
  </template>
  
  <script type="text/javascript">
  import {requestLogin} from '../api/api'
  
  export default {
    name: 'Login',
    data () {
      return {
        loginForm: {
          username: 'admin',
          password: '123456'
        },
        loginRules: {
          username: [{required: true, trigger: 'blur'}],
          password: [{required: true, trigger: 'blur'}]
        },
        loading: false
      }
    },
    methods: {
      handleLogin () {
        this.$refs.loginForm.validate((valid) => {
          if (valid) {
            this.loading = true
            let loginParams = {username: this.loginForm.username, password: this.loginForm.password}
            requestLogin(loginParams).then(data => {
              console.log(data)
              this.loading = false
              let { msg, code, token } = data
              if (code === 200) {
                sessionStorage.setItem('access-token', token)
                this.$router.push({path: '/home'})
              } else {
                this.$message({
                  message: msg,
                  type: 'error'
                })
              }
            })
          } else {
            console.log('error submit!!')
            return false
          }
        })
      }
    }
  }
  </script>

  ```

#### 路由

  ```
  
  import Vue from 'vue'
  import Router from 'vue-router'
  import Home from '@/components/Home'
  
  // 懒加载方式，当路由被访问的时候才加载对应组件
  const Login = resolve => require(['@/components/Login'], resolve)
  
  Vue.use(Router)
  
  let router = new Router({
    routes: [
      {
        path: '/',
        name: 'home',
        component: Home
      }, {
        path: '/login',
        name: 'login',
        component: Login
      }, {
        path: '/home',
        name: '主页',
        component: Home
      }
    ]
  })
  // 访问之前，都检查下是否登录了
  router.beforeEach((to, from, next) => {
    if (to.path.startsWith('/login')) {
      window.sessionStorage.removeItem('access-token')
      next()
    } else {
      let token = window.sessionStorage.getItem('access-token')
      if (!token) {
        next({ path: '/login' })
      } else {
        next()
      }
    }
  })
  export default router
  
  ```    

#### api.js & Mock/index.js

  <pre><code>
  // api/api.js
  import axios from 'axios'
  export const requestLogin = params => {
    return axios.post('/api/user/login', params).then(res => res.data)
  }
  // Mock/index.js
  import axios from 'axios'
  import MockAdapter from 'axios-mock-adapter'
  import { LoginUsers } from './data/user'
  export default {
    init () {
      let mock = new MockAdapter(axios)
      // mock success request
      mock.onGet('/success').reply(200, {
        msg: 'success'
      })
      // mock error request
      mock.onGet('/error').reply(500, {
        msg: 'failure'
      })
      // 登录
      mock.onPost('/api/user/login').reply(arg => {
        let { username, password } = JSON.parse(arg.data)
        return new Promise((resolve, reject) => {
          let token = null
          let hasUser = LoginUsers.some(u => {
            if (u.username === username && u.password === password) {
              token = 'adminXXXXXX'
              return true
            }
          })
          if (hasUser) {
            resolve([200, { code: 200, msg: '请求成功', token: token }])
          } else {
            resolve([200, { code: 500, msg: '账号或密码错误' }])
          }
        })
      })
    }
  }
  // data
  const LoginUsers = [{
    id: 1,
    username: 'admin',
    password: '123',
    email: '123456789@qq.com',
    name: '程序员'
  }]
  
  export { LoginUsers }
  </code></pre>

#### 问题

  + 访问后端接口时，后端接口报403异常。
  
    第一检查package.json中 `webpack-dev-server --host 0.0.0.0` 是否是此类配置。
    
    第二需要后端配置跨域支持。具体可参考[spring cloud-前端跨域问题的解决方案](https://blog.csdn.net/liuchuanhong1/article/details/62237705)

### 效果图

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-24/70824179.jpg)
  
  下一节就准备做主页了.       

### 参考

+ [使用vue.js2.0和element-ui 搭建的一个后台管理界面](http://www.cnblogs.com/fhen/p/6721930.html)
+ [vue.js+elementUI学习01之后台管理登录验证实现axios和springMVC交互](https://blog.csdn.net/dream_broken/article/details/77451639)
+ [手摸手系列](https://juejin.im/post/59097cd7a22b9d0065fb61d2)     