---
title: CSS基础学习
date: 2018-12-03 17:38:30
categories:
- 前端
tags:
- Css
---

## 写在前面的话

 看着容易却总是设计不好一个普通的Html页面。这回趁现在好好的学习一下。不苛求达到前端牛人的地步，只想着想做个什么效果的时候可以不求人。

## 参考

 [RUNOOB.com](http://www.runoob.com/css) 

<!-- more -->

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

### CSS 字体

   | 属性     | 描述    |
   |:---------|:-------------|
   |font            | 字体所有属性的声明     |
   |font-family     | 指定文本的字体系列     |
   |font-size       | 指定字体大小          |
   |font-style      | 指定文本的字体样式     |
   |font-variant    | 以小型大写字体或正常显示|
   |font-weight     | 指定字体粗细          |
   
   **font**
   
   ```css
    <!--font-style font-variant font-weight font-size/line-height font-family -->
    p{
        font: italic bold 12px/30px Georgia, Serif;
    }
   ```
   
   **font-family**
   
   ```css
    p{
        font-family:"Times New Roman",Georgia,Serif;
    }
   ```
   
   **font-style**
   
   | 值  | 描述|
   |:--------|:--------|
   |normal   | 默认值，标准的字体样式|
   |italic   | 斜体    |
   |oblique  | 斜体    |
   |inherit  | 继承父元素的样式     |
   
   **font-variant**
   
   | 值  | 描述|
   |:--------|:--------|
   |normal   | 默认值，标准的字体|
   |small-caps| 浏览器会显示小型大写字母的字体。   |
   |inherit  | 继承父元素的样式     |
   
   **font-weight**
   
   | 值  | 描述|
   |:--------|:--------|
   |normal   | 默认值，标准的字体|
   |bold     | 粗体       |
   |bolder   | 更粗字体   |
   |100-900  | 由细到粗的字体，400=normal，700=bold   |
   |inherit  | 继承父元素的样式     |

### CSS 表格   
   
   ```css
     <!-- border: border-width border-style border-color -->
     table {border-collapse: collapse}
     td {border: 1px solid black}
   ```
   
   border 定义表格的边框属性
   
   border-style: 边框样式，也可以使用border-top-style、border-right-style分别设置四边的边框。
   
   | 值      | 描述    |
   |:--------|:-------|
   |none     |无边框   |
   |solid    |实线边框 |
   |dotted   |点线边框 |
   |dashed   |虚线边框 |
   |...      |其他..  |
   
   border-collapse 定义边框是否合并，一般都使用collapse进行合并边框。默认separate边框会分开。
   
   border-radius 设置边框圆角，也可以使用border-top-left-radius等设置4边的圆角。
   
   ```css
     div.b{
      border: 1px solid red;
      border-top-right-radius: 20px;
      border-bottom-right-radius: 20px;
    }
   ```

### CSS 边距

  **margin外边距**
  
  定义元素周围的空间。可以使用margin-top、margin-left等分别定义四边的外边距。
  
  ```css
    div {margin: 40px;}                  <!-- 4个边距都是40px -->
    div {margin: 40px 20px;}             <!-- 上 40 右 20  下 同上 左 同 右 -->
    div {margin: 40px 20px 30px;}        <!-- 上 40 右 20  下 30  左 同 右-->
    div {margin: 40px 20px 10px 50px;}   <!-- 上 右 下 左-->
  ```
  
  **padding填充**   
  
  padding和margin的设置方式相似，区别在于margin代表元素间的边距，padding则是元素与内容的填充距离。
  
  ![2018-12-12-01.png](https://i.loli.net/2018/12/12/5c1105a86e182.png)


### CSS 分组与嵌套

  不同的元素具有相同的样式，此时可以使用分组选择器。
  
  ```css
    h1,h2,p{color:green;}
  ```
  
  嵌套则是为选择器内部元素定义的。
  
  ```css
    p.test{color:green;}    <!--为class='test'的p元素指定样式-->
    .test p{color:green;}
  ```

### CSS 定位

  