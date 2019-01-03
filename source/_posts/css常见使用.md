---
title: CSS常见使用问题汇总
date: 2018-12-17 17:38:30
categories:
- 前端
tags:
- Css
---

**Span 设置width无效**

  Span标签属于内联元素。
  
  Html元素分为块(block)元素和内联(inline)元素。
  
  block块元素：总是令起一行开始，宽(width)、高(height)和顶/底边距都可控制。宽度缺省是所在容器的100%。
  
  常见的block元素有：div、form、h1-h6、hr、ol、p、table、ul
  
  inline元素：和其他元素在一行，高度、行高和顶/底边距不可改变，宽度为元素内文字或图片宽度，不可改变。
  
  常见的inline元素有：a、br、font、span、img、input、label、select、textarea
  
  我们可以用过display属性摆脱Html标签归类的限制。display可选值有：none、block、inline、inherit。
  

**Div父元素的高度自适应子元素**

  参考地址: [div父元素根据子元素高度自适应高度](https://blog.csdn.net/a5534789/article/details/51233522)
  
  目前子元素因为添加了float属性，float的元素浮动脱离了正常的文档流，所以父元素没有自适应高度。
  
  可以通过overflow:hidden/auto或者在子元素最后添加一个兄弟节点如`<div style="clear:both"/>`或<br clear="all"/>`来处理。