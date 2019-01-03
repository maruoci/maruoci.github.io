---
title: Bootstrap基础
date: 2018-12-03 17:38:30
categories:
- 前端
tags:
- Css
---

## 写在前面的话

 看着容易却总是设计不好一个普通的Html页面。这回趁现在好好的学习一下。不苛求达到前端牛人的地步，只想着想做个什么效果的时候可以不求人。

## CSS属性

### background

  定义元素的背景      
 
| 属性               | 描述          |
|:------------------|:--------------|
|background-color   | 背景色         |
|background-image   | 背景图片       |
|background-size    | 背景图片大小       |
|background-position| 背景图像的起始位置|
|background-repeat  | 背景图像是否重复，如何重复|
|background-attchment| 背景图像是否固定或随页面其余部分滚动|
|background         | 简写属性，将background属性设置在一个声明中|
  
  示例
  
  ```css
    body { background-color: aliceblue;}
    p {background-image: url("demo.png");}
    
    #body {
        background-position: top center;  
        background-position: 20% 30%;
        background-position: 20px 150px;
        background-position: 20px 50%;
    }
    
    body {
        background-repeat: repeat;   <!-- 默认值，水平和垂直重复 -->
        background-repeat: repeat-x; 
        background-repeat: repeat-y;
        background-repeat: no-repeat; 
        background-repeat: inherit; <!-- 属性设置从父元素继承 -->
    }
    
    body{
        background-attachment: scroll; <!-- 默认值，随页面滚动而动-->
        background-attachment: inherit; <!-- 属性设置从父元素继承 -->
        background-attachment: fixed; <!-- 固定 -->
    }
    
    html {
       background: cadetblue url("demo.png") no-repeat scroll top center;
       background-size: 500px 300px; <!--设置固定宽高 -->
       background-size: 60% 39%;  <!--按元素区域百分比来设置-->
    }
  ```
 
 ### CSS 文本
 
  | 属性          | 描述                        |  可选值              |
  |:----------------|:---------------------------|:-----------------------------|
  |color            |设置文本颜色                 |#ff0000 , rgb(0,0,0) ,red,inherit      |
  |direction        |设置文本方向                 |ltr, rtl, inherit                      |
  |letter-spacing   |设置字符间距                 |normal,(-1px,2px..), inherit           |
  |line-height      |设置字符行高                 |normal,(0.5/1/2..), 2px, 40%, inherit  | 
  |text-align       |对齐元素文本                 |center,left,right,justify,inherit      |
  |text-decoration  |向文本添加修饰               |none,underline,overline,line-through,blink,inherit |
  |text-indent      |缩进元素中文本的首行          |10px, 10%, inherit                     |
  |text-shadow      |设置文本阴影                 |text-shadow: h-shadow v-shadow blur color;|
  |text-transform   |控制元素中的字母              |none, uppercase, lowercase,capitalize,inherit|
  |unicode-bidi     |设置或返回文本是否被重写       |normal, embed, bidi-override,initial   |
  |vertical-align   |设置元素的垂直对齐            |baseline, sub, super, top, text-top, middle, bottom, text-bottom, 20%, inherit|
  |white-space      |设置元素中空白的处理方式       |normal, pre, nowrap, pre-wrap, pre-line, inherit|
  |word-spacing     |设置字间距                   |normal, 20px, inherit|
  
  示例
  
  ```html
    <html>
    <style>
    div.ex1
    {
        direction:rtl;
        unicode-bidi:bidi-override;
    }
    </style>
      <body>
        <div>一些文本。默认的书写方向。</div>
        <div class="ex1">一些文本 。从右向左的方向。</div>
      </body>
    </html>
  ```
  
  ![](https://i.loli.net/2018/12/11/5c0f7010b0272.jpg)
  
  ```
    <html>
    <head>
    <style>
        h1
        {
            color:white;
            text-shadow:2px 2px 4px #000000;
        }
    </style>
    </head>
    <body>
        <h1>Text-shadow on white text</h1>
    </body>
    </html>
  ```
  
  ![](https://i.loli.net/2018/12/11/5c0f729b0b182.png)