---
title: "Vue实战：去哪儿App（一）"
date: 2018-08-17 14:35:42
categories:
- Vue
tags:
- Vue
---

### 基础环境配置

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

  + gitee代码管理环境搭建
  
  + 安装vue-cli脚手架
  
  ```
  C:\Users> vue --version
  2.9.6
  ```
  
  + 项目初始化
  
  `vue init webpack travel`
  
  + 启动项目
  
  `npm run dev`
  
  + 其他准备工作
    + reset.css
    + border.css
    + fastClick
    + [http://www.iconfont.cn](http://www.iconfont.cn) 注册并新建一个Travel项目。
        
#### 问题总结

  + 'error Expected linebreaks to be 'LF' but found 'CRLF' linebreak-style'
  
  es_lint语法检查报错，在在.eslintrc.json文件中的 rules下面添加或者修改`"linebreak-style":0`
  
  + 目前我用的IDE是sublime text3， vue文件没有代码高亮。
  
  sublime text3添加vue代码高亮插件`Vue  Syntax Highlight`。
     